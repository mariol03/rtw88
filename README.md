rtw88
===========
### A repo for the newest Realtek rtlwifi codes forked form https://github.com/lwfinger/rtw88.

Created as it doesn't compile on Elementary OS 5.1.7 (Hera), beacause it uses an older kernel that it hasn't got fsleep.

### Installation instruction
##### Requirements
You will need to install "make", "gcc", "kernel headers", "kernel build essentials", and "git".
You can install them with the following command, on **Ubuntu**:
```bash
sudo apt-get update
sudo apt-get install make gcc linux-headers-$(uname -r) build-essential git
```
If any of the packets above are not found check if your distro installs them like that. 

##### Installation
For all distros:
```bash
git clone https://github.com/lwfinger/rtw88.git
cd rtw88
make
sudo make install
```
##### Blacklisting (needed if you want to use these modules)
Some distros provide `RTL8723DE` drivers. To use this driver, that one MUST be
blacklisted. How to do that is left as an exercise as learning that will be very beneficial.

If your system has ANY conflicting drivers installed, you must blacklist them as well. For kernels
5.6 and newer, this will include drivers such as rtw88_xxxx.
Here is a useful [link](https://askubuntu.com/questions/110341/how-to-blacklist-kernel-modules) on how to blacklist a module

Once you have reached this point, then reboot. Use the command `lsmod | grep rtw` and check if there are any
conflicting drivers. The correct ones are:
- `rtw_8723de  rtw_8723d  rtw_8822be  rtw_8822b  rtw_8822ce  rtw_8822c  rtw_core  and rtw_pci`

If you have other modules installed, see if you blacklisted them correctly.

##### How to disable/enable a Kernel module
 ```bash
sudo modprobe -r rtw_8723de         #This unloads the module
sudo modprobe rtw_8723de            #This loads the module
```

##### Option configuration
If it turns out that your system needs one of the configuration options, then do the following:
```bash
sudo nano /etc/modprobe.d/<dev_name>.conf 
```
There, enter the line below:
```bash
options <device_name> <<driver_option_name>>=<value>
```
The available options for rtw_pci are disable_msi and disable_aspm.
The available options for rtw_core are lps_deep_mode, support_bf,  and debug_mask.

***********************************************************************************************

When your kernel changes, then you need to do the following:
```bash
cd ~/rtw88
git pull
make
sudo make install
```

Remember, this MUST be done whenever you get a new kernel - no exceptions.

I haven't tried this to build on an older kernel, if you have some problems try creating a GitHub issue with a listing of the build errors. Without the errors, the issue
will be ignored.
