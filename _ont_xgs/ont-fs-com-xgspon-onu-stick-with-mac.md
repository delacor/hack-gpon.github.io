---
title: FS.com XGSPON ONU Stick with MAC (XGS-ONU-25-20NI)
has_children: false
layout: default
parent: FS.com
---

# Hardware Specifications

|                  |                                                                                  |
| ---------------- | -------------------------------------------------------------------------------- |
| Vendor           | FS.com                                                                           |
| Model            | XGS-ONU-25-20NI                                                                  |
| ODM              | CIG                                                                              |
| ODM Product Code | XG-99S                                                                           |
| Chipset          | Cortina CA8271A                                                                  |
| Manufacter       | CIG                                                                              |
| Flash            | 128MB                                                                            |
| RAM              | 128MB                                                                            |
| System           | Custom Linux by Cortina (Saturn SDK) based on Kernel 4.4                         |
| 10GBaseT         | Yes                                                                              |
| Optics           | SC/APC                                                                           |
| IP address       | 192.168.100.1                                                                    |
| Web Gui          |                                                                                  |
| SSH              |                                                                                  |
| Telnet           |                                                                                  |
| Serial           |                                                                                  |
| Form Factor      | miniONT SFP+                                                                     |


## List of partitions

| dev   | size     | erasesize | name            |
| ----- | -------- | --------- | --------------- |
| mtd0  | 00040000 | 00020000  | "ssb"           |
| mtd1  | 00100000 | 00020000  | "uboot-env"     |
| mtd2  | 00100000 | 00020000  | "dtb0"          |
| mtd3  | 00600000 | 00020000  | "kernel0"       |
| mtd4  | 02800000 | 00020000  | "rootfs0"       |
| mtd5  | 00100000 | 00020000  | "dtb1"          |
| mtd6  | 00600000 | 00020000  | "kernel1"       |
| mtd7  | 02800000 | 00020000  | "rootfs1"       |
| mtd8  | 01400000 | 00020000  | "userdata"      |
| mtd9  | 00100000 | 00020000  | "mfginfo1"      |
| mtd10 | 00100000 | 00020000  | "mfginfo2"      |
| mtd11 | 00001000 | 00001000  | "uboot-env2"    |
| mtd12 | 00b09000 | 0001f000  | "squashfs_ubi"  |
| mtd13 | 01078000 | 0001f000  | "userdata"      |
| mtd14 | 00b09000 | 0001f000  | "squashfs_ubi"  |

This ONT supports dual boot. 

`kernel0` and `rootfs0` respectively contain the kernel and firmware of the first image, `kernel1` and `rootfs1` the kernel and the firmware of the second one

{% include_relative ont-nokia-use.md %}

{% include_relative ont-nokia-useful-command.md %}

## Enable SSH (not persistent)

Port 22 is filtered by default and the SSH daemon can be only enabled in runtime. Here is the procedure but it's not persisten accross reboot:

Access UART with `ONTUSER`

Enter `system\misc`

Set `ssh_en` to `1` with the command:
```sh
#ONT>system
#ONT/system>misc
#ONT/system/misc>ssh_en set 1
---ATECMDRESULT--- OK
```

Go back to `system`, then `shell` and run this command:
```sh
#ONT/system/misc>exit
#ONT/system>shell
#ONT/system/shell>iptables -F
```
## Enable Telnet Full Shell

When you're using default credentials to access telnet (`admin`\\`1234`), the prompt is limited to `GponSLID` shell that permits only to modify or display the `PLOAM`
If you change the `admin_mask` to `255.255.255.255`, default credentials stop to work but you can logon with `ONTUSER` and generated password to have full shell like UART

Here is the procedure to change `admin_mask`:

Access UART with `ONTUSER`

Enter `system\misc`

Set `admin_mask` to `255.255.255.255` with the command:
```sh
#ONT>system
#ONT/system>misc
#ONT/system/misc>admin_mask set 255.255.255.255
normal load eep datat
---ATECMDRESULT--- OK
```

Now reboot the ONT and you can access telnet with `ONTUSER` and full power :)