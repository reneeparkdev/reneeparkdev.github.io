---
title: "[Docker] 도커(Docker)로 MySQL 5.7 설치하기 <도커 체험기 2>"
author: renee
date: 2024-06-19 15:00:00 +0800
categories: [Skill, Docker]
tags: [Docker, MySQL, Mac]
published: true
---

안녕하세요, 여러분!

오늘은 도커(Docker)를 이용하여 맥(Mac) 로컬(local)에 <kbd>MySQL 5.7</kbd>을 설치하는 과정을 포스팅해보려고 합니다. 아직 맥에 도커가 설치되어 있지 않으신 분들은 아래 [**관련 포스팅**](#관련-포스팅)을 참고해주세요!

## **관련 포스팅**
[[Docker] 맥(Mac)에 도커(Docker) 설치하기 <도커 체험기 1>](https://reneeparkdev.github.io/posts/experience-docker-1/)

## **도커로 MySQL 5.7 설치하는 법**

맥에 도커로 MySQL 5.7을 설치하는 과정을 단계별로 살펴보겠습니다.

### **1. docker, docker-compose 버전 확인**

우선 도커와 도커 컴포즈 버전을 확인해줍니다. 저는 맥이든 리눅스든 터미널에서 명령어를 실행하기 전에 하는 습관같은게 있어요. 그것은 바로 현재 디렉토리 확인(`pwd`), 작업 디렉토리로 이동(`cd`), 작업 디렉토리 목록 확인(`ls`), 사용할 소프트웨어의 버전 확인(`--version`, `-V`, `-v`, `-version` 등)을 하는 것입니다. 이걸 해주면 이상하게 마음이 편안하더라구요. 뭐, 빈번하거나 중요도가 떨어지는 작업이라면 생략하는 경우도 있지만요! 그래도 대부분의 작업을 진행할 때는 꼭 하는 편입니다. 위 절차를 해주면, 하지 않는 것보다 실수할 확률이 줄어드는 것만 같은 느낌이랄까요? 🤔 아무래도 스스로가 더 신경쓰다보니 실수가 줄어드는 게 아닐까 싶습니다.

#### **도커 버전 확인**

```shell
docker --version
```

<details>
<summary>출력물</summary>
<div markdown="1">

```console
Docker version 26.1.1, build 4cf5afa
```

</div>
</details>

#### **도커 컴포즈 버전 확인**

```shell
docker-compose --version
```

<details>
<summary>출력물</summary>
<div markdown="1">

```console
Docker Compose version v2.27.0-desktop.2
```

</div>
</details>

<br>

<span style="color: var(--text-muted-color);">*최근 도커가 먹통이 되는 바람에 **완전히** 삭제하고 다시 설치하였습니다. 그래서 이전 포스팅의 도커, 도커 컴포즈와 버전이 다릅니다!*</span>

### **2. MySQL 5.7 도커 이미지 다운로드**

어려울 것 없어요. 초보자 분들은 아래 나와있는 대로 하면 수월하게 진행할 수 있을 겁니다. 비 전공자도 따라하기 쉽게 글을 작성하려고 노력하는 중입니다!

#### **이미지 검색**

```shell
docker search mysql
# or
docker search mysql5.7
```

도커 허브(Docker Hub)에서 MySQL 5.7 이미지를 검색하는 명령어입니다. 명령어 실행 결과 여러 이미지들이 나열되며, 각 이미지의 이름, 설명, 별점 등이 표시됩니다. `OFFICIAL` 열은 공식 이미지 여부를 나타냅니다.

<details>
<summary>출력물</summary>
<div markdown="1">

```console
NAME                            DESCRIPTION                                     STARS     OFFICIAL
mysql                           MySQL is a widely used, open-source relation…   15165     [OK]
mariadb                         MariaDB Server is a high performing open sou…   5774      [OK]
percona                         Percona Server is a fork of the MySQL relati…   629       [OK]
phpmyadmin                      phpMyAdmin - A web interface for MySQL and M…   993       [OK]
circleci/mysql                  MySQL is a widely used, open-source relation…   30
bitnami/mysql                   Bitnami container image for MySQL               111
bitnami/mysqld-exporter         Bitnami container image for MySQL Server Exp…   7
cimg/mysql                                                                      3
ubuntu/mysql                    MySQL open source fast, stable, multi-thread…   63
rapidfort/mysql                 RapidFort optimized, hardened image for MySQL   25
bitnamicharts/mysql                                                             0
rapidfort/mysql8-ib             RapidFort optimized, hardened image for MySQ…   9
google/mysql                    MySQL server for Google Compute Engine          25
elestio/mysql                   Mysql, verified and packaged by Elestio         0
rapidfort/mysql-official        RapidFort optimized, hardened image for MySQ…   9
hashicorp/mysql-portworx-demo                                                   0
newrelic/mysql-plugin           New Relic Plugin for monitoring MySQL databa…   1
databack/mysql-backup           Back up mysql databases to... anywhere!         116
linuxserver/mysql               A Mysql container, brought to you by LinuxSe…   41
mirantis/mysql                                                                  0
linuxserver/mysql-workbench                                                     54
docksal/mysql                   MySQL service images for Docksal - https://d…   0
vitess/mysqlctld                vitess/mysqlctld                                1
drupalci/mysql-5.5              https://www.drupal.org/project/drupalci         3
eclipse/mysql                   Mysql 5.7, curl, rsync                          1
```

```console
NAME                                   DESCRIPTION                                     STARS     OFFICIAL
oilrmutp57/mysql5.7                                                                    6
bingozhou/mysql5.7                     mysql5.7                                        8
alanpeng/mysql5.7-replication-docker   https://github.com/alanpeng/mysql5.7-replica…   1
jayudev/mysql5.7                       mysql5.7                                        2
acdaic4v/mysql5.7-k2                   Mysql 5.7 for use with joomla extension k2 ,…   1
player001/mysql5.7-php5                                                                0
yskkuwahara/mysql5.7                   MySQL5.7                                        0
kurashitech/mysql5.7.22                mysql5.7.22                                     0
ymnoor21/mysql5.7                      Dockerize MySQL 5.7 on a Ubuntu 14.04 setup.    1
alexmbarbosa/mysql5.7                  Based on official mysql:5.7 docker image        1
bunchjesse/mysql5.7                    MySQL 5.7                                       0
transposit/mysql5.7                                                                    0
fukuyama012/mysql5.7                                                                   0
...
```

</div>
</details>

#### **이미지 pull(다운로드)**

```shell
docker pull mysql:5.7
```

도커 허브에서 MySQL 5.7 이미지를 다운로드하는 명령어입니다.

<details>
<summary>출력물</summary>
<div markdown="1">

```console
5.7: Pulling from library/mysql
20e4dcae4c69: Pull complete
1c56c3d4ce74: Pull complete
e9f03a1c24ce: Pull complete
68c3898c2015: Pull complete
6b95a940e7b6: Pull complete
90986bb8de6e: Pull complete
ae71319cb779: Pull complete
ffc89e9dfd88: Pull complete
43d05e938198: Pull complete
064b2d298fba: Pull complete
df9a4d85569b: Pull complete
Digest: sha256:4bc6bc963e6d8443453676cae56536f4b8156d78bae03c0145cbe47c2aad73bb
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview mysql:5.7
```

</div>
</details>

<br>

>
만약 MySQL 8.0을 설치하고 싶다면 5.7 태그 대신에 8.0 태그를 넣어 위 과정을 수행하면 됩니다.
{: .prompt-tip }

- `docker search mysql8.0`
- `docker pull mysql:8.0`

#### **설치된 이미지 확인**

```shell
docker images
```

현재 시스템(로컬)에 다운로드 된 도커 이미지 목록을 표시하는 명령어입니다. 각 이미지의 저장소 이름, 태그, 이미지 ID, 생성된 시간, 크기가 표시됩니다.

<details>
<summary>출력물</summary>
<div markdown="1">

```console
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
mysql        5.7       5107333e08a8   6 months ago   501MB
```

</div>
</details>

#### **이미지 삭제**

```shell
docker rmi mysql:5.7  # mysql:{tag}
# or
docker rmi 5107333e08a8  # {image-id}
# or
docker rmi -f 5107333e08a8  # --force
```

도커 이미지를 삭제하고 싶다면 위 명령어를 수행하면 됩니다. `저장소 이름`과 `태그`를 이용하여 삭제하는 방법, `이미지 ID`를 이용하여 삭제하는 방법이 있습니다. 이미지를 삭제하려면 해당 이미지로부터 생성된 모든 컨테이너를 중지하고 삭제해야합니다. 그렇지 않으면 이미지를 삭제할 수 없다는 오류가 발생합니다. `-f`, `--force` 옵션을 사용하면 해당 이미지에 의존하는 컨테이너가 있다 하더라도 강제로 삭제 작업을 수행합니다.

MySQL 도커 이미지를 다운로드 받는 것은 그리 오래 걸리지 않으니 위 과정을 몇번 반복해 보는 것도 도커에 익숙해지는데 도움이 될 것 같네요! 😉

<details>
<summary>출력물</summary>
<div markdown="1">

```console
Untagged: mysql:5.7
Untagged: mysql@sha256:4bc6bc963e6d8443453676cae56536f4b8156d78bae03c0145cbe47c2aad73bb
Deleted: sha256:5107333e08a87b836d48ff7528b1e84b9c86781cc9f1748bbc1b8c42a870d933
Deleted: sha256:37fd5f1492d4e9cb540c52c26655f220568050438f804275e886200c8807ffb4
Deleted: sha256:1105a50d3483cb9f970e70cf5163e3352f0b2fe2ff07c6abcca6f34228e76dc5
Deleted: sha256:94187496c18bb11b78e71017f2774ad3c0a734da9749a46e917c4239504e9322
Deleted: sha256:ae59716eae3be604a4fd43e86fd2ad504cb06c89cc064c73c78eee651e675805
Deleted: sha256:97d26ca29ec287ff4bd09a49602c44cbcabcf3303ddc726b3b94cbe26dfe1c94
Deleted: sha256:27303974d12144264b32b8936ca7c90d72bdba939a9e791010201b3b1717c4c4
Deleted: sha256:4d4483f06dbe01282c10cb9e429a0be826c18c61048f7860dad49ae7f6bac927
Deleted: sha256:3b73a6f6b3298c568dcfb8fa5e96c581a1b5c0ad395b0c38f9addd0c79703124
Deleted: sha256:46446bf265a411a4a13a4adc86f60c9e0479a2e03273c98cafab7bc4151dd2bc
Deleted: sha256:1d5264146b09a27a8fc6801dc239a4962582ed27dd2fbd8ee708463a1857b06b
Deleted: sha256:cff044e186247f93aa52554c96d77143cc92f99b2b55914038d0941fddeb6623
```

</div>
</details>

### **3. 이미지 정보 확인**

#### **간단 정보 확인**

```shell
docker scout quickview mysql:5.7
```

<details>
<summary>출력물</summary>
<div markdown="1">

```console
    i New version 1.9.3 available (installed version is 1.8.0) at https://github.com/docker/scout-cli
    ✓ SBOM of image already cached, 170 packages indexed

  Target               │  mysql:5.7           │    3C    37H    18M     5L    11?
    digest             │  5107333e08a8        │
  Base image           │  oraclelinux:7-slim  │    0C     0H     0M     0L
  Refreshed base image │  oraclelinux:7-slim  │    0C     0H     0M     0L
                       │                      │
  Updated base image   │  oraclelinux:9-slim  │    0C     0H     0M     0L
                       │                      │

What's next:
    View vulnerabilities → docker scout cves mysql:5.7
    View base image update recommendations → docker scout recommendations mysql:5.7
    Include policy results in your quickview by supplying an organization → docker scout quickview mysql:5.7 --org <organization>
```

![alt text](assets/img/posts/2024-06-18-01-docker-scout-quickview.png)

</div>
</details>

#### **상세 정보 확인**

```shell
docker inspect mysql:5.7
```

`docker scout quickview` 명령어보다 상세한 메타데이터를 제공하는 기능입니다. `docker inspect` 명령어는 도커 객체(이미지, 컨테이너 등)의 자세한 정보를 JSON 형식으로 반환합니다. 아래 출력물을 살펴보면 MySQL 5.7 의 세부 버전도 확인할 수 있습니다.

<details>
<summary>출력물</summary>
<div markdown="1">

```console
[
    {
        "Id": "sha256:5107333e08a87b836d48ff7528b1e84b9c86781cc9f1748bbc1b8c42a870d933",
        "RepoTags": [
            "mysql:5.7"
        ],
        "RepoDigests": [
            "mysql@sha256:4bc6bc963e6d8443453676cae56536f4b8156d78bae03c0145cbe47c2aad73bb"
        ],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2023-12-12T19:11:08Z",
        "ContainerConfig": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": null,
            "Cmd": null,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "DockerVersion": "",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.16",
                "MYSQL_MAJOR=5.7",
                "MYSQL_VERSION=5.7.44-1.el7",
                "MYSQL_SHELL_VERSION=8.0.35-1.el7"
            ],
            "Cmd": [
                "mysqld"
            ],
            "ArgsEscaped": true,
            "Image": "",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 501392011,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/85576ed11c0278afd4202f175ebc5438a0f75af8435d25b6ba6fe4afdd98ab63/diff:/var/lib/docker/overlay2/7cb2d56c51d29aae4a35e3e0ef0558b48d8f4d29f96719343c4d851315324638/diff:/var/lib/docker/overlay2/63a1828285bc41495e1019f68bed20b15b8c6162a40e8f78fe864c71fac0ea57/diff:/var/lib/docker/overlay2/614afcc4e55fe4f969c9d11bd3453d2aa36d6ea14883f2e48b0e3b29eaf833fd/diff:/var/lib/docker/overlay2/dfe4aeaa01fb9521e3e7e0903a48890a4113f4178ab9546231a0f3ea061879b7/diff:/var/lib/docker/overlay2/329dd95e18664e2814630f882e9d6c8576ff382f4dc437fa1477bbfa4eb48a09/diff:/var/lib/docker/overlay2/ca209f336352a506d2cab5743cd463ca38cacc48bf6781c8a5e4f13b673e130a/diff:/var/lib/docker/overlay2/9753c908c07d2536f3690328f062365705d4ded10aa6b422339dfbb38bc8373f/diff:/var/lib/docker/overlay2/f715b8061ad84f9a618ba3fbd41063c5007929851d111e556df860d3cc57c590/diff:/var/lib/docker/overlay2/2a1c9da3fce11e3f370a912fb71f7cf5133aa9fec63aac88f6c42763b059cbd8/diff",
                "MergedDir": "/var/lib/docker/overlay2/f350d521390bf4807e87a3c607ef579e3c1f533cd22be971d00e1a4828ca8817/merged",
                "UpperDir": "/var/lib/docker/overlay2/f350d521390bf4807e87a3c607ef579e3c1f533cd22be971d00e1a4828ca8817/diff",
                "WorkDir": "/var/lib/docker/overlay2/f350d521390bf4807e87a3c607ef579e3c1f533cd22be971d00e1a4828ca8817/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:cff044e186247f93aa52554c96d77143cc92f99b2b55914038d0941fddeb6623",
                "sha256:7ff7abf4911b44c1b705de478892bac6d01821c65ebc2993edb87136d51eb670",
                "sha256:8b2952eb02aac23a82803bf3e25d94ea78f3d4674d972cc7324a712ad9d54b6f",
                "sha256:d76a5f910f6ba5bce12b14e396f8386d385d62bbc4c9d82af25ae956c11bb3aa",
                "sha256:8527ccd6bd857b844293f9efe34222229fa76e040d55dd03e019f305f7bd2a74",
                "sha256:4555572a6bb29d49eb9dbd1fb0938788ca7d772f441f8273626f1a12933fcee3",
                "sha256:0d9e9a9ce9e415229fa3c1953ec32c236bfde6a825f4a74a78013586071c02e8",
                "sha256:532b66f4569dfab5f87219c302ea23478e6ad9504863f2a7410c935593e6b526",
                "sha256:337ec6bae2225e56895f25ef88a874b2796e020332d63a35929f40e9e7fa158e",
                "sha256:73cb62467b8f9e06265bc00441cc3d8026d24ca3708d517a3df93ff5a787af77",
                "sha256:441e16cac4fe6b7abab2653886fbab030752e42c42bd508f1fa2f7f8c5df0fcf"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        },
        "Container": ""
    }
]
```

</div>
</details>

### **4. MySQL 5.7 도커 컨테이너 구축**

#### **컨테이너 생성 및 실행**

```shell
docker run --name mysql-5744-con -e MYSQL_ROOT_PASSWORD=mysql -d -p 3307:3306 mysql:5.7
```

도커 컨테이너를 생성하고 실행하는 명령어입니다.

<details>
<summary>출력물</summary>
<div markdown="1">

```console
34d46afaf0fb2d2414862c5e920d51689b3f1cb0cad8d056b1541c3c6ea4637a
```

명령어 수행 결과 생성한 컨테이너의 ID를 출력해줍니다.

</div>
</details>

<br>

각 옵션에 대해 설명하도록 하겠습니다.
- `--name {container-name}`: 생성할 컨테이너의 이름을 지정
- `-e MYSQL_ROOT_PASSWORD={password}`: 컨테이너 내에서 사용할 root 사용자 비밀번호를 환경 변수로 설정
- `-d`: 컨테이너를 백그라운드에서 실행 (detached mode)
- `-p 3307:3306`: 호스트와 컨테이너 간의 포트 포워딩 설정 (호스트의 3307 포트와 컨테이너의 3306 포트를 연결)
- `mysql:{tag}`: 실행할 도커 이미지 지정

>
만약 로컬에 이미 설치된 MySQL이 있다면 포트 설정에 주의해야합니다! 제 로컬에는 도커로 설치한 MySQL 5.7 외에도 brew로 설치한 MySQL 5.7이 존재합니다. MySQL은 기본적으로 3306 포트를 사용하도록 설정되어 있습니다. 이런 경우에 둘 다 3306 포트를 쓰면 충돌이 나겠죠? 따라서 도커로 설치한 MySQL은 3307 포트를 사용하도록 설정 해주면 충돌을 피할 수 있습니다. 로컬에서 도커 컨테이너로 실행한 MySQL에 접속하려면 3307 포트로 접근하면 됩니다.
{: .prompt-warning }

#### **생성된 컨테이너 확인**

```shell
docker ps -a
```

도커에서 현재 실행 중인 모든 컨테이너들을 조회하는 명령어입니다. `-a` 옵션을 사용하면 중지된(실행 중이지 않은) 컨테이너를 포함한 모든 컨테이너의 목록을 표시할 수 있습니다.

<details>
<summary>출력물</summary>
<div markdown="1">

```console
CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS                               NAMES
34d46afaf0fb   mysql:5.7   "docker-entrypoint.s…"   5 seconds ago   Up 4 seconds   33060/tcp, 0.0.0.0:3307->3306/tcp   mysql-5744-con
```

</div>
</details>

#### **컨테이너 접속**

```shell
docker exec -it mysql-5744-con bash
```

위 명령어를 사용하면 도커 컨테이너의 bash 셸(쉘)에 접속하게 됩니다. 이를 통해 셸을 사용하는 것처럼 명령어를 입력하고 출력 결과를 볼 수 있습니다.

<details>
<summary>출력물</summary>
<div markdown="1">

```console
bash-4.2# mysql --version
mysql  Ver 14.14 Distrib 5.7.44, for Linux (x86_64) using  EditLine wrapper
```

</div>
</details>

#### **컨테이너 삭제**

```shell
docker rm mysql-5744-con  # {container-name}
# or
docker rm 34d46afaf0fb  # {container-id}
# or
docker container prune  # 모든 중지된 컨테이너 삭제
```

도커 컨테이너를 삭제하는 명령어입니다. `컨테이너 이름`과 `컨테이너 ID`를 지정하여 삭제할 수 있습니다. 실행 중인 컨테이너는 기본적으로 `docker rm` 명령어로 삭제할 수 없습니다. 먼저 컨테이너를 중지한 후 삭제해야 합니다. `-f`, `--force` 옵션을 사용하면 강제로 삭제할 수 있습니다.

>
도커 컨테이너를 실행하고 중지하려면 아래 명령어를 사용하면 됩니다.
{: .prompt-info}

- 실행: `docker start {container-name or container-id}`
- 중지: `docker stop {container-name or container-id}`

## **맺음말**

오늘의 포스팅은 여기까지 입니다! 아직 블로그 글 쓰는게 익숙하지 않아서 속도가 더딘 것 같습니다. 🫠 다음 글은 좀 더 빠르게 마무리 짓길 바라며, 이만 마치도록 하겠습니다. 다음 포스팅에서 만나요! 꼭~!

<br>

<span style="color: var(--text-muted-color);">*코멘트는 언제나 환영입니다!*</span>
