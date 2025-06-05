# 🔌 What Happens When You Plug in an Arduino Uno to a Linux System

This guide explains exactly what happens — both hardware and software — when an Arduino Uno is connected via USB to a Linux system. It also shows what commands/tools you can use to observe each step.

---

## 🧠 Full Event Chain with Commands  

| #  | Step                       | Layer         | Description                                              | 🛠️ Tool or Command to Check                                 |  
|----|----------------------------|---------------|----------------------------------------------------------|-------------------------------------------------------------|  
| 1  | USB plug-in                | Physical      | Connect Arduino to USB port                              | *Manual action*                                             |  
| 2  | Power & signal lines       | Electrical    | USB powers Arduino; D+/D− signals device presence        | *Not visible from software*                                 |  
| 3  | USB controller interrupt   | Host hardware | Hardware detects line state change                       | `dmesg -w`                                                  |  
  4  | Device detection           | Kernel        | Kernel detects USB attach                                | `dmesg -w` (look for "new full-speed USB device")           |  
| 5  | USB Enumeration            | Kernel        | Kernel reads USB descriptors (VID/PID/class)             | `lsusb -v`, `dmesg`                                         |  
| 6  | Device class match         | Kernel        | Matches CDC ACM class (USB serial)                       | `lsusb -t`, `usb-devices`                                   |  
| 7  | Driver loaded              | Kernel        | Kernel loads `cdc_acm` driver                            | `lsmod | grep cdc_acm`                                      |  
| 8  | Uevent sent                | Kernel → udev | Kernel sends uevent to udev                              | `udevadm monitor`                                           |  
| 9  | udev rule matched          | User-space    | `udev` matches rule based on device info                 | `udevadm info --name=/dev/ttyACM0 --attribute-walk`         |  
| 10 | Device node created        | udev          | Creates `/dev/ttyACM0`                                   | `ls -l /dev/ttyACM*`                                        |  
| 11 | Permissions assigned       | udev          | Device owned by `root:dialout`, mode `0660`              | `ls -l /dev/ttyACM*`                                        |  
| 12 | User access granted        | User-space    | User in `dialout` group can access it                    | `groups $USER`, `sudo usermod -aG dialout $USER`            |  
| 13 | Communication starts       | Application   | Apps can now open/read/write to `/dev/ttyACM0`           | `cat`, `echo`, `screen`, or code (e.g., Lazarus, Python)    |  

---

## 🧪 Suggested Monitoring Setup

To learn while watching things live, use:

### 1. Kernel Messages
```bash
dmesg -w
