# led-strip-controller
Figure out how to control LED strip from computer 

Currently in research phase. 

Documentation:
* https://www.youtube.com/watch?v=1r_wxBg0VMM
* https://learn.adafruit.com/rgb-led-strips/circuitpython-code
* https://www.reddit.com/r/Python/comments/7r268d/programmable_ledstrips/
* https://dordnung.de/raspberrypi-ledstrip/


There's an off the shelf company that builds this already:
* https://www.blinkstick.com/products/blinkstick-flex#mini-shop


Controlling USB from python:
* https://stackoverflow.com/questions/64990689/control-the-power-of-a-usb-port-in-python
  * Windows: https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/devcon-examples
    * https://stackoverflow.com/questions/53913408/how-to-disable-enable-a-specific-usb-port-in-windows-with-python
      ```
      import subprocess
      # Fetches the list of all usb devices:
      result = subprocess.run(['devcon', 'hwids', '=usb'], 
      capture_output=True, text=True)

      #   ... add code to parse the result and get the hwid of the device you want ...

      subprocess.run(['devcon', 'disable', parsed_hwid]) # to disable
      subprocess.run(['devcon', 'enable', parsed_hwid]) # to enable
      ```
  * Linux: https://stackoverflow.com/questions/4702216/controlling-a-usb-power-supply-on-off-with-linux/12675749#12675749
     ```
     import subprocess

     # determine desired usb device

     # to disable
     subprocess.run(['echo', '0', '>' '/sys/bus/usb/devices/usbX/power/autosuspend_delay_ms']) 
     subprocess.run(['echo', 'auto', '>' '/sys/bus/usb/devices/usbX/power/control']) 
     # to enable
     subprocess.run(['echo', 'on', '>' '/sys/bus/usb/devices/usbX/power/control']) 
     ```
     * I'm using Linux. How do I apply vendor ids and product ids to this? 
     * You can list the the usb devices using the lsusb command. This will list usb buses and the vender id. Then you can check the vender id and/or product id for each usbX with the cat command. Example: "cat /sys/bus/usb/devices/usb1/idVendor" – 
     * You can also use pyserial's serial.tools.list_ports.comports to get vendor ids. – 
