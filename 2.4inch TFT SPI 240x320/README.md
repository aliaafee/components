# 2.4 TFT SPI 240x320

Driver: ILI9341


## Arduino
Note that model requires 3.3v logic, Consider using logic level converter. Otherwise short J1.

see http://www.lcdwiki.com/2.4inch_SPI_Module_ILI9341_SKU:MSP2402
see also http://www.lcdwiki.com/Run_Arduino_Demo_in_spi_model


## RaspberryPi

Source: https://www.xgadget.de/anleitung/2-2-spi-display-ili9341-am-raspberry-betreiben/
(its in german)

pin out

    MISO -> MISO
    LED ->56OhmR->GPIO 18
    SCK -> SCLK
    MOSI -> MOSI
    DC/RS -> GPIO 24
    RESET -> GPIO 25
    CS -> CE 0
    GND -> GND
    VCC -> 3.3V

Enable SPI with raspi-config

Update raspi

Use this driver https://github.com/juj/fbcp-ili9341

    sudo apt-get install git cmake
    cd ~
    git clone https://github.com/juj/fbcp-ili9341.git
    cd fbcp-ili9341
    mkdir build
    cd build
    cmake -DILI9341=ON -DGPIO_TFT_DATA_CONTROL=24 -DGPIO_TFT_RESET_PIN=25 -DGPIO_TFT_BACKLIGHT=18 -DSPI_BUS_CLOCK_DIVISOR=6 -DBACKLIGHT_CONTROL=ON -DSTATISTICS=0 ..
    make -j
    sudo ./fbcp-ili9341


### Following is depricated

Load the module

    sudo modprobe fbtft_device name=tm022hdh26 rotate=90 speed=80000000 fps=60

startx

    sudo FRAMEBUFFER=/dev/fb1 startx

Adjust LED Brightness

    gpio -g mode 18 pwm
    gpio -g pwm 18 1024
    
To autostart create file `/etc/modules-load.d/fbtft.conf`

    spi-bcm2835
    fbtft_device
    
Also create file `/etc/modprobe.d/fbtft.conf`

    options fbtft_device name=tm022hdh26 rotate=90 speed=80000000 fps=60
    
Edit `/etc/rc.local`, add to the end

    con2fbmap 1 1
    sudo FRAMEBUFFER=/dev/fb1 startx
    
    exit 0

Reboot
