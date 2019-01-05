# How to change default console uart port for Orange Pi

Consider for example uart1.  
Go to `/boot/`
  
`sudo bin2fex script.bin custom.bin`
  
locate port below: 
 
    [uart1]
    uart_used = 0
    uart_port = 1
    uart_type = 4
    uart_tx = port:PG06<2><1><default><default>
    uart_rx = port:PG07<2><1><default><default>
    uart_rts = port:PG08<2><1><default><default>
    uart_cts = port:PG09<2><1><default><default>

set `uart_used = 1`, `uart_type = 2` (two-wire mode)

`sudo fex2bin custom.fex custom.bin`

`sudo ln -sF script.bin custom.bin`

change `boot.cmd` the following lines from:

    if test "${console}" = "display" || test "${console}" = "both"; then setenv consoleargs "console=tty1"; fi
    if test "${console}" = "serial" || test "${console}" = "both"; then setenv consoleargs "${consoleargs} console=ttyS0,115200"; fi
    
to:

    if test "${console}" = "display" || test "${console}" = "both"; then setenv consoleargs "console=tty2"; fi
    if test "${console}" = "serial" || test "${console}" = "both"; then setenv consoleargs "${consoleargs} console=ttyS1,115200"; fi

then do `sudo mkimage -C none -A arm -T script -d boot.cmd boot.scr` and `sudo reboot`  
Enjoy

