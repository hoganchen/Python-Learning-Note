### Struct库的基本用法

```
from struct import pack, unpack
import traceback
from colorama import init, Fore, Back, Style

##########################################################################################
##@brief Colorama configuration and variables for command line prints color interpretation
##########################################################################################
#Allow to reset color allocation after each print
init(autoreset=True)

#Header messages color code
h1_cc = Fore.WHITE + Style.BRIGHT
h2_cc = Fore.YELLOW + Style.BRIGHT

#Minor messages color code
h3_cc  = Fore.WHITE + Style.BRIGHT
h4_cc  = Fore.WHITE + Style.BRIGHT
h5_cc  = Fore.WHITE + Style.BRIGHT
h6_cc  = Fore.WHITE + Style.BRIGHT

h5p_cc = Fore.GREEN + Style.BRIGHT
h5f_cc = Fore.RED + Style.BRIGHT
h5i_cc = Fore.YELLOW + Style.BRIGHT
h5b_cc = Fore.CYAN + Style.BRIGHT

exc_cc  = Fore.GREEN
excn_cc = Fore.RED

#Tx messages color code
hms_cc = Fore.WHITE + Style.BRIGHT + Back.BLUE

#Rx messages color code
hmr_cc = Fore.BLUE  + Back.WHITE

#Parameters messages color code
hpr_cc = Fore.WHITE

#Default console width
WIDTH = 125


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


##########################################################################################
##@brief Error message header print format.
##########################################################################################
def ERR(log, message):
    log.printf("ERROR: " + message)
    exit(-1)


##########################################################################################
##@brief Exception message header print format.
##########################################################################################
def EXC(log, message):
    log.printf(message, exc_cc)


def EXCn(log, message):
    log.printf(message, excn_cc)


##########################################################################################
##@brief Message contents.
##########################################################################################
def MSGTX(log, message, timestamp):
    log.printfile('\n[%s]<UART>TX:[' % HTIME_SHORT(timestamp) + B(message) + ']\n')


##########################################################################################
##@brief Message contents.
##########################################################################################
def MSGRX(log, message, timestamp):
    log.printfile('\n[%s]<UART>RX:[' % HTIME_SHORT(timestamp) + B(message) + ']\n')


##########################################################################################
##@brief LOG on file without print on screen.
##########################################################################################
def LOGFILEONLY(log, message):
    log.printfile('\n%s' % message)


##########################################################################################
##@brief Print message in Square star outlined box, centered ttile
##########################################################################################
def H1(log, name):
    txt = ('\n' + '+' + '=' * (WIDTH - 2) + '+' + \
           '\n|' + ' ' * ((WIDTH - 2 - len(name)) / 2) + name + ' ' * ((WIDTH + 1 - 2 - len(name)) / 2) + '|' + \
           '\n' + '+' + '=' * (WIDTH - 2) + '+')
    log.printf(txt, h1_cc)


##########################################################################################
##@brief Print message in Square dash outlined box, left aligned title.
##########################################################################################
def H2(log, name):
    middle_line_left = '\n| ' + name + ' ' * (WIDTH + 1 - 4 - len(name)) + '|'
    middle_line_right = '\n| ' + ' ' * (WIDTH + 1 - 4 - len(name)) + name + '|'

    txt_left = ('\n+' + '-' * (((WIDTH - 2 - len(name)) / 2) + len(name) + ((WIDTH + 1 - 2 - len(name)) / 2)) + '+' + \
                middle_line_left + \
                '\n+' + '-' * (((WIDTH - 2 - len(name)) / 2) + len(name) + ((WIDTH + 1 - 2 - len(name)) / 2)) + '+')

    txt_right = ('\n+' + '-' * (((WIDTH - 2 - len(name)) / 2) + len(name) + ((WIDTH + 1 - 2 - len(name)) / 2)) + '+' + \
                 middle_line_right + \
                 '\n+' + '-' * (((WIDTH - 2 - len(name)) / 2) + len(name) + ((WIDTH + 1 - 2 - len(name)) / 2)) + '+')

    log.trace(txt_left, txt_right, h2_cc)


##########################################################################################
##@brief Print message on double star inited line
##########################################################################################
def H3(log, name):
    log.trace(('** ' + name + ' ' * (WIDTH - 3 - len(name))),
              (' ' * (WIDTH - 3 - len(name)) + name + ' **'), h3_cc)


##########################################################################################
##@brief Print message on star inited line
##########################################################################################
def H4(log, name):
    log.trace(('* ' + name + ' ' * (WIDTH - 2 - len(name))),
              (' ' * (WIDTH - 2 - len(name)) + name + ' *'), h4_cc)


##########################################################################################
##@brief Print message on simple line
##########################################################################################
def H5(log, name):
    log.trace((' ' + name + ' ' * (WIDTH - 1 - len(name))),
              (' ' * (WIDTH - 1 - len(name)) + name + ' '), h5_cc)


def H5p(log, name):
    log.printf((name + ' ' * (WIDTH - 1 - len(name))), h5p_cc)


def H5f(log, name):
    log.printf((name + ' ' * (WIDTH - 1 - len(name))), h5f_cc)


def H5i(log, name):
    log.printf((name + ' ' * (WIDTH - 1 - len(name))), h5i_cc)


def H5b(log, name):
    log.printf((name + ' ' * (WIDTH - 1 - len(name))), h5b_cc)


##########################################################################################
##@brief Print message on simple line, Device side oriented .
##########################################################################################
def H6(log, message):
    log.trace(('|' + message + ' ' * (WIDTH - 2 - len(message)) + '|'),
              ('|' + ' ' * (WIDTH - 2 - len(message)) + message + '|'), h6_cc)


##########################################################################################
##@brief Print send message name line.
##########################################################################################
def HMS(log, message):
    log.trace(('|--' + message + '->' + ' ' * (WIDTH - 6 - len(message)) + '|'),
              ('| ' + ' ' * (WIDTH - 7 - len(message)) + '<-' + message + '--|'), hms_cc)


##########################################################################################
##@brief Print receive message name line.
##########################################################################################
def HMR(log, message):
    log.trace(('|' + '<-' + message + '-' + ' ' * (WIDTH - 5 - len(message)) + '|'),
              ('|--' + ' ' * (WIDTH - 6 - len(message)) + message + '->' + '|'), hmr_cc)


##########################################################################################
##@brief Print receive parameters line.
##########################################################################################
def HPR(log, par):
    log.trace(('|  {PARAM} ' + par + ' ' * (WIDTH - 5 - 7 - len(par)) + '|'),
              ('|' + ' ' * (WIDTH - 5 - 7 - len(par)) + par + ' {PARAM}  |'), hpr_cc)


##########################################################################################
##@brief Print receive parameters line.
##########################################################################################
def HPR2(log, par):
    log.trace(('|       ' + par + ' ' * (WIDTH - 2 - 7 - len(par)) + '|'),
              ('|' + ' ' * (WIDTH - 2 - 7 - len(par)) + par + '       |'), hpr_cc)


##########################################################################################
##@brief Print send parameters line.
##########################################################################################
def HPS(log, par):
    log.trace(('|  {PARAM} ' + par + ' ' * (WIDTH - 5 - 7 - len(par)) + '|'),
              ('|' + ' ' * (WIDTH - 5 - 7 - len(par)) + par + ' {PARAM}  |'))


##########################################################################################
##@brief Print different number of lines depending on alert value
##########################################################################################
def ALERT(log, level):
    alert_levels = {0: ['NO', 1], 1: ['MILD', 3], 2: ['HIGH', 5]}
    # simulate APP alert TO
    i = 0
    while (i < alert_levels[level][1]):
        H2(log, alert_levels[level][0] + ' ALERT on TARGET!')
        time.sleep(1)
        i += 1


#########################################################################################
## @brief Close indicated log file from log reference list
#########################################################################################
def LOG_CLOSE(log):
    log.close()


#########################################################################################
## @brief Convert millisecond value into hours, minutes, seconds and milliseconds.
#  @param msec  Millisecond value
#  @return Hour, minutes, seconds and milliseconds equivalent
#########################################################################################
def msec_convert(msec):
    secs = msec // 1000
    msec_rests = msec % 1000
    mins = secs // 60
    secs_rests = secs % 60
    hrs = mins // 60
    mins_rests = mins % 60
    return hrs % 24, mins_rests, secs_rests, msec_rests


#########################################################################################
## @brief Print hour, minute, second and millisecond values in a certain format
#  @param msec  Millisecond value
#  @return Formatted string
#########################################################################################
def HTIME_SHORT(ms_val):
    h, m, s, ms = msec_convert(ms_val)
    return '%02d:%02d:%02d:%03d' % (h, m, s, ms)


#########################################################################################
## @brief Print hour, minute, second and millisecond values in a certain format
#  @param msec  Millisecond value
#  @return Formatted string
#########################################################################################
def HTIME_LONG(ms_val):
    h, m, s, ms = msec_convert(ms_val)
    return '%02dh %02dmin %02ds %03dms' % (h, m, s, ms)


#########################################################################################
## @brief When an exception occurs, print stack trace.
#########################################################################################
def PRINT_STACK_TRACE(log=None):
    if (log != None):
        formatted_lines = traceback.format_exc().splitlines()
        H3(log, '')
        for excep_line in enumerate(formatted_lines):
            H3(log, excep_line[1])
        H3(log, '')
    else:
        formatted_lines = traceback.format_exc().splitlines()
        for excep_line in enumerate(formatted_lines):
            print
            excep_line[1]


txt_str = '0x12345678'
print('txt: {}'.format(INVB(S2B(str(txt_str)))))

txt_byte = S2B(str(txt_str))
print('txt: {}'.format(txt_byte))
print('txt_byte[0] = 0x{}'.format(B2(txt_byte[0])))
print('txt_byte = 0x{}'.format(B2(txt_byte)))

if 0x12 == int('0x{}'.format(B2(txt_byte[0])), 16):
    print('0x12 == {} (B2(txt_byte[0])'.format(B2(txt_byte[0])))
else:
    print('0x12 != {} (B2(txt_byte[0])'.format(B2(txt_byte[0])))

if 0x12345678 == int('0x{}'.format(B2(txt_byte)), 16):
    print('0x12345678 == 0x{} (B2(txt_byte[0])'.format(B2(txt_byte)))
else:
    print('0x12345678 != 0x{} (B2(txt_byte[0])'.format(B2(txt_byte)))

txt_str = '0x0000a0'
print('txt: {}'.format(INVB(S2B(str(txt_str)))))

txt_str = '0xa0'
print('txt: {}'.format(INVB(S2B(str(txt_str)))))

```



