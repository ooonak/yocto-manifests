# My repo manifest files for Yocto projects

## Get repo

From the official documentation found here https://source.android.com/docs/setup/download.

```bash
$ export REPO=$(mktemp /tmp/repo.XXXXXXXXX)
$ curl -o ${REPO} https://storage.googleapis.com/git-repo-downloads/repo
$ gpg --recv-keys 8BB9AD793E8E6153AF0F9A4416530D5E920F5C65
$ curl -s https://storage.googleapis.com/git-repo-downloads/repo.asc | gpg --verify - ${REPO} && install -m 755 ${REPO} ~/bin/repo
```

## Usage
```bash
$ mkdir -p ~/yocto-env && cd ~/yocto-env
$ repo init -u https://github.com/ooonak/yocto-manifests.git -b main
Downloading Repo source from https://gerrit.googlesource.com/git-repo
...
repo has been initialized in /home/<user>/yocto-work/yocto-rpi-env

$ repo sync
Fetching: 100% (2/2), done in 25.448s
Checking out: 100% (2/2), done in 0.752s
repo sync has finished successfully.

$ tree -L 2
.
└── layers
    ├── meta-openembedded
    ├── meta-raspberrypi
    └── poky
```

## Baking

### Using CROPS
```bash
$ docker run --rm -it -v $HOME/yocto-work/yocto-rpi-env:/workdir crops/poky:debian-11 --workdir=/workdir
pokyuser@0ff769ddcb0a:/workdir$ ls
layers
pokyuser@0ff769ddcb0a:/workdir$ source layers/poky/oe-init-build-env
```

### Update configuration
```bash
# Add the layers you need.
$ vim conf/bblayers.conf
# Set the machine you build for.
$ vim conf/local.conf
```

### Bake
```bash
pokyuser@0ff769ddcb0a:/workdir/build$ bitbake core-image-base
```

