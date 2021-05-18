# ARMSR Manifest for RK3399 EVB Target Setup

The information provided here provides the configuration and build setup for ARM System Ready compliance verification on Rockchip RK3399

There are three Evaluation Platforms being used as platform for the verification

| Target  | Manifest XML  | Device |
| :------------: |:---------------:| :-----:|
| Toy Brick      | Toybrick.xml | RK3399 Pro |
| Lenovo Leez     | default.xml        |  Leez-p710 |
| Pine 64 | RockPro64.xml |    RockPro 64 |


## 1. Install repo
Follow the instructions under the "Installing Repo" section
[here](https://source.android.com/source/downloading.html).

## 2. Get the source code
```
$ mkdir <New_Dir>
$ cd <New_Dir>
$ repo init -u https://github.com/ankuraltran/ARMSRManifest -m ${TARGET}.xml [-b ${BRANCH}]
$ repo sync -j4 --no-clone-bundle
```

## 3. Get the Toolkits
```
$ cd <New_Dir>/build/
$ make -j2 toolchains
```

## 4. Build the target
```
$ make -j `nproc`
```

## 5.Target Image Flashing Procedure 

The sequence will sync the atf(bl31), uboot(uboot.itb) to the local repo and make the images for the selected target. Currently the Image flashing is carried out manually using the rktool and is pending for automation at a later stage.
```
Install Android Tool v2.71
Address Name Path :
0x0000 Miniloader   — ../miniloader.bin
0x0000 Parameter    — ../paramter_toybrick.txt
0x0040 loader1      — ../idbloader.img
0x4000 loader2      — ../uboot.itb
0x8000 SCT          — ../sct.efi
```
