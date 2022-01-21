# led-strip-controller
h1. GOALS
* Figure out how to control LED strip from computer 
* Figure out how to detect if software is running on computer
* Figure out how to change LED strip color depending on software running state

h1. Currently in research phase. 

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


List USB devices on MAC: https://apple.stackexchange.com/questions/170105/list-usb-devices-on-osx-command-line
**ioreg** command:
```
$ ioreg -p IOUSB
+-o Root  <class IORegistryEntry, id 0x100000100, retain 24>
  +-o AppleUSBVHCIBCE Root Hub Simulation@80000000  <class AppleUSBRootHubDevice, id 0x10000052e, registered, matched, active, busy 0$
  | +-o Headset@80400000  <class AppleUSBDevice, id 0x100000533, registered, matched, active, busy 0 (0 ms), retain 11>
  | +-o Ambient Light Sensor@80300000  <class AppleUSBDevice, id 0x100000537, registered, matched, active, busy 0 (0 ms), retain 11>
  | +-o Apple T2 Controller@80100000  <class AppleUSBDevice, id 0x10000053c, registered, matched, active, busy 0 (0 ms), retain 13>
  | +-o FaceTime HD Camera (Built-in)@80200000  <class AppleUSBDevice, id 0x100000543, registered, matched, active, busy 0 (0 ms), re$
  +-o AppleUSBXHCI Root Hub Simulation@14000000  <class AppleUSBRootHubDevice, id 0x100000567, registered, matched, active, busy 0 (0$
  | +-o EMV Smartcard Reader@14500000  <class AppleUSBDevice, id 0x1000005a0, registered, matched, active, busy 0 (0 ms), retain 15>
  | +-o USB2.0 Hub@14700000  <class AppleUSBDevice, id 0x100020914, registered, matched, active, busy 0 (0 ms), retain 14>
  | | +-o Advanced Corded Mouse M500s@14720000  <class AppleUSBDevice, id 0x10002092b, registered, matched, active, busy 0 (7 ms), re$
  | | +-o IOUSBHostDevice@14710000  <class AppleUSBDevice, id 0x100020933, registered, matched, active, busy 0 (4 ms), retain 12>
  | +-o iPhone@14600000  <class AppleUSBDevice, id 0x10002acb3, registered, matched, active, busy 0 (548 ms), retain 18>
  +-o AppleUSBXHCI Root Hub Simulation@00000000  <class AppleUSBRootHubDevice, id 0x100000573, registered, matched, active, busy 0 (0$
  | +-o USB3.0 Hub@00200000  <class AppleUSBDevice, id 0x100000575, registered, matched, active, busy 0 (0 ms), retain 15>
  |   +-o USB 10/100/1000 LAN@00240000  <class AppleUSBDevice, id 0x1000005bc, registered, matched, active, busy 0 (0 ms), retain 16>
  +-o AppleUSBEHCI Root Hub Simulation@40000000  <class AppleUSBRootHubDevice, id 0x1000209ea, registered, matched, active, busy 0 (0$
    +-o IOUSBHostDevice@40100000  <class AppleUSBDevice, id 0x1000209ed, registered, matched, active, busy 0 (0 ms), retain 15>
      +-o Apple Thunderbolt Display@40170000  <class AppleUSBDevice, id 0x100020a0a, registered, matched, active, busy 0 (3 ms), reta$
      +-o FaceTime HD Camera (Display)@40150000  <class AppleUSBDevice, id 0x100020a13, registered, matched, active, busy 0 (9 ms), r$
      +-o Display Audio@40140000  <class AppleUSBDevice, id 0x100020a54, registered, matched, active, busy 0 (14 ms), retain 18>
  ```
