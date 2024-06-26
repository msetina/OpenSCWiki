# Compiling and installing on Unix flavors

The preferred method for using OpenSC on Unix is to use the pre-packaged versions from operating system vendor or distribution (see [Linux Distributions](Linux-Distributions), for example). If the version is older than the current version, first try to convince the vendor to upgrade the packaged version, if that's not a possibility, OpenSC can be easily built from source on any modern Unix.

## Typical Installation

We suggest to install OpenSC into `/usr` and to put the configuration file into `/etc/opensc`. The default however would be `/usr/local` and `/usr/local/etc`, so you might want to change those. We suggest to configure and compile OpenSC like this:

```bash
tar xfvz opensc-*.tar.gz
cd opensc-*
./bootstrap
./configure --prefix=/usr --sysconfdir=/etc/opensc
make
sudo make install
```

## Build Requirements

* [PCSC-Lite](https://pcsclite.apdu.fr/) (runtime and development)
* [OpenSSL](https://www.openssl.org/) (optional, runtime and development)
* [OpenPACE](https://frankmorgner.github.io/openpace/) (optional, runtime and development)
* [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html) (optional, runtime and development)

### Installing Requirements on Debian/Ubuntu

```bash
sudo apt-get install pcscd libccid libpcsclite-dev libssl-dev libreadline-dev autoconf automake build-essential docbook-xsl xsltproc libtool pkg-config
```

If the certificates in the card are compressed, you also need zlib:

```bash
sudo apt-get install zlib1g-dev
```

If the driver for the card needs OpenPACE:

```bash
sudo apt-get install openpace libaec-dev
```

### Installing Requirements on Fedora

```bash
sudo dnf install readline-devel openssl-devel libxslt docbook-style-xsl pcsc-lite-devel automake autoconf libtool gcc
```

If the certificates in the card are compressed, you also need zlib:

```bash
sudo dnf install zlib-devel
```

If the driver for the card needs OpenPACE, follow [OpenPACE installation guide](https://frankmorgner.github.io/openpace/install.html).

## Build Configuration

OpenSC tries to auto-detect all libraries using the [pkg-config](https://www.freedesktop.org/wiki/Software/pkg-config/) system.

If you don't have `pkg-config` installed, and don't want to install it, you can use environment variables to tell configure, how to link with some library:

* `PCSC_CFLAGS` and `PCSC_LIBS` for PCSC-Lite
* `OPENSSL_CFLAGS` and `OPENSSL_LIBS` for OpenSSL
* `OPENPACE_CFLAGS` and `OPENPACE_LIBS` for OpenPACE
* `READLINE_CFLAGS` and `READLINE_LIBS` for GNU Readline

`./configure --help` will list other useful environment variables.

If some libraries are not installed in typical locations, you need to tell `pkg-config` where to find the `*.pc` files. You can do this with the @PKG_CONFIG_PATH@ environment variable, for example:

```bash
export PKG_CONFIG_PATH=/usr/lib/pkgconfig:/usr/local/lib/pkgconfig:/opt/mystuff/liv/pkgconfig
```

By default, we are compiling OpenSC with `-Wall -Wextra -Wno-unused-parameter -Werror`, which causes all compiler warnings to be treated as errors. Use `./configure --disable-strict` to disable this behavior.
