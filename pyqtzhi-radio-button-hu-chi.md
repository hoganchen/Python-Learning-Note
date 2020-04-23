### PyQt入门之radio button互斥

```
import sys
from PyQt4.QtCore import *
from PyQt4.QtGui import *

class Test(QWidget):
    def __init__(self):
        super(Test, self).__init__()

        if True:
            if False:
                layout = QVBoxLayout(self)

                gBBackupFromIntExt = QGroupBox()
                layout.addWidget(gBBackupFromIntExt)

                bGBackupFromIntExt = QButtonGroup(self)

                self.rBBackupFromExt = QRadioButton()
                bGBackupFromIntExt.addButton(self.rBBackupFromExt)
                layout.addWidget(self.rBBackupFromExt)

                self.rBBackupFromInt = QRadioButton()
                bGBackupFromIntExt.addButton(self.rBBackupFromInt)
                layout.addWidget(self.rBBackupFromInt)

                gBBackupToIntExt = QGroupBox()
                layout.addWidget(gBBackupToIntExt)

                bGBackupToIntExt = QButtonGroup(self)

                self.rBBackupToExt = QRadioButton()
                bGBackupToIntExt.addButton (self.rBBackupToExt)
                layout.addWidget(self.rBBackupToExt)

                self.rBBackupToInt = QRadioButton()
                bGBackupToIntExt.addButton (self.rBBackupToInt)
                layout.addWidget(self.rBBackupToInt)
            else:
                font = QFont()
                font.setWeight(75)
                font.setBold(True)

                A_PUB = 0x0
                A_RAND = 0x1

                # Adv type
                ADVCUND = 0x00
                ADVCD = 0x01
                ADVDISC = 0x02
                ADVNCUND = 0x03
                ADVCDLDC = 0x04
                ADVSCRSP = 0x04

                self.own_addr_type = A_PUB
                self.direct_addr_type = A_RAND
                self.adv_type = ADVCD

                #---------------------------------------------------------------------------------
                #START LAYOUT --------------------------------------------------------------------
                #---------------------------------------------------------------------------------
                #vertical general layout
                tab_layv = QVBoxLayout()

                #ADV TYPE GROUP-------------------------------------------------------------------
                grp_advtyp = QGroupBox(self)
                grp_advtyp.setTitle('Type')

                #adv type group horizontal layout
                grp_advtyp_layh = QHBoxLayout()

                self.rb_cund = QRadioButton('Connectable', grp_advtyp)
                self.rb_cund.setChecked(self.adv_type == ADVCUND)
                grp_advtyp_layh.addWidget(self.rb_cund)

                self.rb_cd = QRadioButton('Directed HDC', grp_advtyp)
                self.rb_cd.setChecked(self.adv_type == ADVCD)
                grp_advtyp_layh.addWidget(self.rb_cd)

                self.rb_disc = QRadioButton('Scannable', grp_advtyp)
                self.rb_disc.setChecked(self.adv_type == ADVDISC)
                grp_advtyp_layh.addWidget(self.rb_disc)

                self.rb_ncund = QRadioButton('Non connectable', grp_advtyp)
                self.rb_ncund.setChecked(self.adv_type == ADVNCUND)
                grp_advtyp_layh.addWidget(self.rb_ncund)

                self.rb_cdlow = QRadioButton('Directed LDC', grp_advtyp)
                self.rb_cdlow.setChecked(self.adv_type == ADVCDLDC)
                grp_advtyp_layh.addWidget(self.rb_cdlow)

                #set layout for group adv type
                grp_advtyp.setLayout(grp_advtyp_layh)

                #add group adv type to tab vertical layout
                tab_layv.addWidget(grp_advtyp)

                #=======================================================================
                # ADV ADDRESSES MANAGEMENT ---
                #=======================================================================
                grid_dir = QGridLayout()

                # Own address type
                lbl_ownaddr = QLabel('Own BD Address Type', self)
                lbl_ownaddr.setFont(font)
                grid_dir.addWidget(lbl_ownaddr, 0,0)

                owntype_layh = QHBoxLayout()
                owntype_rbg = QButtonGroup(self)

                #bd addr type radio buttons
                self.rb_ownpub = QRadioButton('Public', self)
                self.rb_ownpub.setChecked(self.own_addr_type == A_PUB)
                owntype_rbg.addButton(self.rb_ownpub)
                owntype_layh.addWidget(self.rb_ownpub)

                self.rb_ownrand = QRadioButton('Random', self)
                self.rb_ownrand.setChecked(self.own_addr_type == A_RAND)
                owntype_rbg.addButton(self.rb_ownrand)
                owntype_layh.addWidget(self.rb_ownrand)

                self.rb_ownrsvp = QRadioButton('Resolvable Public', self)
                self.rb_ownrsvp.setChecked(self.own_addr_type == 0x02)
                owntype_rbg.addButton(self.rb_ownrsvp)
                owntype_layh.addWidget(self.rb_ownrsvp)

                self.rb_ownrsvr = QRadioButton('Resolvable Random', self)
                self.rb_ownrsvr.setChecked(self.own_addr_type == 0x03)
                owntype_rbg.addButton(self.rb_ownrsvr)
                owntype_layh.addWidget(self.rb_ownrsvr)

                """
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
                """

                grid_dir.addLayout(owntype_layh, 0,1)

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

                """
                # direct address type check
                if self.direct_addr_type == A_PUB:
                    self.rb_dirpub.setChecked(True)
                    self.rb_dirrand.setChecked(False)
                elif self.direct_addr_type == A_RAND:
                    self.rb_dirpub.setChecked(False)
                    self.rb_dirrand.setChecked(True)
                """

                #BD address type buttons
                dirtype_layh.addWidget(self.rb_dirpub)
                dirtype_layh.addWidget(self.rb_dirrand)

                grid_dir.addLayout(dirtype_layh, 1,1)

                #DIRECTED BD address
                lbl_diraddr = QLabel('Peer BD Address', self)
                lbl_diraddr.setFont(font)
                #BD address Label
                grid_dir.addWidget(lbl_diraddr, 2,0)

                #flip ce directed address for print out
                self.edt_peerbd = QLineEdit('0x858483828180', self)
                grid_dir.addWidget(self.edt_peerbd, 2,1)

                #DIRECTED BD address
                lbl_roleswitch = QLabel('Role Switch', self)
                lbl_roleswitch.setFont(font)
                #BD address Label
                grid_dir.addWidget(lbl_roleswitch, 3,0)

                #flip ce directed address for print out
                self.edt_roleswitch = QLineEdit('0x3', self)
                grid_dir.addWidget(self.edt_roleswitch, 3,1)

                # Set periodic advertising parameters
                self.b_role_switch = QPushButton('Master-Slave<-->Active-Sniffer', self)
                self.connect(self.b_role_switch, SIGNAL('clicked()'), self.RoleSwitch)
                grid_dir.addWidget(self.b_role_switch, 4, 0)

                # Finalizing full grid
                tab_layv.addLayout(grid_dir)

                self.setLayout(tab_layv)
        else:
            # Setup UI
            layout = QVBoxLayout(self)

            gBBackupFromIntExt = QGroupBox()
            layout.addWidget(gBBackupFromIntExt)

            bGBackupFromIntExt = QButtonGroup()

            self.rBBackupFromExt = QRadioButton()
            bGBackupFromIntExt.addButton (self.rBBackupFromExt)
            layout.addWidget(self.rBBackupFromExt)

            self.rBBackupFromInt = QRadioButton()
            bGBackupFromIntExt.addButton (self.rBBackupFromInt)
            layout.addWidget(self.rBBackupFromInt)

            gBBackupToIntExt = QGroupBox()
            layout.addWidget(gBBackupToIntExt)

            bGBackupToIntExt = QButtonGroup()

            self.rBBackupToExt = QRadioButton()
            bGBackupToIntExt.addButton (self.rBBackupToExt)
            layout.addWidget(self.rBBackupToExt)

            self.rBBackupToInt = QRadioButton()
            bGBackupToIntExt.addButton (self.rBBackupToInt)
            layout.addWidget(self.rBBackupToInt)

    def RoleSwitch(self):
        print('peer address: {}'.format(self.edt_peerbd.text()))
        print('role switch: {}'.format(self.edt_roleswitch.text()))


def main():
    """
    a = QApplication(sys.argv)
    t = Test()
    t.show()
    a.exec()
    """
    app = QApplication(sys.argv)
    ex = Test()
    ex.show()
    sys.exit(app.exec_())
    # """


if __name__ == '__main__':
    main()
```



