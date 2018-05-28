# SEN-14066_Troubleshooting

UPDATE 29 May 2018, 0300Z: I'm reading tags with python-mercuryapi on Linux now.   To get this working I used a Prolific USB UART adapter and supplied Vcc from a separate PSU.  Even with a seprate PSU the genuine FTDI cables were acting weird (as described below). 

```
joe@joe-Precision-T1650:/var/tmp/python-mercuryapi$ python3 test.py 
M6e Nano
['NA2', 'NA3', 'IN', 'JP', 'PRC', 'EU3', 'KR2', 'AU', 'NZ', 'open']
[1]
[(216, 32537)]
[]
Reading...
b'E200400573070143198047A5' 1 1 215
b'E2004005730701341980478D' 1 1 214
b'E20040057307013919804797' 1 1 213
b'E200400573070143198047A5' 1 1 211
b'E20040057307013919804797' 1 1 216
b'E2004005730701341980478D' 1 1 213
b'E2004005730701301980477F' 1 1 211
b'E200400573070147198047A7' 1 1 211
```

*********************************

UPDATE 28 May 2018, 1030Z: I changed the UART adapter to a cleap Prolific device. Using python-mercuryapi on Linux I was able to establish two way communication with the M6E-NANO device. The UART adapter quickly disconnects which is almost certainly because it cannot supply enough current. This is major progess. 

*********************************


I'm having difficulty establishing communication with the SEN-14066 board through the FTDI UART connector.

I've tried the Universal Reader Assistant on both Windows 10 (both VM and real machine) using genuine FTDI
cable without any success. The OS recognizes the FTDI cable and assigns a COM port to it.
On opening the Universal Reader Assistant, it scans the COM port but times out (see screen grab).

![picture of SEN-15066 with FTDI header and cable](./board_with_ftdi_cable.jpg)

![screen grab of Universal Reader Assistant attempting to make a connection to SEN-14066](./Universal_Reader_Assistant.png)

I probed the TXO and TXI lines with an oscillocope to make sure signals are going in both direction. Please
see scope screen grab below. I can see small bursts of traffic in the RXI line (green), but nothing
on return (yellow). 

![scope screen grab of RXI (green) and TXO (yellow) while URA utility scans COM port](./scope_1.png)


However of more concern is a very large high frequency signal superimposed on the UART lines. My first
thought was this was UHF from the transmitter, but I measured
the frequency to be exactly 40MHz: not the 900MHz or so from the transmitter. I suspect 
this signal is prevening the UART from functioning. 

![scope screen grab of RXI (green) and TXO (yellow) at 10ns/div timebase showing 40MHz signal](./scope_0.png)

Other obserations: the power LED is on. The board does get warm. On my SDR I can see a strong 
signal at about 900MHz while the board is powered.

This is the scope trace when I plug in the FTDI cable. Blue is Vcc.

![scope trace on plugging in USB cable](./scope_3.png)


Do you have any suggestions on further troublshooting?

BTW I can assure you, because of the relatively high cost of the board, it has been treated with the
upmost care.  

Update 28 May 2018: More observations:

 * I connected to FTDI cable UART port with Linux picocom: garbage is being printed on the terminal screen dispite the oscilloscope trace showing no data activity on the incomig line (TXO from the board).  It seems this 40MHz noise signal is definately interfering with UART operation.

 * When sending random characters via picocom terminal program, I can see the UART data (with the super-imposed noise signal) on both sides of the buffer chip. I don't see any data being transmitted from the board on either side of the buffer chip.
 
 * Sending any character to the board seems to randomly change the amplitude of the noise signal.
 
 * I wonder is it getting enough power from FTDI cable? Vcc also has this 40MHz noise oscillating from about 4.6V to 5.0V. [No I don't think so. Just tried powering from a separate 5V PSU. Drawing 200mA. But noise signal still there.] 

