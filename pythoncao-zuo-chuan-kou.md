### Python操作串口

可能是pyserial库的bug，在某些情况下，使用readlines\(\)会挂死，这是可使用read\(size\)或者read\_until\(\)函数来替代

```py
class SerialPortHandleClass:
    def __init__(self, port, baud_rate='115200', rw_timeout=5):
        self.port = port
        self.baud_rate = baud_rate
        self.rw_timeout = rw_timeout
        self.__read_size = 10000
        self.__serial_handle = None
        self.__serial_status_flag = False
        self.serial_abnormal_flag = False

    def open_serial_port(self):
        try:
            self.__serial_handle = serial.Serial(self.port, self.baud_rate,
                                                 timeout=self.rw_timeout, write_timeout=self.rw_timeout)
        except (serial.SerialTimeoutException, serial.SerialException):
            self.__serial_status_flag = False
            self.serial_abnormal_flag = False
        else:
            self.__serial_status_flag = True
            self.serial_abnormal_flag = True

        return self.__serial_status_flag

    def close_serial_port(self):
        if self.__serial_status_flag and self.serial_abnormal_flag:
            self.__serial_handle.close()
            self.__serial_status_flag = False

    def send_cmd_to_serial(self, cmd):
        if self.__serial_status_flag and self.serial_abnormal_flag:
            # cmd_byte = cmd.encode('utf-8')'
            try:
                self.__serial_handle.write(cmd)
            except (serial.SerialTimeoutException, serial.SerialException):
                self.__serial_status_flag = False
                self.serial_abnormal_flag = False

    def reset_output_buffer(self):
        if self.__serial_status_flag and self.serial_abnormal_flag:
            self.__serial_handle.reset_output_buffer()

    def get_data_from_serial(self, read_type=0):
        serial_line_data = None

        if self.__serial_status_flag and self.serial_abnormal_flag:
            try:
                if 0 == read_type:
                    serial_line_data = self.__serial_handle.readline()
                else:
                    # serial_line_data = self.__serial_handle.readlines()
                    serial_line_data = self.__serial_handle.read(size=self.__read_size)
            except serial.SerialException:
                self.__serial_status_flag = False
                self.serial_abnormal_flag = False

        return serial_line_data
```

刚查明原因，不是readlines\(\)有bug，而是串口如果一直有数据，readlines不会timeout返回，而一直在读数据，解决办法为readlines\(max\_size\)方式调用

```python
class SerialPortHandleClass:
    def __init__(self, port, baud_rate='115200', read_timeout=1, write_time=1):
        self.port = port
        self.baud_rate = baud_rate
        self.read_timeout = read_timeout
        self.write_timeout = write_time
        self.__read_size = 4000

        self.__serial_handle = None
        self.serial_rw_status_flag = False

        self.open_serial_port()

    def open_serial_port(self):
        try:
            self.__serial_handle = serial.Serial(self.port, self.baud_rate, timeout=self.read_timeout,
                                                 write_timeout=self.write_timeout)
        except (serial.SerialTimeoutException, serial.SerialException) as err:
            self.serial_rw_status_flag = False
            logging.critical('Can not open serial port {}, error message: {}'.format(self.port, err))
            raise IOError(err)
        else:
            self.serial_rw_status_flag = True

    def close_serial_port(self):
        if self.serial_rw_status_flag:
            self.__serial_handle.close()
            self.serial_rw_status_flag = False

    def send_cmd_to_serial(self, cmd_str):
        if self.serial_rw_status_flag:
            try:
                cmd_str = re.sub(r'\s+', '', cmd_str)
                if 'stack' == st_config.TEST_ENV['category_name']:
                    cmd_byte = binascii.a2b_hex(cmd_str)
                else:
                    cmd_byte = cmd_str.encode('utf-8')

                self.__serial_handle.write(cmd_byte)
            except (serial.SerialTimeoutException, serial.SerialException):
                self.serial_rw_status_flag = False
            except Exception as err:
                logging.error('Unexpected error happened, error information: {}'.format(err))

    def reset_output_buffer(self):
        if self.serial_rw_status_flag:
            self.__serial_handle.reset_output_buffer()

    def get_data_from_serial(self, read_flag=1):
        serial_data = None

        if self.serial_rw_status_flag:
            try:
                if 0 == read_flag:
                    serial_data = self.__serial_handle.readline()
                elif 1 == read_flag:
                    serial_data = self.__serial_handle.readlines(self.__read_size)
                else:
                    logging.error('Unsupported read flag value {}'.format(read_flag))
            except serial.SerialException:
                self.serial_rw_status_flag = False

        return serial_data

```



