# Host system requirements

Manifests are provided to build under various versions of OpenEmbedded. The host system should meet the requirements for the corresponding OpenEmbedded release. The recommended manifest [oe40.xml](https://github.com/lgirdk/manifests-ofw/blob/ofw-2410er4/oe40.xml) builds under OE 4.0 ("[kirkstone](https://wiki.yoctoproject.org/wiki/Releases)"), for which the host system requirements are documented in the Yocto Project [Reference Manual](https://docs.yoctoproject.org/4.0.21/ref-manual/system-requirements.html).

```shell
sudo apt-get -y install gawk wget git diffstat unzip texinfo gcc build-essential \
  chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils \
  iputils-ping python3-git python3-jinja2 python3-subunit zstd liblz4-tool file \
  locales libacl1
sudo sudo locale-gen en_US.UTF-8
```

The host should have the Google "repo" tool installed. The older [repo-1](https://source.android.com/setup/develop#old-repo-python2) branch is currently recommended.

```shell
curl https://storage.googleapis.com/git-repo-downloads/repo-1 > ~/bin/repo
chmod a+x ~/bin/repo
```

# Fetching and building the code

Each manifest contains its own specific setup and build instructions. For example, to build using OE 4.0 see the instructions in the comments at the end of the [oe40.xml](https://github.com/lgirdk/manifests-ofw/blob/ofw-2410er4/oe40.xml) manifest:

```shell
repo init --repo-branch=repo-1 -u https://github.com/lgirdk/manifests-ofw.git -b ofw-2410er4 -m oe40.xml
repo sync --no-clone-bundle --current-branch

MACHINE="exm-qemuarm" source ./meta-mng/setup-environment

bitbake ofw
```

# Alternative manifests

Each manifest also has -open variant (e.g. [oe40-open.xml](https://github.com/lgirdk/manifests-ofw/blob/ofw-2410er4/oe40-open.xml)) which fetches only open source components and so can continue to be used without needing access to any private repos. Builds created with the -open manifests will not include the WebUI.

# Alternative builds

The default build configuration uses gcc and glibc. Alternative configurations based on clang or musl may be chosen by selecting an alternative machine configuration (the "repo init" command is the same in all cases).

For example, to build with gcc and musl libc:

```shell
repo init --repo-branch=repo-1 -u https://github.com/lgirdk/manifests-ofw.git -b ofw-2410er4 -m oe40.xml
repo sync --no-clone-bundle --current-branch

MACHINE="exm-qemuarm-musl" source ./meta-mng/setup-environment

bitbake ofw
```
