# SEGA Lindbergh Emulator

[![Lindbergh Loader CI](https://github.com/bobbydilley/lindbergh-loader/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/bobbydilley/lindbergh-loader/actions/workflows/ci.yml)

This project emulates the [SEGA Lindbergh](https://www.system16.com/hardware.php?id=731), allowing games to run on modern Linux computers with an NVIDIA graphics card to be used as replacement hardware for broken Lindbergh systems in physical arcade machines.

You can view the supported titles [here](docs/supported.md).

## Usage

Lindbergh games expect full control of the Linux OS (specifically, this emulator will need access to the input devices and serial devices on Linux). **With root privileges it is possible that they could cause damage to your computer.** Therefore before running this emulator you should add your user account to the following groups and then restart your computer:

```
sudo usermod -a -G dialout,input $USER
```

Additionally, please ensure your NVIDIA drivers are up-to-date before proceeding.

### Running

A `lindbergh` executable ([download a release](https://github.com/bobbydilley/lindbergh-loader/releases) or [build](#build)) is provided to easily run the games. Place it in the same directory as the game elf, and run `./lindbergh` to automatically start the game with the correct environment variables set, or run `./lindbergh -t` for test mode.

For example, copy the binaries into your game directory:

```sh
cp build/* ~/the-house-of-the-dead-4/disk0/elf/.
cd ~/the-house-of-the-dead-4/disk0/elf
```

Then run:

```sh
LD_PRELOAD=lindbergh.so LD_LIBRARY_PATH=$LD_LIBRARY_PATH:. ./hod4M.elf
```

Some games will require the extra libraries like `libposixtime.so`, which can be found in dumps of the Lindbergh CF image.

### Configuring

A default configuration file is provided in `docs/lindbergh.conf`. It should be placed in the same folder the game is run from. If no config file is present, the default setting will be used.

Currently, the default controls are set up for [The House of the Dead 4](https://en.wikipedia.org/wiki/The_House_of_the_Dead_4):

| Key           | Mapping        |
|---------------|----------------|
| `t`           | Test           |
| `s`           | Service        |
| `5`           | Coin 1         |
| `1`           | Player 1 Start |
| `Right Click` | Reload         |
| `Left Click`  | Shoot          |

## Build

You can either use GNU Make to build the binaries locally, use Docker, or simply download a [recent release](https://github.com/bobbydilley/lindbergh-loader/releases) directly from this repository.

Building this project has been tested to work on Ubuntu 22.04 and doesn't currently work on Ubuntu 23.10. Multiple packages such as `freeglut3:i386` are not available anymore on Debian Trixxie or Ubuntu 23.10.

### With Make

First, install the prerequisite dependencies below:

```sh
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install gcc-multilib
sudo apt install build-essential
sudo apt install freeglut3:i386
sudo apt install freeglut3-dev:i386
sudo apt install libglew-dev
sudo apt install xorg-dev
sudo apt install libopenal1:i386 libopenal-dev:i386
sudo apt install libalut-dev:i386 // You will need to find libalut-dev:i386, libalut0:i386 and multiarch-support:i386 from Ubuntu Xenial.
sudo apt install libxmu6:i386
sudo apt install libstdc++5:i386
```

Use `make` from within the root of this repository to build. Binaries are output to a `build` directory.

### With Docker

We use [`debian:bullseye`](https://hub.docker.com/_/debian) as our base image. With Docker installed, run (from the root of this repository):

```sh
docker build . -t lindbergh-loader --output=build
```

This will also output the executables to a `build` directory.

## License

The content of this repository is available under the GPLv3 Public License.
Please see the [LICENSE](/LICENSE) file in the root of this repository for more
information.

## Thanks

This project has been built by referencing earlier projects by Teknoparrot and JayFoxRox and from contributions by Doozer, Rolel and dkeruza-neo with extensive testing by Francesco - thanks to all of them!
