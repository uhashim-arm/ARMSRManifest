# ARMSR Manifest for RK3399 EVB Target Setup

The information provided here provides the configuration and build setup for ARM System Ready compliance verification on Rockchip RK3399

There are three Evaluation Platforms being used as platform for the verification

| Target  | Manifest XML  | Device |
| :------------: |:---------------:| :-----:|
| Toy Brick      | TB-RK3399proD.xml | RK3399 Pro |
| Lenovo Leez     | Leez-P710.xml        |  Leez-p710 |
| Pine 64 | RockPro64.xml |    RockPro 64 |


## 1. Install repo
Follow the instructions under the "Installing Repo" section
[here](https://source.android.com/source/downloading.html).

## 2. Get the source code
```
mkdir <New_Dir>
cd <New_Dir>
repo init -u https://github.com/uhashim-arm/ARMSRManifest.git -m TB-RK3399proD.xml 
repo sync -j4 --no-clone-bundle
```

## 3. Get the Toolkits 
If you dont have an aarch64-linux-gnu cross compiler already installed on your host machine, there are 2 ways of suppling the toolkits required to build the firmware and u-boot binaries. 

1. [Preferred] Package install the cross-compilers on the host machine OS
2. [Backup] Use the toolchain installer thats provided by the makefile.

Option 1: 

```
sudo apt-get install gcc-aarch64-linux-gnu
which aarch64-linux-gnu-gcc
```
Copy the path reported by the above command (excluding the tailing `bin/aarch64-linux-gnu-gcc`) and paste it after the command below:

```
export AARCH64_PATH=
```
For example: 

```
$ which aarch64-linux-gnu-gcc 
/usr/bin/aarch64-linux-gnu-gcc
$ export AARCH64_PATH=/usr
$ echo $AARCH64_PATH
/usr
```

Option 2: 

```
cd build/
make -j2 toolchains
```

## 4. Build the target
```
cd build/
make -j `nproc`
```
Amongst other things the above command will generate the following files of interest in the `../out/bin/u-boot/` directory:

```
u-boot.itb
idbloader.img
```
Check the above by running: 
```
ls -ltr ../out/bin/u-boot/
```



## 5. Building the FW binaries
```
cd ../rkbin
./tools/boot_merger  ./RKBOOT/RK3399PROMINIALL.ini
```
The above command will generate the miniloader file: eg. `rk3399pro_loader_v1.25.126.bin`

## 6. Flashing the FW binaries and U-Boot

### 6.a Preparing the host-machine
To succesgfully detect and flash the board the following packages are required to be installed on the host machine

```
sudo apt-get install pkg-config libudev-dev libusb-1.0-0-dev libusb-1.0 
```


### 6.b Put the board in MaskROM mode 
1. Power-off the board
2. Keep the Maskrom button pressed for 10 seconds
3. Power on the board (either by long-pressing the power button OR plugging in the Power connector)
4. Release maskrom button after 5 seconds
5. The device should now be in Maskrom mode ready for flashing


### 6.c Writing the binaries to device 
```
# Check if the following device is detected:
#    Fuzhou Rockchip Electronics Company RK3399 in Mask ROM mode
lsusb 

sudo ./tools/rkdeveloptool db rk3399pro_loader_v1.25.126.bin && sleep 2
sudo ./tools/rkdeveloptool wl 0x40 ../out/bin/u-boot/idbloader.img && sleep 2
sudo ./tools/rkdeveloptool wl 0x4000 ../out/bin/u-boot/u-boot.itb && sleep 2
sudo ./tools/rkdeveloptool rd
```

The TB_RK3399ProD board should restart and boot into u-boot. If there is a pre-installed distro available on the on-board flash then the system will boot into it.


# Booting standard distros on external storage (uSDCard/USB)
If you have a bootable storage device with a Linux distro installed, connected to USB or installed in MMCslot the board may not automatically boot into it. 

You have 2 options to boot into is: 

## Manual boot device selection: 
1. Interrupt u-boot by hitting any key
2. Run the command: 
   `run usb_boot`

## Update U-boot env variables 
1. Interrupt u-boot by hitting any key
2. Update the *boot* environment variables to either change the boot order or override the "boot" variable to "usb_boot"

This should start booting the linux kernel on the external uSDCard/USB drive. 





