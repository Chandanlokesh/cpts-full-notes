
---

## 🖥️ Linux System Information Commands – Cheat Sheet

|**Command**|**Description**|**Example Usage**|
|---|---|---|
|`whoami`|Displays current username.|`whoami`|
|`id`|Returns user identity (UID, GID, groups).|`id`|
|`hostname`|Sets or prints the system's hostname.|`hostname`|
|`uname`|Prints basic OS and hardware information.|`uname -a`|
|`pwd`|Prints the present working directory.|`pwd`|
|`ifconfig`|Views or configures network interfaces (older systems).|`ifconfig eth0`|
|`ip`|Shows/manages IP address and routes (modern replacement for ifconfig).|`ip addr show`|
|`netstat`|Displays network connections and routing tables. _(deprecated)_|`netstat -tuln`|
|`ss`|Displays socket statistics (faster replacement for `netstat`).|`ss -tuln`|
|`ps`|Shows the currently running processes.|`ps aux`|
|`who`|Shows who is logged in.|`who`|
|`env`|Prints environment variables.|`env`|
|`lsblk`|Lists block devices (hard disks, partitions, etc).|`lsblk`|
|`lsusb`|Lists USB devices connected to the system.|`lsusb`|
|`lsof`|Lists open files and associated processes.|`lsof -i :80`|
|`lspci`|Lists all PCI devices (like graphic cards, NICs, etc.).|`lspci`|
|`ssh user@ip`|Logs into another system remotely via SSH.|`ssh user@192.168.1.10`|

---

