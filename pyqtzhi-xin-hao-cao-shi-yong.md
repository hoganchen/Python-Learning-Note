### PyQt入门之信号槽使用

```
import sys
from PyQt4.QtCore import SIGNAL, QThread, pyqtSignal
from PyQt4.QtGui import *
from struct import *
import random
import Queue
import time

##########################################################################################
##@brief Swap a 32-bits value
##########################################################################################
def BSWAP32(val):
    val = ((val >> 24) & 0xFF) + (((val >> 16) & 0xFF) << 8) + (((val >> 8) & 0xFF) << 16) + (((val >> 0) & 0xFF) << 24)

    return val


##########################################################################################
##@brief Get bytes in text format for received string for multibyte fields.
##
## Bytes separated by ':'
##########################################################################################
def B(txt):
    bytes = unpack('B' * len(txt), txt)
    out = ''
    for b in bytes:
        out += ('%02X' % b) + ':'
    out = out[:(len(out) - 1)]

    return out


##########################################################################################
##@brief Get bytes in text format for received string for multibyte fields.
##
## Bytes concatenate in big endian format MSB->LSB
##########################################################################################
def B2(txt):
    bytes = unpack('B' * len(txt), txt)
    out = ''
    for b in bytes:
        out = ('%02X' % b) + out

    return out


##########################################################################################
##@brief Get bytes in text format for received string for multibyte fields,
##       inverting the bytes in original array
##
## Bytes concatenate in big endian format MSB->LSB
##########################################################################################
def B2INV(txt):
    bytes = unpack('B' * len(txt), txt)
    out = ''
    for b in bytes:
        out = out + ('%02X' % b)

    return out


##########################################################################################
##@brief Get user input text to packed format
##########################################################################################
def S2B(txt):
    ret = ''
    if txt != '':
        # cut text after '0X' - make sure it is in upper case
        txt_nb = txt.upper().split('0X')[1]

        for i in range(len(txt_nb) / 2):
            ret += pack('B', int('0x' + txt_nb[i * 2] + txt_nb[i * 2 + 1], 16))

    return ret


##########################################################################################
##@brief Print out in GUI in 0xXXXXXX... format
##########################################################################################
def B2S(txt):
    bytes = unpack('B' * len(txt), txt)
    out = '0x'
    for b in bytes: out += ('%02X' % b)

    return out


##########################################################################################
##@brief Print out in GUI in 0xXXXXXX... format, inverting the bytes in original array
##########################################################################################
def B2SINV(txt):
    if (type(txt) is int):
        out = '0x%X' % (txt)
    else:
        b = unpack('B' * len(txt), txt)
        out = '0x'
        for i in range(len(b)): out += ('%02X' % b[len(b) - 1 - i])

    return out


##########################################################################################
##@brief Print out in XX:XX:XX:... format, inverting the bytes in original array
##########################################################################################
def B2SINV2(txt):
    b = unpack('B' * len(txt), txt)
    out = ('%02X' % b[len(b) - 1])
    for i in range(len(b) - 1):
        out += (':%02X' % b[len(b) - 2 - i])

    return out


##########################################################################################
##@brief Invert bytes in packed string.
##########################################################################################
def INVB(txt):
    txt_inv = ''
    for i in range(len(txt)):
        txt_inv += txt[len(txt) - 1 - i]
    return txt_inv


class AppendTextBrowser(QThread):
    postSignal = pyqtSignal(str)

    def __init__(self, parent=None):
        super(AppendTextBrowser, self).__init__(parent)
        # self.handle = handle
        self.is_running = True

    def run(self):
        while self.is_running:
            byte_len = 371
            lrx = ''

            for index in range(byte_len):
                lrx += pack('B', random.randint(0, 255))
            # self.handle.append('Raw Data: {}'.format(lrx))

            struct_value = B2(lrx)
            self.postSignal.emit(struct_value)

            # self.is_running -= 1

            time.sleep(0.01)

    def stop(self):
        self.is_running = False
        # self.terminate()

#########################################################################################
# @brief get the statistics from global variable address
#########################################################################################
class tab_sp_statistics(QWidget):
    #########################################################################################
    # @brief Class constructor.
    # @param parent Parent window, is allowed to be None.
    #########################################################################################
    def __init__(self, dev, peer, parent=None):

        # ---------------------------------------------------------------------------------
        # INIT-----------------------------------------------------------------------------
        # ---------------------------------------------------------------------------------
        # call the constructor of the inherited class and use the parent
        super(tab_sp_statistics, self).__init__(parent)

        # keep parent class reference in case it needs updating from here
        self.parent = parent

        # device to which the dialog instance is associated
        self.dev = dev
        self.peer = peer

        self.bw_thread = None

        # create font for labels
        font = QFont()
        font.setWeight(75)
        font.setBold(True)

        # ---------------------------------------------------------------------------------
        # START LAYOUT --------------------------------------------------------------------
        # ---------------------------------------------------------------------------------
        # vertical connection tab layout
        tab_layv = QGridLayout()

        # statistics output label
        lbl_rwip_info_addr = QLabel('Address: ', self)
        lbl_rwip_info_addr.setFont(font)
        tab_layv.addWidget(lbl_rwip_info_addr, 0, 0)

        self.edt_rwip_info_addr = QLineEdit('0x3000154c', self)  # Add local dev variable value
        tab_layv.addWidget(self.edt_rwip_info_addr, 0, 1)

        # statistics length label
        lbl_rwip_info_len = QLabel('Length: ', self)
        lbl_rwip_info_len.setFont(font)
        tab_layv.addWidget(lbl_rwip_info_len, 0, 2)

        self.edt_rwip_info_len = QLineEdit('0', self)  # Add local dev variable value
        self.edt_rwip_info_len.setReadOnly(True)
        tab_layv.addWidget(self.edt_rwip_info_len, 0, 3)

        # statistics output label
        lbl_statistics = QLabel('Statistics Output: ', self)
        lbl_statistics.setFont(font)
        tab_layv.addWidget(lbl_statistics, 1, 0)

        # statistics output text browser
        # self.tb_statistics_output = QPlainTextEdit()
        # self.tb_statistics_output.setMaximumBlockCount(100)
        # tab_layv.addWidget(self.tb_statistics_output, 1, 1, 1, 80)

        self.doc = QTextDocument()
        # self.doc.setDefaultFont(font)
        self.doc.setMaximumBlockCount(100)

        self.tb_statistics_output = QTextBrowser(self)
        self.tb_statistics_output.setDocument(self.doc)
        tab_layv.addWidget(self.tb_statistics_output, 1, 1, 1, 80)

        # get button
        self.qb_get_statistics_output = QPushButton('Con_Stat', self)
        self.connect(self.qb_get_statistics_output, SIGNAL('clicked()'), self.get_statistics_output)
        tab_layv.addWidget(self.qb_get_statistics_output, 2, 0)

        # clear button
        self.qb_clear_statistics_output = QPushButton('Clear', self)
        self.connect(self.qb_clear_statistics_output, SIGNAL('clicked()'), self.clear_statistics_output)
        tab_layv.addWidget(self.qb_clear_statistics_output, 2, 1)

        # ---------------------------------------------------------------------------------
        # SET LAYOUT ----------------------------------------------------------------------
        # ---------------------------------------------------------------------------------
        self.setLayout(tab_layv)

    def get_statistics_output(self):
        con_handle_num = 12
        statistics_t_size = 7 * 4
        magic1_size = 2
        act_state_size = 1
        magic2_size = 2
        adv_rx_err_cnt_size = 2
        adv_rx_normal_cnt_size = 2
        adv_rx_con_req_cnt_size = 2
        adv_rx_scn_rsp_cnt_size = 2
        adv_rx_other_cnt_size = 2
        scn_rx_total_cnt_size = 2
        scn_rx_err_cnt_size = 2
        scn_adv_data_len_err_cnt_size = 2
        magic3_size = 2
        sniffer_ccm_pkt_cnt_req_musk_size = 1

        '''
struct dbg_con_pkt_statistics_t
{
    /// Counts of RX Times
    uint32_t rx_total_cnt;
    /// Counts of RX SYNC Error Times
    uint32_t rx_sync_err_cnt;
    /// Counts of RX CRC Error Times
    uint32_t rx_crc_err_cnt;
    uint32_t rx_other_err_cnt;
    uint32_t rx_sn_err_cnt;
    uint32_t rx_mic_err_cnt;
    uint32_t rx_normal_cnt;
} __attribute__((packed, aligned(1)));

struct dbg_rwip_info_t
{
    struct dbg_con_pkt_statistics_t con_pkt_stat[BLE_ACTIVITY_MAX];
    uint16_t magic1;
    uint8_t act_state[BLE_ACTIVITY_MAX];  // this is valid for adv&scan act only, currently
    uint16_t magic2;
    uint16_t adv_rx_err_cnt;
    uint16_t adv_rx_normal_cnt;
    uint16_t adv_rx_con_req_cnt;
    uint16_t adv_rx_scn_rsp_cnt;
    uint16_t adv_rx_other_cnt;
    uint16_t scn_rx_total_cnt;
    uint16_t scn_rx_err_cnt;
    uint16_t scn_adv_data_len_err_cnt;
    uint16_t magic3;
    uint8_t  sniffer_ccm_pkt_cnt_req_musk;
} __attribute__((packed, aligned(1)));
// #pragma pack(pop)
        '''

        byte_len = con_handle_num * statistics_t_size + magic1_size + con_handle_num * act_state_size + magic2_size + \
                   adv_rx_err_cnt_size + adv_rx_normal_cnt_size + adv_rx_con_req_cnt_size + adv_rx_scn_rsp_cnt_size + \
                   adv_rx_other_cnt_size + scn_rx_total_cnt_size + scn_rx_err_cnt_size + \
                   scn_adv_data_len_err_cnt_size + magic3_size + sniffer_ccm_pkt_cnt_req_musk_size

        self.edt_rwip_info_len.setText(hex(byte_len))

        # lrx = self.dev.hci.h2c_dbg_rd_mem_cmd(int(self.edt_rwip_info_addr.text()), 32, byte_len)
        # self.dev.check_par(lrx, [EVT, evt_code.H_EVT_CC, 'NOCHK', op_code.H_CMD_DBG_RD_MEM, err_code.H_ERR_SUCCESS,
        #                          byte_len])

        lrx = ''
        for index in range(byte_len):
            lrx += pack('B', random.randint(0, 255))
            # self.tb_statistics_output.append('Raw Data: {}'.format(B2(lrx)))
            # self.tb_statistics_output.append('')

        # struct_value = lrx[6:]
        struct_value = B2(lrx[:])
        struct_value_index = 0

        self.tb_statistics_output.append('Raw Data: {}'.format(struct_value))
        self.tb_statistics_output.append('')

        self.tb_statistics_output.append('Address: {}'.format(self.edt_rwip_info_addr.text()))
        self.tb_statistics_output.append('')

        # self.tb_statistics_output.append('First 4 bytes Data: {}'.format(struct_value[:4 * 2]))
        # self.tb_statistics_output.append('')

        for handle_index in range(con_handle_num):
            con_pkt_stat_info = struct_value[handle_index * statistics_t_size * 2:
                                             (handle_index + 1) * statistics_t_size * 2]
            rx_total_cnt = '0x{}'.format(con_pkt_stat_info[0:8])
            rx_sync_err_cnt = '0x{}'.format(con_pkt_stat_info[8:16])
            rx_crc_err_cnt = '0x{}'.format(con_pkt_stat_info[16:24])
            rx_other_err_cnt = '0x{}'.format(con_pkt_stat_info[24:32])
            rx_sn_err_cnt = '0x{}'.format(con_pkt_stat_info[32:40])
            rx_mic_err_cnt = '0x{}'.format(con_pkt_stat_info[40:48])
            rx_normal_cnt = '0x{}'.format(con_pkt_stat_info[48:56])

            handle_stat_output = 'con_handle{}: rx_total_cnt: {}, rx_sync_err_cnt: {}, rx_crc_err_cnt: {}, ' \
                                 'rx_other_err_cnt: {}, rx_sn_err_cnt: {}, rx_mic_err_cnt: {}, rx_normal_cnt: {}'.\
                format(handle_index, rx_total_cnt, rx_sync_err_cnt, rx_crc_err_cnt, rx_other_err_cnt, rx_sn_err_cnt,
                       rx_mic_err_cnt, rx_normal_cnt)
            self.tb_statistics_output.append(handle_stat_output)

        self.tb_statistics_output.append('')
        struct_value_index += con_handle_num * statistics_t_size * 2

        magic1 = struct_value[struct_value_index:(struct_value_index + magic1_size * 2)]
        struct_value_index += magic1_size * 2

        for handle_index in range(con_handle_num):
            act_state_info = struct_value[(struct_value_index + handle_index * act_state_size * 2):
                                          ((handle_index + 1) * act_state_size * 2 + struct_value_index)]

            act_state_output = 'act_state{}: {}'.format(handle_index, '0x{}'.format(act_state_info))
            self.tb_statistics_output.append(act_state_output)

        self.tb_statistics_output.append('')
        struct_value_index += con_handle_num * act_state_size * 2

        magic2 = struct_value[struct_value_index:(struct_value_index + magic2_size * 2)]
        struct_value_index += magic2_size * 2

        adv_rx_err_cnt = struct_value[struct_value_index:(struct_value_index + adv_rx_err_cnt_size * 2)]
        struct_value_index += adv_rx_err_cnt_size * 2

        adv_rx_normal_cnt = struct_value[struct_value_index:(struct_value_index + adv_rx_normal_cnt_size * 2)]
        struct_value_index += adv_rx_normal_cnt_size * 2

        adv_rx_con_req_cnt = struct_value[struct_value_index:(struct_value_index + adv_rx_con_req_cnt_size * 2)]
        struct_value_index += adv_rx_con_req_cnt_size * 2

        adv_rx_scn_rsp_cnt = struct_value[struct_value_index:(struct_value_index + adv_rx_scn_rsp_cnt_size * 2)]
        struct_value_index += adv_rx_scn_rsp_cnt_size * 2

        adv_rx_other_cnt = struct_value[struct_value_index:(struct_value_index + adv_rx_other_cnt_size * 2)]
        struct_value_index += adv_rx_other_cnt_size * 2

        scn_rx_total_cnt = struct_value[struct_value_index:(struct_value_index + scn_rx_total_cnt_size * 2)]
        struct_value_index += scn_rx_total_cnt_size * 2

        scn_rx_err_cnt = struct_value[struct_value_index:(struct_value_index + scn_rx_err_cnt_size * 2)]
        struct_value_index += scn_rx_err_cnt_size * 2

        scn_adv_data_len_err_cnt = struct_value[struct_value_index:(struct_value_index +
                                                                    scn_adv_data_len_err_cnt_size * 2)]
        struct_value_index += scn_adv_data_len_err_cnt_size * 2

        magic3 = struct_value[struct_value_index:(struct_value_index + magic3_size * 2)]
        struct_value_index += magic3_size * 2

        sniffer_ccm_pkt_cnt_req_musk = struct_value[struct_value_index:(struct_value_index +
                                                                        sniffer_ccm_pkt_cnt_req_musk_size * 2)]
        struct_value_index += sniffer_ccm_pkt_cnt_req_musk_size * 2

        self.tb_statistics_output.append('magic1: 0x{}'.format(magic1))
        self.tb_statistics_output.append('magic2: 0x{}'.format(magic2))
        self.tb_statistics_output.append('magic3: 0x{}'.format(magic3))
        self.tb_statistics_output.append('')

        self.tb_statistics_output.append('adv_rx_err_cnt: 0x{}'.format(adv_rx_err_cnt))
        self.tb_statistics_output.append('adv_rx_normal_cnt: 0x{}'.format(adv_rx_normal_cnt))
        self.tb_statistics_output.append('adv_rx_con_req_cnt: 0x{}'.format(adv_rx_con_req_cnt))
        self.tb_statistics_output.append('adv_rx_scn_rsp_cnt: 0x{}'.format(adv_rx_scn_rsp_cnt))
        self.tb_statistics_output.append('adv_rx_other_cnt: 0x{}'.format(adv_rx_other_cnt))
        self.tb_statistics_output.append('')

        self.tb_statistics_output.append('scn_rx_total_cnt: 0x{}'.format(scn_rx_total_cnt))
        self.tb_statistics_output.append('scn_rx_err_cnt: 0x{}'.format(scn_rx_err_cnt))
        self.tb_statistics_output.append('scn_adv_data_len_err_cnt: 0x{}'.format(scn_adv_data_len_err_cnt))
        self.tb_statistics_output.append('')

        self.tb_statistics_output.append('sniffer_ccm_pkt_cnt_req_musk: 0x{}'.format(sniffer_ccm_pkt_cnt_req_musk))

        # self.tb_statistics_output.append('Raw Data: {}'.format(struct_value))
        self.bw_thread = AppendTextBrowser()
        self.bw_thread.start()
        self.bw_thread.postSignal.connect(self.update_statistics_data_output)

    def update_statistics_output(self):
        self.bw_thread = AppendTextBrowser()
        self.bw_thread.start()
        self.bw_thread.postSignal.connect(self.update_statistics_data_output)

    def update_statistics_data_output(self, lrx):
        self.tb_statistics_output.append('Raw Data: {}'.format(lrx))

    def clear_statistics_output(self):
        if self.bw_thread:
            self.bw_thread.stop()
        self.tb_statistics_output.clear()


def main():
    app = QApplication(sys.argv)
    ex = tab_sp_statistics('a', 'b')
    ex.show()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```



