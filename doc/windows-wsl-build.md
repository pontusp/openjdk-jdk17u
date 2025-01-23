# OpenJDK build for Windows using WSL

Download WSL ubuntu image tar.gz from https://cloud-images.ubuntu.com/wsl/releases/24.04/current/

Create new WSL instance to build in

    ❯ wsl --import jdk-build instances\jdk-build images\ubuntu-noble-wsl-amd64-wsl.rootfs.tar.gz
    Import in progress, this may take a few minutes.
    The operation completed successfully.

    ❯ wsl -d jdk-build

    root$ useradd -G adm,dialout,cdrom,floppy,sudo,audio,dip,video,plugdev,users,netdev -m -s /bin/bash jdkbuilder
    root$ passwd jdkbuilder
    root$ cat << EOF >> /etc/wsl.conf
    >
    > [user]
    > default=jdkbuilder
    >
    > [automount]
    > options = case = off
    > EOF

    ❯ exit
    ❯ wsl --terminate jdk-build

    ❯ wsl -d jdk-build
    ❯ sudo apt-get update
    ❯ sudo apt-get upgrade 
    ❯ sudo apt-get install build-essential autoconf zip unzip

Install Visual Studio 2022 Community Edition from https://visualstudio.microsoft.com/downloads/

Prepare and build JDK from WSL

    ❯ wsl -d jdk-build
    ❯ mkdir -p /mnt/c/git/openjdk
    ❯ cd /mnt/c/git/openjdk
    ❯ git clone git@github.com:openjdk/jdk17u.git
    ❯ cd jdk17u/
    ❯ export JAVA_HOME=/mnt/c/jdk/jdk17.0.12_7/
    ❯ bash configure --with-num-cores=6 --with-version-opt=jdk17u.win.osmxbean.patch
    ❯ make images

    # From Windows CMD
    C:\git\openjdk\jdk17u>build\windows-x86_64-server-release\images\jdk\bin\java -version
    openjdk 17.0.14-internal 2025-01-21
    OpenJDK Runtime Environment (build 17.0.14-internal+0-jdk17u.win.osmxbean.patch)
    OpenJDK 64-Bit Server VM (build 17.0.14-internal+0-jdk17u.win.osmxbean.patch, mixed mode)






