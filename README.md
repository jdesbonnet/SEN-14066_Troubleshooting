# SEN-14066_Troubleshooting

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
thought was this was the lines or scope probe picking up the UHF transmitter, but I measured
the frequency to be exactly 40MHz: not the 900MHz or so from the transmitter. I suspect 
this signal is prevening the UART from functioning. 

Other obserations: the power LED is on. The board does get warm. On my SDR I can see a strong 
signal at about 900MHz while the board is powered.

Do you have any suggestions on further troublshooting?

BTW I can assure you, because of the relatively high cost of the board, it has been treated with the
upmost care.  

