---
title: "Build and install GNSS-SDR"
permalink: /build-and-install/
excerpt: "How to quickly build and install GNSS-SDR in your system."
last_modified_at: 2022-07-31T11:13:02+02:00
header:
  teaser: "/assets/images/geniuss-building.png"
sidebar:
  nav: "start"
toc: true
toc_sticky: true
redirect_from:
  - /documentation/building-guide
  - /node/45
---

This page describes several ways to build and install GNSS-SDR.

![GeNiuSS building]({{ "/assets/images/geniuss-building.png" | relative_url }} "GeNiuSS at work. What a self-made character!"){:height="250px" width="250px"}
{: style="text-align: center;"}

# Installing everything through software package managers

## GNU/Linux distributions based on Debian

Starting from Debian 9 and Ubuntu 16.04, you can install GNSS-SDR just by doing:

```console
$ sudo apt-get install gnss-sdr
```

However, even on very recent distributions, the latest packaged GNSS-SDR version
is often at least months behind the latest developments, and the same applies to
its dependencies. In order to get access to the most recent features and bug
fixes, you might want to build the software from the source code.

**The good part:** This is the easiest and fastest way to get GNSS-SDR installed
in your system. Just a single line in your terminal will do all the work for
you.
{: .notice--success}

**The downside:** GNSS-SDR package updates need to undergo an acceptance process
before they are included in the different distributions, so it might not be the
ultimate version of the source code. The same applies to dependency libraries.
{: .notice--danger}

If everything went fine, you can directly jump into how to get your [first
position fix]({{ "/my-first-fix/" | relative_url }}). If your Operating System
release still does not include the GNSS-SDR package, you can
[build it from source](#source).

## macOS / Mac OS X using Macports

If you are using macOS 11 Big Sur (or Mac OS X 10.9 and above), and the
[Macports](https://www.macports.org/) package manager, GNSS-SDR can be installed
by typing in a Terminal:

```console
$ sudo port install gnss-sdr
```

This will install the latest stable release of GNSS-SDR.

{% capture mac-os-text %}
Instead of installing the latest stable release, you
can install the code found on the `next` branch, which might contain some bug
fixes and new features with respect to the latest stable release:

```console
$ sudo port install gnss-sdr-devel
```
{% endcapture %}

<div class="notice--success">
  <h4>You have another option here!</h4>
  {{ mac-os-text | markdownify }}
</div>

If everything went fine, you can directly jump into how to get your [first
position fix]({{ "/my-first-fix/" | relative_url }}). Or maybe you might prefer
other options, such as using [Homebrew](https://brew.sh/) instead of Macports as
a software package manager, or building the source code manually. For more
details of GNSS-SDR on macOS, please check the
[README.md](https://github.com/gnss-sdr/gnss-sdr/blob/main/README.md#macos-and-mac-os-x)
file.

# Building from source

<a name="source"></a>

**Is it better to compile from source or to install from a package?** Here we
provide some guidelines. The rule of thumb is: Always consider installing from
standard packages of your Operating System first; only compile from sources if
you know exactly why you need to do that.
{: .notice--info}

GNSS-SDR and its software dependencies can all be installed either by
downloading their source code, building and installing them in your system, or
by installing the corresponding software package. Under GNU/Linux, the package
manager may vary according to the distribution you are using (`.deb` for
Debian-based distros, such as Ubuntu, Linaro and Raspbian; `.rpm` for Fedora /
SUSE / Mandriva; etc.). In Mac OS X, the most popular open-source package
managers are Macports and Homebrew. In all cases, a possible option for a quick
jump into GNSS-SDR is to install all the dependencies from packages, and then
building GNSS-SDR from the source code.

But maybe this approach does not fit your needs. Maybe you already have some
dependency already built from source and want to use it, or your setup requires
some specific flag somewhere. This is a building-time _vs_. fine-tuning
trade-off. In order to take the adequate approach, just remember this basic
rule:

![Install package or build from source?]({{ "/assets/images/deb-or-build-from-source.png" | relative_url }}){: .align-center .invert-colors}
_Software packages require that all their dependencies must be also installed from packages._
{: style="text-align: center;"}

and take into account the following considerations:

**The good part:** The advantage of compiling from source is that you can
compile packages with certain flags/options which may be missing or disabled in
standard packages. Also, it makes it easy to have multiple versions of the same
program installed. Also, you can choose an exact version of a package that may
be already removed from, or not yet present in, your OS repositories.
{: .notice--success}

**The downside:** The disadvantage of compiling from source is that the usual
`cmake .. && make && sudo make install` procedure keeps your package manager
completely unaware of the changes you are making, so you are not going to get
automatic updates for the manually-compiled software, and it is possible
that package manager will later override/break your package if you are not
careful to install it in a separate location.
{: .notice--danger}

Some highly automated tools that can do some of the work for you are described
below.

## Install dependencies

### Install all dependencies using packages

If you are using Debian 8, Ubuntu 14.10 or above, this can be done by copying
and pasting the following line in a terminal:

```console
$ sudo apt-get install build-essential cmake git pkg-config libboost-dev \
   libboost-date-time-dev libboost-system-dev libboost-filesystem-dev \
   libboost-thread-dev libboost-chrono-dev libboost-serialization-dev \
   libboost-program-options-dev libboost-test-dev liblog4cpp5-dev \
   libuhd-dev gnuradio-dev gr-osmosdr libblas-dev liblapack-dev \
   libarmadillo-dev libgflags-dev libgoogle-glog-dev libhdf5-dev \
   libgnutls-openssl-dev libmatio-dev libpugixml-dev libpcap-dev \
   libprotobuf-dev protobuf-compiler libgtest-dev googletest \
   python3-mako python3-six
```

**Note for Ubuntu 14.04 LTS users:** you will need to build from source and
install GNU Radio manually, as explained below, since GNSS-SDR requires
gnuradio-dev >= 3.7.3, and Ubuntu 14.04 came with 3.7.2. Install all the
packages above BUT EXCEPT `libuhd-dev`, `gnuradio-dev` and `gr-osmosdr` (and
remove them if they are already installed in your machine), and install those
dependencies using PyBOMBS, as explained below. <br/><br/> **Note for Ubuntu
older than 16.04:** Packages `python3-mako` and `python3-six` must be replaced
by `python-mako` and `python-six`.
{: .notice--warning}

Once you have installed these packages, you can jump directly to
[clone, build and install GNSS-SDR](#build).

<i class="fab fa-linux fa-xl"></i> For other distributions (_e.g._ Arch Linux,
Debian, Fedora, openSUSE, Rocky Linux) please check the list of dependency
packages in the
[README.md](https://github.com/gnss-sdr/gnss-sdr/blob/next/README.md#alternative-1-install-dependencies-using-software-packages)
file.
{: .notice--info}

### Install dependencies using PyBOMBS

This option is adequate if you are interested in development, in working with
the most recent versions of software dependencies, want more fine-tuning on the
installed versions, or simply in building everything from the scratch just for
the fun of it. In such cases, we recommend using
[PyBOMBS](https://www.gnuradio.org/blog/2016-06-19-pybombs-the-what-the-how-and-the-why/ "PyBOMBS - The What, the How and the Why")
(Python Build Overlay Managed Bundle System), GNU Radio's meta-package manager
tool that installs software from source, or whatever the local package manager
is, that automatically does all the work for you. Please take a look at the
[configuration options and general PyBOMBS usage](https://github.com/gnuradio/pybombs).
Here we provide a quick step-by-step tutorial.

First of all, install some basic packages:

```console
$ sudo apt-get install git python3-pip
```

Download, build and install PyBOMBS:

```console
$ sudo pip3 install --upgrade git+https://github.com/gnuradio/pybombs.git
```

Apply a configuration:

```console
$ pybombs auto-config
```

Add list of default recipes (_i.e._, instructions on how to install software
dependencies):

```console
$ pybombs recipes add-defaults
```

Download, build and install GNU Radio, related drivers, and some other extra
modules into the directory `/path/to/prefix` (replace this path by your
preferred one, for instance `$HOME/sdr`):

```console
$ pybombs prefix init /path/to/prefix -a myprefix -R gnuradio-default
```

This will perform a local installation of the dependencies under
`/path/to/prefix`, so they will not be visible when opening a new terminal. In
order to make them available, you will need to set up the adequate environment
variables by sourcing the `setup_env.sh` script:

```console
$ cd /path/to/prefix
$ . ./setup_env.sh
```

Now you are ready to use GNU Radio and to jump into [building GNSS-SDR](#build)
after installing a few other dependencies. Actually, those are steps that
PyBOMBS can do for you as well:

```console
$ pybombs install gnss-sdr
```

By default, PyBOMBS installs the ‘next’ branch of GNSS-SDR development, which is
the most recent version of the source code. This behavior can be modified by
altering the corresponding recipe at
`$HOME/.pybombs/recipes/gr-recipes/gnss-sdr.lwr`

In case you do not want to use PyBOMBS and prefer to build and install GNSS-SDR
step by step (i.e., cloning the repository and doing the usual
`cmake .. && make && sudo make install` dance, as explained below), there are
still some missing dependencies (_i.e._, Armadillo, GFlags, Glog, GnuTLS and
Protocol Buffers) that can be installed either by using PyBOMBS:

```console
$ pybombs install armadillo gflags glog gnutls protobuf
```

or manually, just downloading, building and installing them. More details are
available in the
[README.md](https://github.com/gnss-sdr/gnss-sdr/blob/main/README.md#manual-installation-of-other-required-dependencies)
file.

## Clone, build and install GNSS-SDR

<a name="build"></a>

Once all the dependencies are installed in your system, you are ready to clone
the repository, build the source code and install the software in your system:

```console
$ git clone https://github.com/gnss-sdr/gnss-sdr
$ cd gnss-sdr/build
$ git checkout next
$ cmake ..
$ make
$ sudo make install
```

The step `git checkout next` is optional, and sets the source tree pointing to
the `next` branch, which is the most recent development snapshot. If this step
is omitted it takes the `master` branch by default, which contains the latest
stable release, and maybe some bug fixes.

In addition, CMake accepts a number of configuration options for your building
process. For instance, if you want to compile your source in "Debug" mode
instead of the default "Release", you can type:

```console
$ cmake -DCMAKE_BUILD_TYPE=Debug ..
```

and then CMake will add the debug flags appropriate for your compiler. There are
more options that can be of your interest, such as the addition of extra drivers
for RF front-ends not included by default, implementations of signal processing
blocks that take advantage of your GPU, addition of experimental features, the
building of optional QA code, and so on. Check out the available
[building configuration options]({{ "/docs/tutorials/configuration-options-building-time/" | relative_url }})
documentation for more details.

If everything went fine in the building process, now you can jump into how to
get your [first position fix]({{ "/my-first-fix/" | relative_url }}).

# Alternative tools

## Using Ninja

[Ninja](https://ninja-build.org/) is a small build system with a focus on speed,
that can be seen as a replacement for `make`. If the parameter `-GNinja` is
passed to CMake, it generates a `build.ninja` file (instead of a `Makefile`)
that is used by the Ninja build system to compile and link the source code.
Thus, after cloning the repository, the build workflow is:

```console
$ cd gnss-sdr/build
$ git checkout next
$ cmake -GNinja ..
$ ninja
$ sudo ninja install
```

In general, the compilation time when using Ninja is comparable to that when
using Make for a full build, although its performance is quite platform
dependent. Ninja is specially targeted to improve performance in large projects
and for incremental builds, so it seems to be a good replacement for Make
especially for developers who need to often recompile the source code. In
Debian-based GNU/Linux distributions, it can be installed by doing:

```console
$ sudo apt-get install ninja-build
```

On macOS, Ninja can be installed using Macports:

```console
$ sudo port install ninja
```

or Homebrew:

```console
$ brew install ninja
```

More information about Ninja usage can be found in the
[Ninja Manual](https://ninja-build.org/manual.html).

## Using Clang

[Clang](https://clang.llvm.org/) is a compiler front end for C, C++ and other
programming languages. It uses [LLVM](https://llvm.org/) as its back end, and it
is designed to be able to replace the full GNU Compiler
Collection ([GCC](https://gcc.gnu.org/)). Under macOS, this compiler is used by
default. In GNU/Linux, it can be used to build GNSS-SDR in replacement of GCC.

In Debian/Ubuntu-based distributions, Clang can be installed by doing:

```console
$ sudo apt-get install clang
```

Other packages specifying the Clang version, such as `clang-3.4`, `clang-3.8`,
`clang-4.0`, `clang-5.0`, ..., or `clang-12` could exist for your distribution,
check its documentation. Once installed, its use can be configured by passing
the following parameters to CMake:

```console
$ cmake -DCMAKE_CXX_COMPILER=/usr/bin/clang++-12 -DCMAKE_C_COMPILER=/usr/bin/clang-12 ..
```

of course replacing `12` by the actual version installed in your machine. Some
distributions drop the version number, so you may just have `/usr/bin/clang++`
and `/usr/bin/clang`.

If you have the Ninja build system installed, you can build GNSS-SDR replacing
GCC and `make` by Clang and Ninja:

```console
$ cmake -DCMAKE_CXX_COMPILER=/usr/bin/clang++-12 -DCMAKE_C_COMPILER=/usr/bin/clang-12 -GNinja ..
```

## Using Xcode

[Xcode](https://developer.apple.com/xcode/) is an integrated development
environment (IDE) for macOS containing a suite of software development tools
developed by Apple.

In order to build GNSS-SDR with Xcode, pass the following parameter to CMake:

```console
$ cmake -GXcode ..
```

This will create a `gnss-sdr.xcodeproj` project that can be opened by Xcode (in
the top bar menu, click File <i class="fas fa-long-arrow-alt-right"></i> Open
... <i class="fas fa-long-arrow-alt-right"></i> open the `gnss-sdr.xcodeproj`
project).

You can also build from the command line:

```console
$ xcodebuild
```

or

```console
$ xcodebuild -config Release
```

&nbsp;

# Other builds

## Snap package

<figure style="width: 64px" class="align-left">
  <img src="{{ "/assets/images/Snapcraft-logo-bird.png" | relative_url }}" alt="Snapcraft logo">
</figure>

[Snaps](https://snapcraft.io) are Linux packages aimed for Ubuntu or Ubuntu-like
distros. Snaps are self-contained packages that bundle the application and all
the libraries and runtimes it needs, and can be updated and reverted without
affecting the rest of the system. Snaps are confined from the OS and other apps
through security mechanisms, but can exchange content and functions with other
snaps.

Visit
[https://github.com/carlesfernandez/snapcraft-sandbox](https://github.com/carlesfernandez/snapcraft-sandbox)
for instructions on building your own snap package of GNSS-SDR, or install it
directly from the [Snap Store](https://snapcraft.io/gnss-sdr-next):

[![Get it from the Snap Store]({{ "/assets/images/snap-store-white.svg" | relative_url }})](https://snapcraft.io/gnss-sdr-next)
{: style="text-align: center;"}

## Docker

<figure style="width: 64px; height: 64px" class="align-left">
  <img src="{{ "/assets/images/docker_logo.png" | relative_url }}" alt="Docker logo">
</figure>

[Docker](https://www.docker.com) is an open-source tool designed to make
it easier to create, deploy, and run applications by using _containers_. Docker
containers wrap a piece of software in a complete filesystem that contains
everything needed to run: code, runtime, system tools and system libraries, and
ship it all out as one package. This guarantees that the software will always
run the same, regardless of any customized settings that the executing machine
might have that could differ from the machine used for writing and testing the
code. Using containers is another way of packing applications, in a much lighter
weight and with a much faster delivery model than using Virtual Machines.

Releases of Docker images with GNSS-SDR already installed are available from the
Docker Hub.

- An image based on Ubuntu with dependencies installed via .deb packages

  ```console
  $ docker run -it carlesfernandez/docker-gnsssdr
  ```

  The latest built from GNSS-SDR's `next` branch can be pulled as:

  ```console
  $ docker pull carlesfernandez/docker-gnsssdr:latest
  ```

- An image based on Ubuntu with dependencies built and installed via PyBOMBS:

  ```console
  $ docker run -it carlesfernandez/docker-pybombs-gnsssdr
  ```

  The latest built from GNSS-SDR's `next` branch can be pulled as:

  ```console
  $ docker pull carlesfernandez/docker-pybombs-gnsssdr:latest
  ```

Visit
[https://hub.docker.com/r/carlesfernandez/docker-gnsssdr](https://hub.docker.com/r/carlesfernandez/docker-gnsssdr)
or
[https://hub.docker.com/r/carlesfernandez/docker-pybombs-gnsssdr](https://hub.docker.com/r/carlesfernandez/docker-pybombs-gnsssdr)
for further instructions (how to share folders within the host machine and the
Docker image, how to set the graphical environment, etc.).

## GNSS-SDR in embedded platforms

<figure style="width: 64px" class="align-left">
  <img src="{{ "/assets/images/oe-logo.png" | relative_url }}" alt="Openembedded logo">
</figure>

We provide a Software Development Kit (SDK) based on
[OpenEmbedded](http://www.openembedded.org/wiki/Main_Page) for cross-compiling
GNSS-SDR in your desktop computer, producing executables that can run in
embedded platforms, such as a Zedboard or a Raspberry Pi 3. OpenEmbedded offers
a best-in-class cross-compile environment, allowing developers to create a
complete, custom GNU/Linux distribution for embedded systems.

Visit [Cross-compiling GNSS-SDR]({{ "/docs/tutorials/cross-compiling/" | relative_url }})
for instructions on cross-compiling GNSS-SDR for embedded systems.

-----

<link rel="prerender" href="{{ "/my-first-fix/" | relative_url }}" />
<link rel="prerender" href="{{ "/conf/" | relative_url }}" />
<link rel="prerender" href="{{ "/docs/" | relative_url }}" />
<link rel="prerender" href="{{ "/requirements/" | relative_url }}" />
<link rel="prerender" href="{{ "/quick-start-guide/" | relative_url }}" />
