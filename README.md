# Host system requirements

Manifests are provided to build under various versions of OpenEmbedded. The host system should meet the requirements for the corresponding OpenEmbedded release. The recommended manifest [oe31.xml](https://github.com/lgirdk/manifests-ofw/blob/ofw-2112.10/oe31.xml) builds under OE 3.1 ("[dunfell](https://wiki.yoctoproject.org/wiki/Releases)"), for which the host system requirements are documented in the Yocto Project [Reference Manual](https://www.yoctoproject.org/docs/3.1.4/ref-manual/ref-manual.html#ref-manual-system-requirements).

```shell
sudo apt-get -y install gawk wget git-core diffstat unzip texinfo gcc-multilib \
  build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
  xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
  pylint3 xterm
```

The host should have the Google "repo" tool installed. The older [repo-1](https://source.android.com/setup/develop#old-repo-python2) branch is currently recommended.

```shell
curl https://storage.googleapis.com/git-repo-downloads/repo-1 > ~/bin/repo
chmod a+x ~/bin/repo
```

Both Python 2 and Python 3 should be installed and /usr/bin/python should run Python 2.

```shell
sudo apt-get -y install python2.7

/usr/bin/python --version
Python 2.7.12
```

# Fetching and building the code

Each manifest contains its own specific setup and build instructions. For example, to build using OE 3.1 see the instructions in the comments at the end of the [oe31.xml](https://github.com/lgirdk/manifests-ofw/blob/ofw-2112.10/oe31.xml) manifest:

```shell
repo init --repo-branch=repo-1 -u https://github.com/lgirdk/manifests-ofw.git -b ofw-2112.10 -m oe31.xml
repo sync --no-clone-bundle --current-branch

MACHINE="exm-qemuarm" source ./meta-mng/setup-environment

bitbake mng-image-rdkb
```

# Alternative manifests

The alternative manifests [oe22.xml](https://github.com/lgirdk/manifests-ofw/blob/ofw-2112.10/oe22.xml) and [oe30.xml](https://github.com/lgirdk/manifests-ofw/blob/ofw-2112.10/oe30.xml) are provided as a reference and to support a transition from older hosts. Both OE 2.2 ("[morty](https://wiki.yoctoproject.org/wiki/Releases)") and OE 3.0 ("[zeus](https://wiki.yoctoproject.org/wiki/Releases)") may be built using Ubuntu 14.04, whereas OE 3.1 requires Ubuntu 16.04 or 18.04.

The alternative manifest [oe32.xml](https://github.com/lgirdk/manifests-ofw/blob/ofw-2112.10/oe32.xml) is provided as a preview of OE 3.2 ("[gatesgarth](https://wiki.yoctoproject.org/wiki/Releases)") and requires Ubuntu 18.04 or 20.04.

Each manifest also has -open variant (e.g. [oe31-open.xml](https://github.com/lgirdk/manifests-ofw/blob/ofw-2112.10/oe31-open.xml)) which fetches only open source components and so can continue to be used without needing access to any private repos. Builds created with the -open manifests will not include the WebUI.

# Alternative builds

The default build configuration uses gcc and glibc. Alternative configurations may be chosen by selecting an alternative machine configuration (the "repo init" command is the same in all cases).

For example, to build with gcc and musl libc:

```shell
repo init --repo-branch=repo-1 -u https://github.com/lgirdk/manifests-ofw.git -b ofw-2112.10 -m oe31.xml
repo sync --no-clone-bundle --current-branch

MACHINE="exm-qemuarm-musl" source ./meta-mng/setup-environment

bitbake mng-image-rdkb
```

To build with clang and glibc:

```shell
repo init --repo-branch=repo-1 -u https://github.com/lgirdk/manifests-ofw.git -b ofw-2112.10 -m oe31.xml
repo sync --no-clone-bundle --current-branch

MACHINE="exm-qemuarm-clang" source ./meta-mng/setup-environment

bitbake mng-image-rdkb
```
