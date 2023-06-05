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
splashfan@splashfan:~/SplashFan/workspace/projects/lab08$ sudo docker build -t logger .
[sudo] password for splashfan: 
Sending build context to Docker daemon  3.674MB
Step 1/12 : FROM ubuntu:18.04
 ---> 97ba4bbc97fc
Step 2/12 : RUN apt update
 ---> Using cache
 ---> c636aa0a1acd
Step 3/12 : RUN apt install -yy gcc g++ cmake
 ---> Using cache
 ---> d5c9ca2e2e90
Step 4/12 : COPY . print/
 ---> f6455a75fb44
Step 5/12 : WORKDIR print
 ---> Running in 3a677c7bc321
Removing intermediate container 3a677c7bc321
 ---> 322a0bb13a15
Step 6/12 : RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
 ---> Running in b595bd8ae7d4
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
-- Configuring done
-- Generating done
-- Build files have been written to: /print/_build
Removing intermediate container b595bd8ae7d4
 ---> 0c30040dc900
Step 7/12 : RUN cmake --build _build
 ---> Running in 6e1f7ddf22d6
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target demo
[ 75%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[100%] Linking CXX executable demo
[100%] Built target demo
Removing intermediate container 6e1f7ddf22d6
 ---> d77a573c4085
Step 8/12 : RUN cmake --build _build --target install
 ---> Running in 8e064d7003f2
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
Removing intermediate container 8e064d7003f2
 ---> 7cbfb4a06b00
Step 9/12 : ENV LOG_PATH /home/logs/log.txt
 ---> Running in 9f156daeca3b
Removing intermediate container 9f156daeca3b
 ---> b82e0a88397a
Step 10/12 : VOLUME /home/logs
 ---> Running in e3e4516f7ac3
Removing intermediate container e3e4516f7ac3
 ---> 7d2d39dff438
Step 11/12 : WORKDIR _install/bin
 ---> Running in 4571d90f2b1e
Removing intermediate container 4571d90f2b1e
 ---> e38c82681fec
Step 12/12 : ENTRYPOINT ./demo
 ---> Running in 2108704dcf6b
Removing intermediate container 2108704dcf6b
 ---> fb241d65dd91
Successfully built fb241d65dd91
Successfully tagged logger:latest
```
 
 splashfan@splashfan:~/SplashFan/workspace/projects/lab08$ sudo docker inspect logger
 ```sh
[
    {
        "Id": "sha256:fb241d65dd9150dd1cf287a5fc91330671ac5a270c94fbb21b906a2b13e75f5f",
        "RepoTags": [
            "logger:latest"
        ],
        "RepoDigests": [],
        "Parent": "sha256:e38c82681fec69cd0c059744f1ac601503b80d97b01d211ff25fb105a9f1b9a7",
        "Comment": "",
        "Created": "2023-06-05T07:24:29.006721352Z",
        "Container": "2108704dcf6b31ee81156d4a9661e445d7d9fd60a149e1e40173d062956d3c93",
        "ContainerConfig": {
            "Hostname": "2108704dcf6b",
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
            "Image": "sha256:e38c82681fec69cd0c059744f1ac601503b80d97b01d211ff25fb105a9f1b9a7",
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
            "Image": "sha256:e38c82681fec69cd0c059744f1ac601503b80d97b01d211ff25fb105a9f1b9a7",
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
        "Size": 337014537,
        "VirtualSize": 337014537,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/7e032272dea537b1573cfa2938defd5135e6b9f8ccaacd9b8777508405552e99/diff:/var/lib/docker/overlay2/dde1ab00dfe8e101b69b4f58d9c4b1018e4915b6e0fabc3d44c0ca692026544b/diff:/var/lib/docker/overlay2/6bdd02326cead7727531f416c2190ce992b9d64d92289bf55870b6356eef9176/diff:/var/lib/docker/overlay2/69e99f79fa879387538bdd9b97ae8d18987856910cfc43b4c19ba651226f6fb9/diff:/var/lib/docker/overlay2/88da07987ad02f060da1f46bfada340aa41eca63fb1370de71191d713cc89763/diff:/var/lib/docker/overlay2/72eb9719fb094f4abaf5c2226f0f7d1eeb17c61f82dfb4e86389e8e1d8ee8223/diff",
                "MergedDir": "/var/lib/docker/overlay2/f8a91094c330aa7cc25ce7fe82d8da05648074f5dc0a7ce891648da704031d45/merged",
                "UpperDir": "/var/lib/docker/overlay2/f8a91094c330aa7cc25ce7fe82d8da05648074f5dc0a7ce891648da704031d45/diff",
                "WorkDir": "/var/lib/docker/overlay2/f8a91094c330aa7cc25ce7fe82d8da05648074f5dc0a7ce891648da704031d45/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:09d5e92ce69e5b398823f217f748114698689cc296331870e9ddac8bdf5e9b65",
                "sha256:89597868ebdbdaef7d0b3b2f4fa32eca4cbd064ca98e08c74462d3240fc4bb0a",
                "sha256:3e5b87b9d3b126e51e963f04e9c2c6c858d2e455b7aa9286c33d77f57b3b110e",
                "sha256:28973e254c6923ecadcbe23330b3032ff50cdd1f3a24a162d0cb2e4e2cb12323",
                "sha256:54b55df24ae79008b73ed347898a6a7b7fc7ae7d992424f21a85636b664d723b",
                "sha256:978de3d115333092aeb99156210d4adf764c19da60bcab0fb2473d2046a5d0b6",
                "sha256:216300e12043cf35bfb16a4d8b766e2056861ce87d3a722ba19335195e408884"
            ]
        },
        "Metadata": {
            "LastTagTime": "2023-06-05T10:24:29.062342327+03:00"
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



