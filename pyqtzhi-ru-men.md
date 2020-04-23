### PyQt入门之Demo 01

```
import sys
import tushare
from PyQt5 import QtWidgets
from PyQt5.QtWidgets import QWidget
from PyQt5.QtCore import QCoreApplication, Qt


class Example(QWidget):
    def __init__(self):
        super().__init__()
        self.init_ui()

    def init_ui(self):
        qt_button = QtWidgets.QPushButton('Quit', self)
        qt_button.clicked.connect(QCoreApplication.instance().quit)
        qt_button.resize(qt_button.sizeHint())
        qt_button.move(50, 50)

        qt_label = QtWidgets.QLabel(self)
        qt_label.setText("test")
        qt_label.move(10, 10)

        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('Quit Button')
        self.setWindowFlags(Qt.FramelessWindowHint)
        self.show()


if __name__ == '__main__':
    app = QtWidgets.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())
```



