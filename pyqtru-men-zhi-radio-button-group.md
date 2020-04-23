### PyQt入门之Radio Button Group

```
import sys
from PyQt4.QtCore import *
from PyQt4.QtGui import *

class Radiodemo(QWidget):
    def __init__(self, parent = None):
        super(Radiodemo, self).__init__(parent)

        """
        owntype_layh = QHBoxLayout()
        self.b1 = QRadioButton("Button1")
        self.b1.setChecked(True)
        self.b1.toggled.connect(lambda:self.btnstate(self.b1))
        owntype_layh.addWidget(self.b1)

        self.b2 = QRadioButton("Button2")
        self.b2.toggled.connect(lambda:self.btnstate(self.b2))

        owntype_layh.addWidget(self.b2)
        self.setLayout(owntype_layh)
        self.setWindowTitle("RadioButton demo")
        """

        A_PUB = 0x0
        A_RAND = 0x1

        self.own_addr_type = A_PUB
        self.direct_addr_type = A_RAND

        font = QFont()
        font.setWeight(75)
        font.setBold(True)

        if True:
            if False:
                tab_layv = QVBoxLayout()

                grid_dir = QGridLayout()

                # Own address type
                lbl_ownaddr = QLabel('Own BD Address Type', self)
                lbl_ownaddr.setFont(font)
                grid_dir.addWidget(lbl_ownaddr, 0,0)

                owntype_layh = QHBoxLayout()
                #bd addr type radio buttons
                self.rb_ownpub = QRadioButton('Public')
                # self.rb_ownpub.setChecked(self.dev.own_addr_type == A_PUB)
                self.rb_ownpub.setChecked(self.own_addr_type == A_PUB)
                self.rb_ownpub.toggled.connect(lambda:self.btnstate(self.rb_ownpub))
                owntype_layh.addWidget(self.rb_ownpub)

                self.rb_ownrand = QRadioButton('Random')
                # self.rb_ownrand.setChecked(self.dev.own_addr_type == A_RAND)
                self.rb_ownrand.setChecked(self.own_addr_type == A_RAND)
                self.rb_ownrand.toggled.connect(lambda:self.btnstate(self.rb_ownrand))
                owntype_layh.addWidget(self.rb_ownrand)

                self.rb_ownrsvp = QRadioButton('Resolvable Public')
                # self.rb_ownrsvp.setChecked(self.dev.own_addr_type == 0x02)
                self.rb_ownrsvp.setChecked(self.own_addr_type == 0x02)
                self.rb_ownrsvp.toggled.connect(lambda:self.btnstate(self.rb_ownrsvp))
                owntype_layh.addWidget(self.rb_ownrsvp)

                self.rb_ownrsvr = QRadioButton('Resolvable Random')
                # self.rb_ownrsvr.setChecked(self.dev.own_addr_type == 0x03)
                self.rb_ownrsvr.setChecked(self.own_addr_type == 0x03)
                self.rb_ownrsvr.toggled.connect(lambda:self.btnstate(self.rb_ownrsvr))
                owntype_layh.addWidget(self.rb_ownrsvr)

                if False:
                    # own address type check
                    if self.own_addr_type == A_PUB:
                        self.rb_ownpub.setChecked(True)
                        self.rb_ownrand.setChecked(False)
                        self.rb_ownrsvp.setChecked(False)
                        self.rb_ownrsvr.setChecked(False)
                    elif self.own_addr_type == A_RAND:
                        self.rb_ownpub.setChecked(False)
                        self.rb_ownrand.setChecked(True)
                        self.rb_ownrsvp.setChecked(False)
                        self.rb_ownrsvr.setChecked(False)
                    elif self.own_addr_type == 0x02:
                        self.rb_ownpub.setChecked(False)
                        self.rb_ownrand.setChecked(False)
                        self.rb_ownrsvp.setChecked(True)
                        self.rb_ownrsvr.setChecked(False)
                    elif self.own_addr_type == 0x03:
                        self.rb_ownpub.setChecked(False)
                        self.rb_ownrand.setChecked(False)
                        self.rb_ownrsvp.setChecked(False)
                        self.rb_ownrsvr.setChecked(True)

                grid_dir.addLayout(owntype_layh, 0, 1)

                #Directed BD Addr type
                lbl_peertype = QLabel('Peer BD Address Type', self)
                lbl_peertype.setFont(font)
                #BD address type Label
                grid_dir.addWidget(lbl_peertype, 1,0)

                #rb h layout
                dirtype_layh = QHBoxLayout()

                #bd addr type radio buttons
                self.rb_dirpub = QRadioButton('Public', self)
                self.rb_dirpub.setChecked(self.direct_addr_type == A_PUB)

                self.rb_dirrand = QRadioButton('Random', self)
                self.rb_dirrand.setChecked(self.direct_addr_type == A_RAND)

                if False:
                    # direct address type check
                    if self.direct_addr_type == A_PUB:
                        self.rb_dirpub.setChecked(True)
                        self.rb_dirrand.setChecked(False)
                    elif self.direct_addr_type == A_RAND:
                        self.rb_dirpub.setChecked(False)
                        self.rb_dirrand.setChecked(True)

                #BD address type buttons
                dirtype_layh.addWidget(self.rb_dirpub)
                dirtype_layh.addWidget(self.rb_dirrand)

                grid_dir.addLayout(dirtype_layh, 1,1)

                #DIRECTED BD address
                lbl_diraddr = QLabel('Peer BD Address', self)
                lbl_diraddr.setFont(font)
                #BD address Label
                grid_dir.addWidget(lbl_diraddr, 2,0)

                #flip device directed address for print out
                self.edt_peerbd = QLineEdit('0x858483828180', self)
                grid_dir.addWidget(self.edt_peerbd, 2,1)

                tab_layv.addLayout(grid_dir)
            else:
                if True:
                    tab_layv = QVBoxLayout()

                    grid_dir = QGridLayout()

                    # Own address type
                    lbl_ownaddr = QLabel('Own BD Address Type', self)
                    lbl_ownaddr.setFont(font)
                    grid_dir.addWidget(lbl_ownaddr, 0, 0)

                    owntype_layh = QHBoxLayout()
                    owntype_rbg = QButtonGroup(self)
                    #bd addr type radio buttons
                    self.rb_ownpub = QRadioButton('Public')
                    # self.rb_ownpub.setChecked(self.dev.own_addr_type == A_PUB)
                    self.rb_ownpub.setChecked(self.own_addr_type == A_PUB)
                    self.rb_ownpub.toggled.connect(lambda:self.btnstate(self.rb_ownpub))
                    owntype_rbg.addButton(self.rb_ownpub)
                    owntype_layh.addWidget(self.rb_ownpub)

                    self.rb_ownrand = QRadioButton('Random')
                    # self.rb_ownrand.setChecked(self.dev.own_addr_type == A_RAND)
                    self.rb_ownrand.setChecked(self.own_addr_type == A_RAND)
                    self.rb_ownrand.toggled.connect(lambda:self.btnstate(self.rb_ownrand))
                    owntype_rbg.addButton(self.rb_ownrand)
                    owntype_layh.addWidget(self.rb_ownrand)

                    self.rb_ownrsvp = QRadioButton('Resolvable Public')
                    # self.rb_ownrsvp.setChecked(self.dev.own_addr_type == 0x02)
                    self.rb_ownrsvp.setChecked(self.own_addr_type == 0x02)
                    self.rb_ownrsvp.toggled.connect(lambda:self.btnstate(self.rb_ownrsvp))
                    owntype_rbg.addButton(self.rb_ownrsvp)
                    owntype_layh.addWidget(self.rb_ownrsvp)

                    self.rb_ownrsvr = QRadioButton('Resolvable Random')
                    # self.rb_ownrsvr.setChecked(self.dev.own_addr_type == 0x03)
                    self.rb_ownrsvr.setChecked(self.own_addr_type == 0x03)
                    self.rb_ownrsvr.toggled.connect(lambda:self.btnstate(self.rb_ownrsvr))
                    owntype_rbg.addButton(self.rb_ownrsvr)
                    owntype_layh.addWidget(self.rb_ownrsvr)

                    grid_dir.addLayout(owntype_layh, 0, 1)

                    #Directed BD Addr type
                    lbl_peertype = QLabel('Peer BD Address Type', self)
                    lbl_peertype.setFont(font)
                    #BD address type Label
                    grid_dir.addWidget(lbl_peertype, 1,0)

                    #rb h layout
                    dirtype_layh = QHBoxLayout()
                    dirtype_rbg = QButtonGroup(self)

                    #bd addr type radio buttons
                    self.rb_dirpub = QRadioButton('Public', self)
                    self.rb_dirpub.setChecked(self.direct_addr_type == A_PUB)
                    dirtype_rbg.addButton(self.rb_dirpub)

                    self.rb_dirrand = QRadioButton('Random', self)
                    self.rb_dirrand.setChecked(self.direct_addr_type == A_RAND)
                    dirtype_rbg.addButton(self.rb_dirrand)

                    #BD address type buttons
                    dirtype_layh.addWidget(self.rb_dirpub)
                    dirtype_layh.addWidget(self.rb_dirrand)

                    grid_dir.addLayout(dirtype_layh, 1,1)

                    #DIRECTED BD address
                    lbl_diraddr = QLabel('Peer BD Address', self)
                    lbl_diraddr.setFont(font)
                    #BD address Label
                    grid_dir.addWidget(lbl_diraddr, 2,0)

                    #flip device directed address for print out
                    self.edt_peerbd = QLineEdit('0x858483828180', self)
                    grid_dir.addWidget(self.edt_peerbd, 2,1)

                    tab_layv.addLayout(grid_dir)
                else:
                    tab_layv = QVBoxLayout()

                    grid_dir = QGridLayout()

                    # Own address type
                    lbl_ownaddr = QLabel('Own BD Address Type', self)
                    lbl_ownaddr.setFont(font)
                    grid_dir.addWidget(lbl_ownaddr, 0, 0)

                    owntype_layh = QHBoxLayout()
                    owntype_rbg = QButtonGroup()
                    #bd addr type radio buttons
                    self.rb_ownpub = QRadioButton('Public')
                    # self.rb_ownpub.setChecked(self.dev.own_addr_type == A_PUB)
                    self.rb_ownpub.setChecked(self.own_addr_type == A_PUB)
                    self.rb_ownpub.toggled.connect(lambda:self.btnstate(self.rb_ownpub))
                    owntype_rbg.addButton(self.rb_ownpub)
                    owntype_layh.addWidget(self.rb_ownpub)

                    self.rb_ownrand = QRadioButton('Random')
                    # self.rb_ownrand.setChecked(self.dev.own_addr_type == A_RAND)
                    self.rb_ownrand.setChecked(self.own_addr_type == A_RAND)
                    self.rb_ownrand.toggled.connect(lambda:self.btnstate(self.rb_ownrand))
                    owntype_rbg.addButton(self.rb_ownrand)
                    owntype_layh.addWidget(self.rb_ownrand)

                    self.rb_ownrsvp = QRadioButton('Resolvable Public')
                    # self.rb_ownrsvp.setChecked(self.dev.own_addr_type == 0x02)
                    self.rb_ownrsvp.setChecked(self.own_addr_type == 0x02)
                    self.rb_ownrsvp.toggled.connect(lambda:self.btnstate(self.rb_ownrsvp))
                    owntype_rbg.addButton(self.rb_ownrsvp)
                    owntype_layh.addWidget(self.rb_ownrsvp)

                    self.rb_ownrsvr = QRadioButton('Resolvable Random')
                    # self.rb_ownrsvr.setChecked(self.dev.own_addr_type == 0x03)
                    self.rb_ownrsvr.setChecked(self.own_addr_type == 0x03)
                    self.rb_ownrsvr.toggled.connect(lambda:self.btnstate(self.rb_ownrsvr))
                    owntype_rbg.addButton(self.rb_ownrsvr)
                    owntype_layh.addWidget(self.rb_ownrsvr)

                    grid_dir.addLayout(owntype_layh, 0, 1)

                    #Directed BD Addr type
                    lbl_peertype = QLabel('Peer BD Address Type', self)
                    lbl_peertype.setFont(font)
                    #BD address type Label
                    grid_dir.addWidget(lbl_peertype, 1,0)

                    #rb h layout
                    dirtype_layh = QHBoxLayout()
                    dirtype_rbg = QButtonGroup()

                    #bd addr type radio buttons
                    self.rb_dirpub = QRadioButton('Public', self)
                    self.rb_dirpub.setChecked(self.direct_addr_type == A_PUB)
                    dirtype_rbg.addButton(self.rb_dirpub)

                    self.rb_dirrand = QRadioButton('Random', self)
                    self.rb_dirrand.setChecked(self.direct_addr_type == A_RAND)
                    dirtype_rbg.addButton(self.rb_dirrand)

                    #BD address type buttons
                    dirtype_layh.addWidget(self.rb_dirpub)
                    dirtype_layh.addWidget(self.rb_dirrand)

                    grid_dir.addLayout(dirtype_layh, 1,1)

                    #DIRECTED BD address
                    lbl_diraddr = QLabel('Peer BD Address', self)
                    lbl_diraddr.setFont(font)
                    #BD address Label
                    grid_dir.addWidget(lbl_diraddr, 2,0)

                    #flip device directed address for print out
                    self.edt_peerbd = QLineEdit('0x858483828180', self)
                    grid_dir.addWidget(self.edt_peerbd, 2,1)

                    tab_layv.addLayout(grid_dir)
        else:
            tab_layv = QGridLayout()

            grp_addr = QGroupBox(self)
            grp_addr.setTitle('Advertising Addresses')

            #BD address group layout: grid
            grp_addr_layg = QGridLayout()

            #Directed Peer BD address
            lbl_diraddr = QLabel('Peer BD Address', self)
            lbl_diraddr.setFont(font)
            grp_addr_layg.addWidget(lbl_diraddr, 0, 0)

            #Peer address MSB->LSB for print out
            self.edt_peerbd = QLineEdit('0x858483828180', self)
            grp_addr_layg.addWidget(self.edt_peerbd, 0, 1)
            #self.edt_peerbd.setEnabled(self.cbx_dirhdc.isChecked())

            if True:
                #Directed Peer BD addr type
                lbl_peertype = QLabel('Peer BD Address Type', self)
                lbl_peertype.setFont(font)
                grp_addr_layg.addWidget(lbl_peertype, 1, 0)

                #Directed Peer BD addr buttons
                self.rb_dirpub = QCheckBox('Public Device / Identity', self)
                self.rb_dirpub.setChecked(self.direct_addr_type == A_PUB)
                #self.rb_dirpub.setEnabled(self.cbx_dirhdc.isChecked())

                self.rb_dirrand = QCheckBox('Random Device / Identity', self)
                self.rb_dirrand.setChecked(self.direct_addr_type == A_RAND)
                #self.rb_dirrand.setEnabled(self.cbx_dirhdc.isChecked())

                #Directed Peer BD addr buttons location
                grp_addr_layg.addWidget(self.rb_dirpub, 1, 1)
                grp_addr_layg.addWidget(self.rb_dirrand, 1, 2)
            else:
                #Directed BD Addr type
                lbl_peertype = QLabel('Peer BD Address Type', self)
                lbl_peertype.setFont(font)
                #BD address type Label
                grp_addr_layg.addWidget(lbl_peertype, 1,0)

                #rb h layout
                dirtype_layh = QHBoxLayout()

                #bd addr type radio buttons
                self.rb_dirpub = QRadioButton('Public', self)
                self.rb_dirpub.setChecked(self.direct_addr_type == A_PUB)

                self.rb_dirrand = QRadioButton('Random', self)
                self.rb_dirrand.setChecked(self.direct_addr_type == A_RAND)

                #BD address type buttons
                dirtype_layh.addWidget(self.rb_dirpub)
                dirtype_layh.addWidget(self.rb_dirrand)

                grp_addr_layg.addLayout(dirtype_layh, 1,1)

            #Own BD addr type
            lbl_owntype = QLabel('Own BD Address Type', self)
            lbl_owntype.setFont(font)
            grp_addr_layg.addWidget(lbl_owntype, 2, 0)

            #Own BD addr type buttons
            self.rb_ownpub = QRadioButton('Public Device', self)
            self.rb_ownpub.setChecked(self.own_addr_type == A_PUB)

            self.rb_ownrand = QRadioButton('Random Device', self)
            self.rb_ownrand.setChecked(self.own_addr_type == A_RAND)

            self.rb_ownrsvp = QRadioButton('Resolvable Public', self)
            self.rb_ownrsvp.setChecked(self.own_addr_type == 0x02)

            self.rb_ownrsvr = QRadioButton('Resolvable Random', self)
            self.rb_ownrsvr.setChecked(self.own_addr_type == 0x03)

            #Own BD addr type buttons location
            grp_addr_layg.addWidget(self.rb_ownpub, 2, 1)
            grp_addr_layg.addWidget(self.rb_ownrand, 2, 2)
            grp_addr_layg.addWidget(self.rb_ownrsvp, 3, 1)
            grp_addr_layg.addWidget(self.rb_ownrsvr, 3, 2)

            #set layout for group evt prop
            grp_addr.setLayout(grp_addr_layg)

            #add group evt prop to tab
            tab_layv.addWidget(grp_addr, 0, 0)

        self.setLayout(tab_layv)
        self.setWindowTitle("RadioButton demo")
        """

        #keep parent class reference in case it needs updating from here
        # self.parent = parent

        #device to which the dialog instance is associated
        # self.dev = dev
        # self.peer = peer

        #create font for labels
        font = QFont()
        font.setWeight(75)
        font.setBold(True)

        #---------------------------------------------------------------------------------
        #START LAYOUT --------------------------------------------------------------------
        #---------------------------------------------------------------------------------
        #vertical connection tab layout
        tab_layv = QGridLayout()

        #=======================================================================
        # SET ADV PARAM GROUP ---
        #=======================================================================
        grp_set_ud_timersync = QGroupBox(self)
        grp_set_ud_timersync.setTitle('Timer Sync')

        # Periodic advertising parameters grid
        grp_ud_timersync_layg = QGridLayout()

        # Adv handle fields
        lbl_param_advhdl = QLabel('Handle', self)
        lbl_param_advhdl.setFont(font)
        grp_ud_timersync_layg.addWidget(lbl_param_advhdl, 0, 0)

        self.edt_param_advhdl = QLineEdit(str(int(0)), self)  # Add local dev variable value
        grp_ud_timersync_layg.addWidget(self.edt_param_advhdl, 0, 1)

        # Set periodic advertising parameters
        self.b_enable_timer_sync = QPushButton('Enable', self)
        self.connect(self.b_enable_timer_sync, SIGNAL('clicked()'), self.EnableTimerSync)
        grp_ud_timersync_layg.addWidget(self.b_enable_timer_sync, 1, 0)

        # Set periodic advertising parameters
        self.b_disable_timer_sync = QPushButton('Disable', self)
        self.connect(self.b_disable_timer_sync, SIGNAL('clicked()'), self.DisableTimerSync)
        grp_ud_timersync_layg.addWidget(self.b_disable_timer_sync, 1, 1)

        # Set layout for group adv type
        grp_set_ud_timersync.setLayout(grp_ud_timersync_layg)

        # Add group adv type to tab
        tab_layv.addWidget(grp_set_ud_timersync, 0, 0)

        #---------------------------------------------------------------------------------
        #SET LAYOUT ----------------------------------------------------------------------
        #---------------------------------------------------------------------------------
        self.setLayout(tab_layv)
        """

    def EnableTimerSync(self):
        pass

    def DisableTimerSync(self):
        pass

    def btnstate(self,b):
        """
        if b.text() == "Button1":
            if b.isChecked() == True:
                print b.text()+" is selected"
            else:
                print b.text()+" is deselected"

        if b.text() == "Button2":
            if b.isChecked() == True:
                print b.text()+" is selected"
            else:
                print b.text()+" is deselected"
        """

        if b.text() == "Public":
            if b.isChecked() == True:
                print(b.text()+" is selected")
            else:
                print(b.text()+" is deselected")

        if b.text() == "Random":
            if b.isChecked() == True:
                print(b.text()+" is selected")
            else:
                print(b.text()+" is deselected")

        if b.text() == "Resolvable Public":
            if b.isChecked() == True:
                print(b.text()+" is selected")
            else:
                print(b.text()+" is deselected")

        if b.text() == "Resolvable Random":
            if b.isChecked() == True:
                print(b.text()+" is selected")
            else:
                print(b.text()+" is deselected")


def main():
    app = QApplication(sys.argv)
    ex = Radiodemo()
    ex.show()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```



