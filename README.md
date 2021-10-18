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
$ mkdir <New_Dir>
$ cd <New_Dir>
$ repo init -u https://github.com/ankuraltran/ARMSRManifest -m ${TARGET}.xml [-b ${BRANCH}]
$ repo sync -j4 --no-clone-bundle
```

## 3. Get the Toolkits
```
$ cd build/
$ make -j2 toolchains
```

## 4. Build the target
```
$ make -j `nproc`
```
