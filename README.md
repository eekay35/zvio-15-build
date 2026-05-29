
Forked from https://github.com/zvaultio


# Building zVaultIO 15.0 from Scratch

Note: All these commands must be run as `root`.


## Requirements:

* Hardware

  * CPU: amd64-compatible 64-bit Intel or AMD CPU.
  * 16GB memory, or the equivalent in memory plus swap space
  * at least 80GB of free disk space

* Operating System

  * The build environment must be FreeBSD 15.x (or 15-STABLE) or higher


## Make Targets

* ```checkout``` creates a local working copy of the git repositories with
  ```git clone```

* ```update``` does a ```git pull``` to update the local working copy with
  any changes made to the git repositories since the last update

* ```release``` actually builds the zVaultIO release

* ```clean``` removes previously built files


## Procedure

* Install git
    ```
    pkg install -y git (or git-tiny)
    rehash
    ```

* Clone the build repository (```/usr/build``` is used for this example):

    ```
    git clone https://github.com/eekay35/zvio-15-build.git /usr/build
    ```

* Install Dependencies

    ```
    cd /usr/build
    make bootstrap-pkgs
    ```

* First-time checkout of source:

    ```
    make checkout zvault
    ```

* To build zVaultIO 15.0

    ```
    make release PROFILE=zvault PRODUCT=zVault Train=zVault-15
    ```

A zVaultIO release is built by first updating the source, then building:

```
make update
make release
```

To build the SDK version:

```
make update
make release BUILD_SDK=yes
```


Clean builds take a while, not just due to operating system builds, but
because poudriere has to build all of the ports. Later builds are faster,
only rebuilding files that need it.

Use ```make clean``` to remove all built files.


## Results

Built files are in the ```zvault/_BE``` subdirectory,
```/usr/build/zvault/_BE``` in this example.

ISO files: ```zvault/_BE/release/zVault-15-MASTER-{date}/x64/```.

Update files: ```zvault/_BE/release/```.

Log files: ```zvault/_BE/objs/logs/```.

## Current issues

* Need to get these in place for a successful port build as they are 
  un-fetchable without manual intervention:
    (Current sysutils/arcconf and sysutils/tw_cli ports)
  ```
  mkdir -p zvault/_BE/zvio-ports/distfiles/3dm2
  cp -a zvault/_BE/zvio-ports/distfiles/* zvault/_BE/objs/ports/distfiles/
  ```
* Errors at the moment:
