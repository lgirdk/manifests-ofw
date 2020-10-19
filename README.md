# Host system requirements

Manifests are provided to build under various versions of Open Embedded. The host system should meet the requirements for the corresponding Open Embedded release. The recommended manifest [ofw-2010-oe31.xml](https://github.com/lgirdk/manifests-ofw/blob/master/ofw-2010-oe31.xml) builds under OE 3.1 (Dunfell), for which the host system requirements are documented in the Yocto Project [Reference Manual](https://www.yoctoproject.org/docs/3.1.2/ref-manual/ref-manual.html#ref-manual-system-requirements).

Additionally, the host should have the Google "repo" tool installed. The older [repo-1](https://source.android.com/setup/develop#old-repo-python2) branch is currently recommended.

Both Python 2 and Python 3 should be installed and /usr/bin/python should run Python 2.

# Alternative manifests

The alternative manifests [ofw-2010-oe22.xml](https://github.com/lgirdk/manifests-ofw/blob/master/ofw-2010-oe22.xml) and [ofw-2010-oe30.xml](https://github.com/lgirdk/manifests-ofw/blob/master/ofw-2010-oe30.xml) are provided as a reference and to support a transition from older hosts. Both OE 2.2 and OE 3.0 may be built using Ubuntu 14.04, whereas OE 3.1 requires Ubuntu 16.04 or 18.04.

# Fetching and building the code

Each manifest contains its own specific setup and build instructions. For example, to build using OE 3.1:

```shell
repo init --repo-branch=repo-1 -u https://github.com/lgirdk/manifests-ofw.git -m ofw-2010-oe31.xml
repo sync --no-clone-bundle

MACHINE="exm-qemuarm" source ./meta-mng/setup-environment

bitbake mng-image-rdkb
```

The default and recommended build configuration uses gcc and glibc. Alternative configurations may be chosen by selecting an alternative machine configuration.

For example, to build with gcc and musl libc:

```shell
repo init --repo-branch=repo-1 -u https://github.com/lgirdk/manifests-ofw.git -m ofw-2010-oe31.xml
repo sync --no-clone-bundle

MACHINE="exm-qemuarm-musl" source ./meta-mng/setup-environment

bitbake mng-image-rdkb
```

To build with clang and glibc:

```shell
repo init --repo-branch=repo-1 -u https://github.com/lgirdk/manifests-ofw.git -m ofw-2010-oe31.xml
repo sync --no-clone-bundle

MACHINE="exm-qemuarm-clang" source ./meta-mng/setup-environment

bitbake mng-image-rdkb
```
