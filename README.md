# ARMSR Manifest for Build Development

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
