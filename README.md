## Laboratory work VIII

Данная лабораторная работа посвещена изучению систем автоматизации развёртывания и управления приложениями на примере **Docker**

```sh
$ open https://docs.docker.com/get-started/
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab08** на сервисе **GitHub**
- [ ] 2. Ознакомиться со ссылками учебного материала
- [ ] 3. Выполнить инструкцию учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
```

```
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab07 lab08
$ cd lab08
$ git submodule update --init
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08
```

```sh
$ cat > Dockerfile <<EOF
FROM ubuntu:18.04
EOF
```

```sh
$ cat >> Dockerfile <<EOF

RUN apt update
RUN apt install -yy gcc g++ cmake
EOF
```

```sh
$ cat >> Dockerfile <<EOF

COPY . print/
WORKDIR print
EOF
```

```sh
$ cat >> Dockerfile <<EOF

RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build
RUN cmake --build _build --target install
EOF
```

```sh
$ cat >> Dockerfile <<EOF

ENV LOG_PATH /home/logs/log.txt
EOF
```

```sh
$ cat >> Dockerfile <<EOF

VOLUME /home/logs
EOF
```

```sh
$ cat >> Dockerfile <<EOF

WORKDIR _install/bin
EOF
```

```sh
$ cat >> Dockerfile <<EOF

ENTRYPOINT ./demo
EOF
```

```sh
$ docker build -t logger .
```

<details><summary>Output</summary>
<p>
```sh
Sending build context to Docker daemon  3.575MB
Step 1/12 : FROM ubuntu:18.04
18.04: Pulling from library/ubuntu
4e43cebf9258: Pull complete 
Digest: sha256:14f1045816502e16fcbfc0b2a76747e9f5e40bc3899f8cfe20745abaafeaeab3
Status: Downloaded newer image for ubuntu:18.04
 ---> 97ba4bbc97fc
Step 2/12 : RUN apt update
 ---> Running in 2b9eb5d2e0d2

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Get:1 http://archive.ubuntu.com/ubuntu bionic InRelease [242 kB]
Get:2 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
Get:3 http://archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Get:4 http://archive.ubuntu.com/ubuntu bionic-backports InRelease [83.3 kB]
Get:5 http://archive.ubuntu.com/ubuntu bionic/main amd64 Packages [1344 kB]
Get:6 http://security.ubuntu.com/ubuntu bionic-security/main amd64 Packages [3332 kB]
Get:7 http://archive.ubuntu.com/ubuntu bionic/restricted amd64 Packages [13.5 kB]
Get:8 http://archive.ubuntu.com/ubuntu bionic/multiverse amd64 Packages [186 kB]
Get:9 http://security.ubuntu.com/ubuntu bionic-security/restricted amd64 Packages [1627 kB]
Get:10 http://archive.ubuntu.com/ubuntu bionic/universe amd64 Packages [11.3 MB]
Get:11 http://security.ubuntu.com/ubuntu bionic-security/universe amd64 Packages [1629 kB]
Get:12 http://security.ubuntu.com/ubuntu bionic-security/multiverse amd64 Packages [23.8 kB]
Get:13 http://archive.ubuntu.com/ubuntu bionic-updates/multiverse amd64 Packages [30.8 kB]
Get:14 http://archive.ubuntu.com/ubuntu bionic-updates/restricted amd64 Packages [1709 kB]
Get:15 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages [3771 kB]
Get:16 http://archive.ubuntu.com/ubuntu bionic-updates/universe amd64 Packages [2403 kB]
Get:17 http://archive.ubuntu.com/ubuntu bionic-backports/universe amd64 Packages [20.6 kB]
Get:18 http://archive.ubuntu.com/ubuntu bionic-backports/main amd64 Packages [64.0 kB]
Fetched 28.0 MB in 32s (872 kB/s)
Reading package lists...
Building dependency tree...
Reading state information...
6 packages can be upgraded. Run 'apt list --upgradable' to see them.
Removing intermediate container 2b9eb5d2e0d2
 ---> c636aa0a1acd
Step 3/12 : RUN apt install -yy gcc g++ cmake
 ---> Running in 42561aa83cca

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Reading package lists...
Building dependency tree...
Reading state information...
The following additional packages will be installed:
  binutils binutils-common binutils-x86-64-linux-gnu ca-certificates
  cmake-data cpp cpp-7 g++-7 gcc-7 gcc-7-base krb5-locales libarchive13
  libasan4 libasn1-8-heimdal libatomic1 libbinutils libc-dev-bin libc6-dev
  libcc1-0 libcilkrts5 libcurl4 libexpat1 libgcc-7-dev libgomp1
  libgssapi-krb5-2 libgssapi3-heimdal libhcrypto4-heimdal libheimbase1-heimdal
  libheimntlm0-heimdal libhx509-5-heimdal libicu60 libisl19 libitm1
  libjsoncpp1 libk5crypto3 libkeyutils1 libkrb5-26-heimdal libkrb5-3
  libkrb5support0 libldap-2.4-2 libldap-common liblsan0 liblzo2-2 libmpc3
  libmpfr6 libmpx2 libnghttp2-14 libpsl5 libquadmath0 librhash0
  libroken18-heimdal librtmp1 libsasl2-2 libsasl2-modules libsasl2-modules-db
  libsqlite3-0 libssl1.1 libstdc++-7-dev libtsan0 libubsan0 libuv1
  libwind0-heimdal libxml2 linux-libc-dev make manpages manpages-dev
  multiarch-support openssl publicsuffix
Suggested packages:
  binutils-doc cmake-doc ninja-build cpp-doc gcc-7-locales g++-multilib
  g++-7-multilib gcc-7-doc libstdc++6-7-dbg gcc-multilib autoconf automake
  libtool flex bison gdb gcc-doc gcc-7-multilib libgcc1-dbg libgomp1-dbg
  libitm1-dbg libatomic1-dbg libasan4-dbg liblsan0-dbg libtsan0-dbg
  libubsan0-dbg libcilkrts5-dbg libmpx2-dbg libquadmath0-dbg lrzip glibc-doc
  krb5-doc krb5-user libsasl2-modules-gssapi-mit
  | libsasl2-modules-gssapi-heimdal libsasl2-modules-ldap libsasl2-modules-otp
  libsasl2-modules-sql libstdc++-7-doc make-doc man-browser
The following NEW packages will be installed:
  binutils binutils-common binutils-x86-64-linux-gnu ca-certificates cmake
  cmake-data cpp cpp-7 g++ g++-7 gcc gcc-7 gcc-7-base krb5-locales
  libarchive13 libasan4 libasn1-8-heimdal libatomic1 libbinutils libc-dev-bin
  libc6-dev libcc1-0 libcilkrts5 libcurl4 libexpat1 libgcc-7-dev libgomp1
  libgssapi-krb5-2 libgssapi3-heimdal libhcrypto4-heimdal libheimbase1-heimdal
  libheimntlm0-heimdal libhx509-5-heimdal libicu60 libisl19 libitm1
  libjsoncpp1 libk5crypto3 libkeyutils1 libkrb5-26-heimdal libkrb5-3
  libkrb5support0 libldap-2.4-2 libldap-common liblsan0 liblzo2-2 libmpc3
  libmpfr6 libmpx2 libnghttp2-14 libpsl5 libquadmath0 librhash0
  libroken18-heimdal librtmp1 libsasl2-2 libsasl2-modules libsasl2-modules-db
  libsqlite3-0 libssl1.1 libstdc++-7-dev libtsan0 libubsan0 libuv1
  libwind0-heimdal libxml2 linux-libc-dev make manpages manpages-dev
  multiarch-support openssl publicsuffix
0 upgraded, 73 newly installed, 0 to remove and 6 not upgraded.
Need to get 62.0 MB of archives.
After this operation, 238 MB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 multiarch-support amd64 2.27-3ubuntu1.6 [6960 B]
Get:2 http://archive.ubuntu.com/ubuntu bionic/main amd64 liblzo2-2 amd64 2.08-1.2 [48.7 kB]
Get:3 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libssl1.1 amd64 1.1.1-1ubuntu2.1~18.04.23 [1303 kB]
Get:4 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 openssl amd64 1.1.1-1ubuntu2.1~18.04.23 [614 kB]
Get:5 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 ca-certificates all 20230311ubuntu0.18.04.1 [151 kB]
Get:6 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libexpat1 amd64 2.2.5-3ubuntu0.9 [82.8 kB]
Get:7 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libicu60 amd64 60.2-3ubuntu3.2 [8050 kB]
Get:8 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libsqlite3-0 amd64 3.22.0-1ubuntu0.7 [499 kB]
Get:9 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libxml2 amd64 2.9.4+dfsg1-6.1ubuntu1.9 [663 kB]
Get:10 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 krb5-locales all 1.16-2ubuntu0.4 [13.4 kB]
Get:11 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libkrb5support0 amd64 1.16-2ubuntu0.4 [30.9 kB]
Get:12 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libk5crypto3 amd64 1.16-2ubuntu0.4 [85.3 kB]
Get:13 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libkeyutils1 amd64 1.5.9-9.2ubuntu2.1 [8764 B]
Get:14 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libkrb5-3 amd64 1.16-2ubuntu0.4 [278 kB]
Get:15 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libgssapi-krb5-2 amd64 1.16-2ubuntu0.4 [122 kB]
Get:16 http://archive.ubuntu.com/ubuntu bionic/main amd64 libpsl5 amd64 0.19.1-5build1 [41.8 kB]
Get:17 http://archive.ubuntu.com/ubuntu bionic/main amd64 manpages all 4.15-1 [1234 kB]
Get:18 http://archive.ubuntu.com/ubuntu bionic/main amd64 publicsuffix all 20180223.1310-1 [97.6 kB]
Get:19 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 binutils-common amd64 2.30-21ubuntu1~18.04.9 [197 kB]
Get:20 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libbinutils amd64 2.30-21ubuntu1~18.04.9 [489 kB]
Get:21 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 binutils-x86-64-linux-gnu amd64 2.30-21ubuntu1~18.04.9 [1839 kB]
Get:22 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 binutils amd64 2.30-21ubuntu1~18.04.9 [3392 B]
Get:23 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 cmake-data all 3.10.2-1ubuntu2.18.04.2 [1332 kB]
Get:24 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libarchive13 amd64 3.2.2-3.1ubuntu0.7 [288 kB]
Get:25 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libroken18-heimdal amd64 7.5.0+dfsg-1ubuntu0.4 [42.3 kB]
Get:26 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libasn1-8-heimdal amd64 7.5.0+dfsg-1ubuntu0.4 [175 kB]
Get:27 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libheimbase1-heimdal amd64 7.5.0+dfsg-1ubuntu0.4 [30.3 kB]
Get:28 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libhcrypto4-heimdal amd64 7.5.0+dfsg-1ubuntu0.4 [85.9 kB]
Get:29 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libwind0-heimdal amd64 7.5.0+dfsg-1ubuntu0.4 [48.0 kB]
Get:30 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libhx509-5-heimdal amd64 7.5.0+dfsg-1ubuntu0.4 [107 kB]
Get:31 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libkrb5-26-heimdal amd64 7.5.0+dfsg-1ubuntu0.4 [207 kB]
Get:32 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libheimntlm0-heimdal amd64 7.5.0+dfsg-1ubuntu0.4 [14.8 kB]
Get:33 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libgssapi3-heimdal amd64 7.5.0+dfsg-1ubuntu0.4 [96.7 kB]
Get:34 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libsasl2-modules-db amd64 2.1.27~101-g0780600+dfsg-3ubuntu2.4 [15.0 kB]
Get:35 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libsasl2-2 amd64 2.1.27~101-g0780600+dfsg-3ubuntu2.4 [49.2 kB]
Get:36 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libldap-common all 2.4.45+dfsg-1ubuntu1.11 [15.8 kB]
Get:37 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libldap-2.4-2 amd64 2.4.45+dfsg-1ubuntu1.11 [154 kB]
Get:38 http://archive.ubuntu.com/ubuntu bionic/main amd64 libnghttp2-14 amd64 1.30.0-1ubuntu1 [77.8 kB]
Get:39 http://archive.ubuntu.com/ubuntu bionic/main amd64 librtmp1 amd64 2.4+20151223.gitfa8646d.1-1 [54.2 kB]
Get:40 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libcurl4 amd64 7.58.0-2ubuntu3.24 [221 kB]
Get:41 http://archive.ubuntu.com/ubuntu bionic/main amd64 libjsoncpp1 amd64 1.7.4-3 [73.6 kB]
Get:42 http://archive.ubuntu.com/ubuntu bionic/main amd64 librhash0 amd64 1.3.6-2 [78.1 kB]
Get:43 http://archive.ubuntu.com/ubuntu bionic/main amd64 libuv1 amd64 1.18.0-3 [64.4 kB]
Get:44 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 cmake amd64 3.10.2-1ubuntu2.18.04.2 [3152 kB]
Get:45 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 gcc-7-base amd64 7.5.0-3ubuntu1~18.04 [18.3 kB]
Get:46 http://archive.ubuntu.com/ubuntu bionic/main amd64 libisl19 amd64 0.19-1 [551 kB]
Get:47 http://archive.ubuntu.com/ubuntu bionic/main amd64 libmpfr6 amd64 4.0.1-1 [243 kB]
Get:48 http://archive.ubuntu.com/ubuntu bionic/main amd64 libmpc3 amd64 1.1.0-1 [40.8 kB]
Get:49 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 cpp-7 amd64 7.5.0-3ubuntu1~18.04 [8591 kB]
Get:50 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 cpp amd64 4:7.4.0-1ubuntu2.3 [27.7 kB]
Get:51 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libcc1-0 amd64 8.4.0-1ubuntu1~18.04 [39.4 kB]
Get:52 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libgomp1 amd64 8.4.0-1ubuntu1~18.04 [76.5 kB]
Get:53 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libitm1 amd64 8.4.0-1ubuntu1~18.04 [27.9 kB]
Get:54 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libatomic1 amd64 8.4.0-1ubuntu1~18.04 [9192 B]
Get:55 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libasan4 amd64 7.5.0-3ubuntu1~18.04 [358 kB]
Get:56 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 liblsan0 amd64 8.4.0-1ubuntu1~18.04 [133 kB]
Get:57 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libtsan0 amd64 8.4.0-1ubuntu1~18.04 [288 kB]
Get:58 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libubsan0 amd64 7.5.0-3ubuntu1~18.04 [126 kB]
Get:59 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libcilkrts5 amd64 7.5.0-3ubuntu1~18.04 [42.5 kB]
Get:60 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libmpx2 amd64 8.4.0-1ubuntu1~18.04 [11.6 kB]
Get:61 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libquadmath0 amd64 8.4.0-1ubuntu1~18.04 [134 kB]
Get:62 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libgcc-7-dev amd64 7.5.0-3ubuntu1~18.04 [2378 kB]
Get:63 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 gcc-7 amd64 7.5.0-3ubuntu1~18.04 [9381 kB]
Get:64 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 gcc amd64 4:7.4.0-1ubuntu2.3 [5184 B]
Get:65 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libc-dev-bin amd64 2.27-3ubuntu1.6 [71.9 kB]
Get:66 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 linux-libc-dev amd64 4.15.0-212.223 [991 kB]
Get:67 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libc6-dev amd64 2.27-3ubuntu1.6 [2587 kB]
Get:68 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libstdc++-7-dev amd64 7.5.0-3ubuntu1~18.04 [1471 kB]
Get:69 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 g++-7 amd64 7.5.0-3ubuntu1~18.04 [9697 kB]
Get:70 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 g++ amd64 4:7.4.0-1ubuntu2.3 [1568 B]
Get:71 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libsasl2-modules amd64 2.1.27~101-g0780600+dfsg-3ubuntu2.4 [48.9 kB]
Get:72 http://archive.ubuntu.com/ubuntu bionic/main amd64 make amd64 4.1-9.1ubuntu1 [154 kB]
Get:73 http://archive.ubuntu.com/ubuntu bionic/main amd64 manpages-dev all 4.15-1 [2217 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 62.0 MB in 16s (3929 kB/s)
Selecting previously unselected package multiarch-support.
(Reading database ... 4050 files and directories currently installed.)
Preparing to unpack .../multiarch-support_2.27-3ubuntu1.6_amd64.deb ...
Unpacking multiarch-support (2.27-3ubuntu1.6) ...
Setting up multiarch-support (2.27-3ubuntu1.6) ...
Selecting previously unselected package liblzo2-2:amd64.
(Reading database ... 4053 files and directories currently installed.)
Preparing to unpack .../00-liblzo2-2_2.08-1.2_amd64.deb ...
Unpacking liblzo2-2:amd64 (2.08-1.2) ...
Selecting previously unselected package libssl1.1:amd64.
Preparing to unpack .../01-libssl1.1_1.1.1-1ubuntu2.1~18.04.23_amd64.deb ...
Unpacking libssl1.1:amd64 (1.1.1-1ubuntu2.1~18.04.23) ...
Selecting previously unselected package openssl.
Preparing to unpack .../02-openssl_1.1.1-1ubuntu2.1~18.04.23_amd64.deb ...
Unpacking openssl (1.1.1-1ubuntu2.1~18.04.23) ...
Selecting previously unselected package ca-certificates.
Preparing to unpack .../03-ca-certificates_20230311ubuntu0.18.04.1_all.deb ...
Unpacking ca-certificates (20230311ubuntu0.18.04.1) ...
Selecting previously unselected package libexpat1:amd64.
Preparing to unpack .../04-libexpat1_2.2.5-3ubuntu0.9_amd64.deb ...
Unpacking libexpat1:amd64 (2.2.5-3ubuntu0.9) ...
Selecting previously unselected package libicu60:amd64.
Preparing to unpack .../05-libicu60_60.2-3ubuntu3.2_amd64.deb ...
Unpacking libicu60:amd64 (60.2-3ubuntu3.2) ...
Selecting previously unselected package libsqlite3-0:amd64.
Preparing to unpack .../06-libsqlite3-0_3.22.0-1ubuntu0.7_amd64.deb ...
Unpacking libsqlite3-0:amd64 (3.22.0-1ubuntu0.7) ...
Selecting previously unselected package libxml2:amd64.
Preparing to unpack .../07-libxml2_2.9.4+dfsg1-6.1ubuntu1.9_amd64.deb ...
Unpacking libxml2:amd64 (2.9.4+dfsg1-6.1ubuntu1.9) ...
Selecting previously unselected package krb5-locales.
Preparing to unpack .../08-krb5-locales_1.16-2ubuntu0.4_all.deb ...
Unpacking krb5-locales (1.16-2ubuntu0.4) ...
Selecting previously unselected package libkrb5support0:amd64.
Preparing to unpack .../09-libkrb5support0_1.16-2ubuntu0.4_amd64.deb ...
Unpacking libkrb5support0:amd64 (1.16-2ubuntu0.4) ...
Selecting previously unselected package libk5crypto3:amd64.
Preparing to unpack .../10-libk5crypto3_1.16-2ubuntu0.4_amd64.deb ...
Unpacking libk5crypto3:amd64 (1.16-2ubuntu0.4) ...
Selecting previously unselected package libkeyutils1:amd64.
Preparing to unpack .../11-libkeyutils1_1.5.9-9.2ubuntu2.1_amd64.deb ...
Unpacking libkeyutils1:amd64 (1.5.9-9.2ubuntu2.1) ...
Selecting previously unselected package libkrb5-3:amd64.
Preparing to unpack .../12-libkrb5-3_1.16-2ubuntu0.4_amd64.deb ...
Unpacking libkrb5-3:amd64 (1.16-2ubuntu0.4) ...
Selecting previously unselected package libgssapi-krb5-2:amd64.
Preparing to unpack .../13-libgssapi-krb5-2_1.16-2ubuntu0.4_amd64.deb ...
Unpacking libgssapi-krb5-2:amd64 (1.16-2ubuntu0.4) ...
Selecting previously unselected package libpsl5:amd64.
Preparing to unpack .../14-libpsl5_0.19.1-5build1_amd64.deb ...
Unpacking libpsl5:amd64 (0.19.1-5build1) ...
Selecting previously unselected package manpages.
Preparing to unpack .../15-manpages_4.15-1_all.deb ...
Unpacking manpages (4.15-1) ...
Selecting previously unselected package publicsuffix.
Preparing to unpack .../16-publicsuffix_20180223.1310-1_all.deb ...
Unpacking publicsuffix (20180223.1310-1) ...
Selecting previously unselected package binutils-common:amd64.
Preparing to unpack .../17-binutils-common_2.30-21ubuntu1~18.04.9_amd64.deb ...
Unpacking binutils-common:amd64 (2.30-21ubuntu1~18.04.9) ...
Selecting previously unselected package libbinutils:amd64.
Preparing to unpack .../18-libbinutils_2.30-21ubuntu1~18.04.9_amd64.deb ...
Unpacking libbinutils:amd64 (2.30-21ubuntu1~18.04.9) ...
Selecting previously unselected package binutils-x86-64-linux-gnu.
Preparing to unpack .../19-binutils-x86-64-linux-gnu_2.30-21ubuntu1~18.04.9_amd64.deb ...
Unpacking binutils-x86-64-linux-gnu (2.30-21ubuntu1~18.04.9) ...
Selecting previously unselected package binutils.
Preparing to unpack .../20-binutils_2.30-21ubuntu1~18.04.9_amd64.deb ...
Unpacking binutils (2.30-21ubuntu1~18.04.9) ...
Selecting previously unselected package cmake-data.
Preparing to unpack .../21-cmake-data_3.10.2-1ubuntu2.18.04.2_all.deb ...
Unpacking cmake-data (3.10.2-1ubuntu2.18.04.2) ...
Selecting previously unselected package libarchive13:amd64.
Preparing to unpack .../22-libarchive13_3.2.2-3.1ubuntu0.7_amd64.deb ...
Unpacking libarchive13:amd64 (3.2.2-3.1ubuntu0.7) ...
Selecting previously unselected package libroken18-heimdal:amd64.
Preparing to unpack .../23-libroken18-heimdal_7.5.0+dfsg-1ubuntu0.4_amd64.deb ...
Unpacking libroken18-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Selecting previously unselected package libasn1-8-heimdal:amd64.
Preparing to unpack .../24-libasn1-8-heimdal_7.5.0+dfsg-1ubuntu0.4_amd64.deb ...
Unpacking libasn1-8-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Selecting previously unselected package libheimbase1-heimdal:amd64.
Preparing to unpack .../25-libheimbase1-heimdal_7.5.0+dfsg-1ubuntu0.4_amd64.deb ...
Unpacking libheimbase1-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Selecting previously unselected package libhcrypto4-heimdal:amd64.
Preparing to unpack .../26-libhcrypto4-heimdal_7.5.0+dfsg-1ubuntu0.4_amd64.deb ...
Unpacking libhcrypto4-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Selecting previously unselected package libwind0-heimdal:amd64.
Preparing to unpack .../27-libwind0-heimdal_7.5.0+dfsg-1ubuntu0.4_amd64.deb ...
Unpacking libwind0-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Selecting previously unselected package libhx509-5-heimdal:amd64.
Preparing to unpack .../28-libhx509-5-heimdal_7.5.0+dfsg-1ubuntu0.4_amd64.deb ...
Unpacking libhx509-5-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Selecting previously unselected package libkrb5-26-heimdal:amd64.
Preparing to unpack .../29-libkrb5-26-heimdal_7.5.0+dfsg-1ubuntu0.4_amd64.deb ...
Unpacking libkrb5-26-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Selecting previously unselected package libheimntlm0-heimdal:amd64.
Preparing to unpack .../30-libheimntlm0-heimdal_7.5.0+dfsg-1ubuntu0.4_amd64.deb ...
Unpacking libheimntlm0-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Selecting previously unselected package libgssapi3-heimdal:amd64.
Preparing to unpack .../31-libgssapi3-heimdal_7.5.0+dfsg-1ubuntu0.4_amd64.deb ...
Unpacking libgssapi3-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Selecting previously unselected package libsasl2-modules-db:amd64.
Preparing to unpack .../32-libsasl2-modules-db_2.1.27~101-g0780600+dfsg-3ubuntu2.4_amd64.deb ...
Unpacking libsasl2-modules-db:amd64 (2.1.27~101-g0780600+dfsg-3ubuntu2.4) ...
Selecting previously unselected package libsasl2-2:amd64.
Preparing to unpack .../33-libsasl2-2_2.1.27~101-g0780600+dfsg-3ubuntu2.4_amd64.deb ...
Unpacking libsasl2-2:amd64 (2.1.27~101-g0780600+dfsg-3ubuntu2.4) ...
Selecting previously unselected package libldap-common.
Preparing to unpack .../34-libldap-common_2.4.45+dfsg-1ubuntu1.11_all.deb ...
Unpacking libldap-common (2.4.45+dfsg-1ubuntu1.11) ...
Selecting previously unselected package libldap-2.4-2:amd64.
Preparing to unpack .../35-libldap-2.4-2_2.4.45+dfsg-1ubuntu1.11_amd64.deb ...
Unpacking libldap-2.4-2:amd64 (2.4.45+dfsg-1ubuntu1.11) ...
Selecting previously unselected package libnghttp2-14:amd64.
Preparing to unpack .../36-libnghttp2-14_1.30.0-1ubuntu1_amd64.deb ...
Unpacking libnghttp2-14:amd64 (1.30.0-1ubuntu1) ...
Selecting previously unselected package librtmp1:amd64.
Preparing to unpack .../37-librtmp1_2.4+20151223.gitfa8646d.1-1_amd64.deb ...
Unpacking librtmp1:amd64 (2.4+20151223.gitfa8646d.1-1) ...
Selecting previously unselected package libcurl4:amd64.
Preparing to unpack .../38-libcurl4_7.58.0-2ubuntu3.24_amd64.deb ...
Unpacking libcurl4:amd64 (7.58.0-2ubuntu3.24) ...
Selecting previously unselected package libjsoncpp1:amd64.
Preparing to unpack .../39-libjsoncpp1_1.7.4-3_amd64.deb ...
Unpacking libjsoncpp1:amd64 (1.7.4-3) ...
Selecting previously unselected package librhash0:amd64.
Preparing to unpack .../40-librhash0_1.3.6-2_amd64.deb ...
Unpacking librhash0:amd64 (1.3.6-2) ...
Selecting previously unselected package libuv1:amd64.
Preparing to unpack .../41-libuv1_1.18.0-3_amd64.deb ...
Unpacking libuv1:amd64 (1.18.0-3) ...
Selecting previously unselected package cmake.
Preparing to unpack .../42-cmake_3.10.2-1ubuntu2.18.04.2_amd64.deb ...
Unpacking cmake (3.10.2-1ubuntu2.18.04.2) ...
Selecting previously unselected package gcc-7-base:amd64.
Preparing to unpack .../43-gcc-7-base_7.5.0-3ubuntu1~18.04_amd64.deb ...
Unpacking gcc-7-base:amd64 (7.5.0-3ubuntu1~18.04) ...
Selecting previously unselected package libisl19:amd64.
Preparing to unpack .../44-libisl19_0.19-1_amd64.deb ...
Unpacking libisl19:amd64 (0.19-1) ...
Selecting previously unselected package libmpfr6:amd64.
Preparing to unpack .../45-libmpfr6_4.0.1-1_amd64.deb ...
Unpacking libmpfr6:amd64 (4.0.1-1) ...
Selecting previously unselected package libmpc3:amd64.
Preparing to unpack .../46-libmpc3_1.1.0-1_amd64.deb ...
Unpacking libmpc3:amd64 (1.1.0-1) ...
Selecting previously unselected package cpp-7.
Preparing to unpack .../47-cpp-7_7.5.0-3ubuntu1~18.04_amd64.deb ...
Unpacking cpp-7 (7.5.0-3ubuntu1~18.04) ...
Selecting previously unselected package cpp.
Preparing to unpack .../48-cpp_4%3a7.4.0-1ubuntu2.3_amd64.deb ...
Unpacking cpp (4:7.4.0-1ubuntu2.3) ...
Selecting previously unselected package libcc1-0:amd64.
Preparing to unpack .../49-libcc1-0_8.4.0-1ubuntu1~18.04_amd64.deb ...
Unpacking libcc1-0:amd64 (8.4.0-1ubuntu1~18.04) ...
Selecting previously unselected package libgomp1:amd64.
Preparing to unpack .../50-libgomp1_8.4.0-1ubuntu1~18.04_amd64.deb ...
Unpacking libgomp1:amd64 (8.4.0-1ubuntu1~18.04) ...
Selecting previously unselected package libitm1:amd64.
Preparing to unpack .../51-libitm1_8.4.0-1ubuntu1~18.04_amd64.deb ...
Unpacking libitm1:amd64 (8.4.0-1ubuntu1~18.04) ...
Selecting previously unselected package libatomic1:amd64.
Preparing to unpack .../52-libatomic1_8.4.0-1ubuntu1~18.04_amd64.deb ...
Unpacking libatomic1:amd64 (8.4.0-1ubuntu1~18.04) ...
Selecting previously unselected package libasan4:amd64.
Preparing to unpack .../53-libasan4_7.5.0-3ubuntu1~18.04_amd64.deb ...
Unpacking libasan4:amd64 (7.5.0-3ubuntu1~18.04) ...
Selecting previously unselected package liblsan0:amd64.
Preparing to unpack .../54-liblsan0_8.4.0-1ubuntu1~18.04_amd64.deb ...
Unpacking liblsan0:amd64 (8.4.0-1ubuntu1~18.04) ...
Selecting previously unselected package libtsan0:amd64.
Preparing to unpack .../55-libtsan0_8.4.0-1ubuntu1~18.04_amd64.deb ...
Unpacking libtsan0:amd64 (8.4.0-1ubuntu1~18.04) ...
Selecting previously unselected package libubsan0:amd64.
Preparing to unpack .../56-libubsan0_7.5.0-3ubuntu1~18.04_amd64.deb ...
Unpacking libubsan0:amd64 (7.5.0-3ubuntu1~18.04) ...
Selecting previously unselected package libcilkrts5:amd64.
Preparing to unpack .../57-libcilkrts5_7.5.0-3ubuntu1~18.04_amd64.deb ...
Unpacking libcilkrts5:amd64 (7.5.0-3ubuntu1~18.04) ...
Selecting previously unselected package libmpx2:amd64.
Preparing to unpack .../58-libmpx2_8.4.0-1ubuntu1~18.04_amd64.deb ...
Unpacking libmpx2:amd64 (8.4.0-1ubuntu1~18.04) ...
Selecting previously unselected package libquadmath0:amd64.
Preparing to unpack .../59-libquadmath0_8.4.0-1ubuntu1~18.04_amd64.deb ...
Unpacking libquadmath0:amd64 (8.4.0-1ubuntu1~18.04) ...
Selecting previously unselected package libgcc-7-dev:amd64.
Preparing to unpack .../60-libgcc-7-dev_7.5.0-3ubuntu1~18.04_amd64.deb ...
Unpacking libgcc-7-dev:amd64 (7.5.0-3ubuntu1~18.04) ...
Selecting previously unselected package gcc-7.
Preparing to unpack .../61-gcc-7_7.5.0-3ubuntu1~18.04_amd64.deb ...
Unpacking gcc-7 (7.5.0-3ubuntu1~18.04) ...
Selecting previously unselected package gcc.
Preparing to unpack .../62-gcc_4%3a7.4.0-1ubuntu2.3_amd64.deb ...
Unpacking gcc (4:7.4.0-1ubuntu2.3) ...
Selecting previously unselected package libc-dev-bin.
Preparing to unpack .../63-libc-dev-bin_2.27-3ubuntu1.6_amd64.deb ...
Unpacking libc-dev-bin (2.27-3ubuntu1.6) ...
Selecting previously unselected package linux-libc-dev:amd64.
Preparing to unpack .../64-linux-libc-dev_4.15.0-212.223_amd64.deb ...
Unpacking linux-libc-dev:amd64 (4.15.0-212.223) ...
Selecting previously unselected package libc6-dev:amd64.
Preparing to unpack .../65-libc6-dev_2.27-3ubuntu1.6_amd64.deb ...
Unpacking libc6-dev:amd64 (2.27-3ubuntu1.6) ...
Selecting previously unselected package libstdc++-7-dev:amd64.
Preparing to unpack .../66-libstdc++-7-dev_7.5.0-3ubuntu1~18.04_amd64.deb ...
Unpacking libstdc++-7-dev:amd64 (7.5.0-3ubuntu1~18.04) ...
Selecting previously unselected package g++-7.
Preparing to unpack .../67-g++-7_7.5.0-3ubuntu1~18.04_amd64.deb ...
Unpacking g++-7 (7.5.0-3ubuntu1~18.04) ...
Selecting previously unselected package g++.
Preparing to unpack .../68-g++_4%3a7.4.0-1ubuntu2.3_amd64.deb ...
Unpacking g++ (4:7.4.0-1ubuntu2.3) ...
Selecting previously unselected package libsasl2-modules:amd64.
Preparing to unpack .../69-libsasl2-modules_2.1.27~101-g0780600+dfsg-3ubuntu2.4_amd64.deb ...
Unpacking libsasl2-modules:amd64 (2.1.27~101-g0780600+dfsg-3ubuntu2.4) ...
Selecting previously unselected package make.
Preparing to unpack .../70-make_4.1-9.1ubuntu1_amd64.deb ...
Unpacking make (4.1-9.1ubuntu1) ...
Selecting previously unselected package manpages-dev.
Preparing to unpack .../71-manpages-dev_4.15-1_all.deb ...
Unpacking manpages-dev (4.15-1) ...
Setting up libquadmath0:amd64 (8.4.0-1ubuntu1~18.04) ...
Setting up libgomp1:amd64 (8.4.0-1ubuntu1~18.04) ...
Setting up libatomic1:amd64 (8.4.0-1ubuntu1~18.04) ...
Setting up manpages (4.15-1) ...
Setting up libexpat1:amd64 (2.2.5-3ubuntu0.9) ...
Setting up libicu60:amd64 (60.2-3ubuntu3.2) ...
Setting up libcc1-0:amd64 (8.4.0-1ubuntu1~18.04) ...
Setting up make (4.1-9.1ubuntu1) ...
Setting up libnghttp2-14:amd64 (1.30.0-1ubuntu1) ...
Setting up libldap-common (2.4.45+dfsg-1ubuntu1.11) ...
Setting up libuv1:amd64 (1.18.0-3) ...
Setting up libpsl5:amd64 (0.19.1-5build1) ...
Setting up libtsan0:amd64 (8.4.0-1ubuntu1~18.04) ...
Setting up libsasl2-modules-db:amd64 (2.1.27~101-g0780600+dfsg-3ubuntu2.4) ...
Setting up linux-libc-dev:amd64 (4.15.0-212.223) ...
Setting up libmpfr6:amd64 (4.0.1-1) ...
Setting up libsasl2-2:amd64 (2.1.27~101-g0780600+dfsg-3ubuntu2.4) ...
Setting up cmake-data (3.10.2-1ubuntu2.18.04.2) ...
Setting up libroken18-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Setting up librtmp1:amd64 (2.4+20151223.gitfa8646d.1-1) ...
Setting up libkrb5support0:amd64 (1.16-2ubuntu0.4) ...
Setting up libxml2:amd64 (2.9.4+dfsg1-6.1ubuntu1.9) ...
Setting up librhash0:amd64 (1.3.6-2) ...
Setting up liblsan0:amd64 (8.4.0-1ubuntu1~18.04) ...
Setting up gcc-7-base:amd64 (7.5.0-3ubuntu1~18.04) ...
Setting up binutils-common:amd64 (2.30-21ubuntu1~18.04.9) ...
Setting up libmpx2:amd64 (8.4.0-1ubuntu1~18.04) ...
Setting up krb5-locales (1.16-2ubuntu0.4) ...
Setting up publicsuffix (20180223.1310-1) ...
Setting up libssl1.1:amd64 (1.1.1-1ubuntu2.1~18.04.23) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.26.1 /usr/local/share/perl/5.26.1 /usr/lib/x86_64-linux-gnu/perl5/5.26 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.26 /usr/share/perl/5.26 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up libheimbase1-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Setting up openssl (1.1.1-1ubuntu2.1~18.04.23) ...
Setting up libsqlite3-0:amd64 (3.22.0-1ubuntu0.7) ...
Setting up libmpc3:amd64 (1.1.0-1) ...
Setting up libc-dev-bin (2.27-3ubuntu1.6) ...
Setting up libkeyutils1:amd64 (1.5.9-9.2ubuntu2.1) ...
Setting up libsasl2-modules:amd64 (2.1.27~101-g0780600+dfsg-3ubuntu2.4) ...
Setting up ca-certificates (20230311ubuntu0.18.04.1) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.26.1 /usr/local/share/perl/5.26.1 /usr/lib/x86_64-linux-gnu/perl5/5.26 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.26 /usr/share/perl/5.26 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Updating certificates in /etc/ssl/certs...
137 added, 0 removed; done.
Setting up manpages-dev (4.15-1) ...
Setting up libc6-dev:amd64 (2.27-3ubuntu1.6) ...
Setting up libitm1:amd64 (8.4.0-1ubuntu1~18.04) ...
Setting up liblzo2-2:amd64 (2.08-1.2) ...
Setting up libisl19:amd64 (0.19-1) ...
Setting up libjsoncpp1:amd64 (1.7.4-3) ...
Setting up libk5crypto3:amd64 (1.16-2ubuntu0.4) ...
Setting up libwind0-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Setting up libasan4:amd64 (7.5.0-3ubuntu1~18.04) ...
Setting up libbinutils:amd64 (2.30-21ubuntu1~18.04.9) ...
Setting up libarchive13:amd64 (3.2.2-3.1ubuntu0.7) ...
Setting up libcilkrts5:amd64 (7.5.0-3ubuntu1~18.04) ...
Setting up libasn1-8-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Setting up libubsan0:amd64 (7.5.0-3ubuntu1~18.04) ...
Setting up libhcrypto4-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Setting up libhx509-5-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Setting up libgcc-7-dev:amd64 (7.5.0-3ubuntu1~18.04) ...
Setting up cpp-7 (7.5.0-3ubuntu1~18.04) ...
Setting up libstdc++-7-dev:amd64 (7.5.0-3ubuntu1~18.04) ...
Setting up libkrb5-3:amd64 (1.16-2ubuntu0.4) ...
Setting up libkrb5-26-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Setting up libheimntlm0-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Setting up binutils-x86-64-linux-gnu (2.30-21ubuntu1~18.04.9) ...
Setting up cpp (4:7.4.0-1ubuntu2.3) ...
Setting up libgssapi-krb5-2:amd64 (1.16-2ubuntu0.4) ...
Setting up binutils (2.30-21ubuntu1~18.04.9) ...
Setting up libgssapi3-heimdal:amd64 (7.5.0+dfsg-1ubuntu0.4) ...
Setting up gcc-7 (7.5.0-3ubuntu1~18.04) ...
Setting up g++-7 (7.5.0-3ubuntu1~18.04) ...
Setting up gcc (4:7.4.0-1ubuntu2.3) ...
Setting up libldap-2.4-2:amd64 (2.4.45+dfsg-1ubuntu1.11) ...
Setting up g++ (4:7.4.0-1ubuntu2.3) ...
update-alternatives: using /usr/bin/g++ to provide /usr/bin/c++ (c++) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/c++.1.gz because associated file /usr/share/man/man1/g++.1.gz (of link group c++) doesn't exist
Setting up libcurl4:amd64 (7.58.0-2ubuntu3.24) ...
Setting up cmake (3.10.2-1ubuntu2.18.04.2) ...
Processing triggers for libc-bin (2.27-3ubuntu1.6) ...
Processing triggers for ca-certificates (20230311ubuntu0.18.04.1) ...
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
Removing intermediate container 42561aa83cca
 ---> d5c9ca2e2e90
Step 4/12 : COPY . print/
 ---> 5b95ed895261
Step 5/12 : WORKDIR print
 ---> Running in 643e4af32777
Removing intermediate container 643e4af32777
 ---> fefee9ed908e
Step 6/12 : RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
 ---> Running in ddfa07cf58dd
-- [hunter] Initializing Hunter workspace (e6396699e414120e32557fe92db097b7655b760b)
-- [hunter]   https://github.com/cpp-pm/hunter/archive/v0.24.17.tar.gz
-- [hunter]   -> /root/.hunter/_Base/Download/Hunter/0.24.17/e639669
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /root/.hunter
-- [hunter] [ Hunter-ID: e639669 | Toolchain-ID: 9b2c9d4 | Config-ID: 338f12f ]
-- [hunter] GTEST_ROOT: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Install (ver.: 1.12.1)
-- [hunter] Building GTest
loading initial cache file /root/.hunter/_Base/e639669/9b2c9d4/338f12f/cache.cmake
loading initial cache file /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/args.cmake
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Build
Scanning dependencies of target GTest-Release
[  6%] Creating directories for 'GTest-Release'
[ 12%] Performing download step (download, verify and extract) for 'GTest-Release'
-- Downloading...
   dst='/root/.hunter/_Base/Download/GTest/1.12.1/cdddd44/release-1.12.1.tar.gz'
   timeout='none'
-- Using src='https://github.com/google/googletest/archive/release-1.12.1.tar.gz'
-- [download 0% complete]
-- [download 1% complete]
-- [download 2% complete]
-- [download 3% complete]
-- [download 4% complete]
-- [download 5% complete]
-- [download 6% complete]
-- [download 7% complete]
-- [download 8% complete]
-- [download 9% complete]
-- [download 10% complete]
-- [download 11% complete]
-- [download 12% complete]
-- [download 13% complete]
-- [download 14% complete]
-- [download 15% complete]
-- [download 16% complete]
-- [download 17% complete]
-- [download 18% complete]
-- [download 19% complete]
-- [download 20% complete]
-- [download 21% complete]
-- [download 22% complete]
-- [download 23% complete]
-- [download 24% complete]
-- [download 25% complete]
-- [download 26% complete]
-- [download 28% complete]
-- [download 30% complete]
-- [download 32% complete]
-- [download 34% complete]
-- [download 36% complete]
-- [download 38% complete]
-- [download 40% complete]
-- [download 42% complete]
-- [download 44% complete]
-- [download 46% complete]
-- [download 48% complete]
-- [download 49% complete]
-- [download 51% complete]
-- [download 53% complete]
-- [download 55% complete]
-- [download 57% complete]
-- [download 59% complete]
-- [download 61% complete]
-- [download 63% complete]
-- [download 65% complete]
-- [download 67% complete]
-- [download 69% complete]
-- [download 70% complete]
-- [download 72% complete]
-- [download 74% complete]
-- [download 76% complete]
-- [download 78% complete]
-- [download 80% complete]
-- [download 82% complete]
-- [download 84% complete]
-- [download 86% complete]
-- [download 88% complete]
-- [download 90% complete]
-- [download 92% complete]
-- [download 93% complete]
-- [download 95% complete]
-- [download 97% complete]
-- [download 99% complete]
-- [download 100% complete]
-- verifying file...
       file='/root/.hunter/_Base/Download/GTest/1.12.1/cdddd44/release-1.12.1.tar.gz'
-- Downloading... done
-- extracting...
     src='/root/.hunter/_Base/Download/GTest/1.12.1/cdddd44/release-1.12.1.tar.gz'
     dst='/root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 18%] No patch step for 'GTest-Release'
[ 25%] No update step for 'GTest-Release'
[ 31%] Performing configure step for 'GTest-Release'
loading initial cache file /root/.hunter/_Base/e639669/9b2c9d4/338f12f/cache.cmake
loading initial cache file /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/args.cmake
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Could NOT find PythonInterp (missing: PYTHON_EXECUTABLE) 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Build/GTest-Release-prefix/src/GTest-Release-build
[ 37%] Performing build step for 'GTest-Release'
Scanning dependencies of target gtest
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtest.a
[ 25%] Built target gtest
Scanning dependencies of target gtest_main
Scanning dependencies of target gmock
[ 37%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_main.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmock.a
[ 75%] Built target gmock
Scanning dependencies of target gmock_main
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_main.a
[100%] Built target gmock_main
[ 43%] Performing install step for 'GTest-Release'
[ 25%] Built target gtest
[ 50%] Built target gmock
[ 75%] Built target gmock_main
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Release"
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/libgmock.a
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/libgmock_main.a
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/cmake/GTest/GTestTargets-release.cmake
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-assertion-result.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/libgtest.a
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/libgtest_main.a
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/args.cmake
[ 50%] Completed 'GTest-Release'
[ 50%] Built target GTest-Release
Scanning dependencies of target GTest-Debug
[ 56%] Creating directories for 'GTest-Debug'
[ 62%] Performing download step (download, verify and extract) for 'GTest-Debug'
-- verifying file...
       file='/root/.hunter/_Base/Download/GTest/1.12.1/cdddd44/release-1.12.1.tar.gz'
-- File already exists and hash match (skip download):
  file='/root/.hunter/_Base/Download/GTest/1.12.1/cdddd44/release-1.12.1.tar.gz'
  SHA1='cdddd449d4e3aa7bd421d4519c17139ea1890fe7'
-- extracting...
     src='/root/.hunter/_Base/Download/GTest/1.12.1/cdddd44/release-1.12.1.tar.gz'
     dst='/root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 68%] No patch step for 'GTest-Debug'
[ 75%] No update step for 'GTest-Debug'
[ 81%] Performing configure step for 'GTest-Debug'
loading initial cache file /root/.hunter/_Base/e639669/9b2c9d4/338f12f/cache.cmake
loading initial cache file /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/args.cmake
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Could NOT find PythonInterp (missing: PYTHON_EXECUTABLE) 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Build/GTest-Debug-prefix/src/GTest-Debug-build
[ 87%] Performing build step for 'GTest-Debug'
Scanning dependencies of target gtest
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtestd.a
[ 25%] Built target gtest
Scanning dependencies of target gtest_main
Scanning dependencies of target gmock
[ 37%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_maind.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmockd.a
[ 75%] Built target gmock
Scanning dependencies of target gmock_main
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_maind.a
[100%] Built target gmock_main
[ 93%] Performing install step for 'GTest-Debug'
[ 25%] Built target gtest
[ 50%] Built target gmock
[ 75%] Built target gmock_main
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Debug"
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-actions.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/libgmockd.a
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/libgmock_maind.a
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/cmake/GTest/GTestTargets-debug.cmake
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-printers.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest_prod.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-assertion-result.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-message.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Up-to-date: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/libgtestd.a
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/libgtest_maind.a
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /root/.hunter/_Base/e639669/9b2c9d4/338f12f/Build/GTest)
-- [hunter] Cache saved: /root/.hunter/_Base/Cache/raw/ee0c74ec7e0aeb7acb10da7148a7c7f819e2b39d.tar.bz2
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /print/_build
Removing intermediate container ddfa07cf58dd
 ---> 62a677aaa832
Step 7/12 : RUN cmake --build _build
 ---> Running in bf75f6348ef9
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target demo
[ 75%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[100%] Linking CXX executable demo
[100%] Built target demo
Removing intermediate container bf75f6348ef9
 ---> 8114f996c7cf
Step 8/12 : RUN cmake --build _build --target install
 ---> Running in 77a3fa1bd45a
[ 50%] Built target print
[100%] Built target demo
Install the project...
-- Install configuration: "Release"
-- Installing: /print/_install/lib/libprint.a
-- Up-to-date: /print/_install/include
-- Up-to-date: /print/_install/include/print.hpp
-- Old export file "/print/_install/cmake/print-config.cmake" will be replaced.  Removing files [/print/_install/cmake/print-config-noconfig.cmake].
-- Installing: /print/_install/cmake/print-config.cmake
-- Installing: /print/_install/cmake/print-config-release.cmake
-- Installing: /print/_install/bin/demo
Removing intermediate container 77a3fa1bd45a
 ---> d1c59f28fd15
Step 9/12 : ENV LOG_PATH /home/logs/log.txt
 ---> Running in 8acf08409f99
Removing intermediate container 8acf08409f99
 ---> 4aa3858c0f71
Step 10/12 : VOLUME /home/logs
 ---> Running in d724dd291d47
Removing intermediate container d724dd291d47
 ---> cb6ef7f158cb
Step 11/12 : WORKDIR _install/bin
 ---> Running in 9a17f31f0d5a
Removing intermediate container 9a17f31f0d5a
 ---> 2f4dd4cbab4c
Step 12/12 : ENTRYPOINT ./demo
 ---> Running in 50238194c69f
Removing intermediate container 50238194c69f
 ---> e5a80b3c975a
Successfully built e5a80b3c975a
Successfully tagged logger:latest
```
</p>
</details>

$ docker images
```sh
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
logger       latest    e5a80b3c975a   3 minutes ago   359MB
ubuntu       18.04     97ba4bbc97fc   2 weeks ago     63.2MB
```


```sh
 mkdir logs
$ docker run -it -v "$(pwd)/logs/:/home/logs/" logger
text1
text2
text3
<C-D>
```
```sh
$ docker inspect logger
[
    {
        "Id": "sha256:e5a80b3c975aa1a30bf9f45b0e7dc75d7cca88df8d8744ce0978f1c61d914aa6",
        "RepoTags": [
            "logger:latest"
        ],
        "RepoDigests": [],
        "Parent": "sha256:2f4dd4cbab4c6268dcb9766a6518fb39d49041604acad937632cffb8af5e26fc",
        "Comment": "",
        "Created": "2023-05-31T18:19:36.859429601Z",
        "Container": "50238194c69f4cca809535c02b006adc88ce3a551c24d53d9d28eddc6bf27cde",
        "ContainerConfig": {
            "Hostname": "50238194c69f",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LOG_PATH=/home/logs/log.txt"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "ENTRYPOINT [\"/bin/sh\" \"-c\" \"./demo\"]"
            ],
            "Image": "sha256:2f4dd4cbab4c6268dcb9766a6518fb39d49041604acad937632cffb8af5e26fc",
            "Volumes": {
                "/home/logs": {}
            },
            "WorkingDir": "/print/_install/bin",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "./demo"
            ],
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "18.04"
            }
        },
        "DockerVersion": "20.10.21",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LOG_PATH=/home/logs/log.txt"
            ],
            "Cmd": null,
            "Image": "sha256:2f4dd4cbab4c6268dcb9766a6518fb39d49041604acad937632cffb8af5e26fc",
            "Volumes": {
                "/home/logs": {}
            },
            "WorkingDir": "/print/_install/bin",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "./demo"
            ],
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "18.04"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 359078529,
        "VirtualSize": 359078529,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/96794a818dc2ce5430a6456161e38878f1451bd26eae53f47285e11675d02b62/diff:/var/lib/docker/overlay2/e69bef1e1c8f19b78620d43a89824316588d9f30459d06021c43d64cf14a2ffb/diff:/var/lib/docker/overlay2/f63f9fce7419591f87219f74a6a2e2ef0cf29a9374327988b35d453bf3bf7ce9/diff:/var/lib/docker/overlay2/69e99f79fa879387538bdd9b97ae8d18987856910cfc43b4c19ba651226f6fb9/diff:/var/lib/docker/overlay2/88da07987ad02f060da1f46bfada340aa41eca63fb1370de71191d713cc89763/diff:/var/lib/docker/overlay2/72eb9719fb094f4abaf5c2226f0f7d1eeb17c61f82dfb4e86389e8e1d8ee8223/diff",
                "MergedDir": "/var/lib/docker/overlay2/01dff53fc17bbe51398e6f8dd99876836406b988c904070695a56219efc4aec5/merged",
                "UpperDir": "/var/lib/docker/overlay2/01dff53fc17bbe51398e6f8dd99876836406b988c904070695a56219efc4aec5/diff",
                "WorkDir": "/var/lib/docker/overlay2/01dff53fc17bbe51398e6f8dd99876836406b988c904070695a56219efc4aec5/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:09d5e92ce69e5b398823f217f748114698689cc296331870e9ddac8bdf5e9b65",
                "sha256:89597868ebdbdaef7d0b3b2f4fa32eca4cbd064ca98e08c74462d3240fc4bb0a",
                "sha256:3e5b87b9d3b126e51e963f04e9c2c6c858d2e455b7aa9286c33d77f57b3b110e",
                "sha256:1384fda2693d5ef924bc202416eb4c905cc092fcb67be07c783755c66fc83c18",
                "sha256:726d0abc54ca4ae64fe4c1d84eb3be9f614c0ea768fb16bc24479e5a3aeac986",
                "sha256:9dd07937b92f1d65c4722670be83094fb39693122046bdf75f3aa0a0939933ee",
                "sha256:cfa7098db8c06b23d68e3d87cc8a54098adbadc89f28c8f2c66c4a93e53e415f"
            ]
        },
        "Metadata": {
            "LastTagTime": "2023-05-31T21:19:36.880979537+03:00"
        }
    }
]
```

```sh
$ cat logs/log.txt
```

```sh
$ gsed -i 's/lab07/lab08/g' README.md
```

```sh
$ git add Dockerfile
$ git add .travis.yml
$ git commit -m"adding Dockerfile"
$ git push origin master
```



