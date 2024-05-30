---
title: "[Docker] 맥(Mac)에 도커(Docker) 설치하기 <도커 체험기 1>"
author: renee
date: 2024-05-30 11:00:00 +0800
categories: [Skill, Docker]
tags: [Docker, Mac]
published: true
pin: true
---

안녕하세요, 여러분!

블로그를 개설해 놓고 오랫동안 방치했었네요. 깃허브(Github) 블로그 초기 세팅을 완료한 지 벌써 두 달이 지났습니다. 😅 시간 참 빠르죠? 더 이상 미루다가는 끝이 없을 것 같아서 **'오늘은 꼭 해내고야 만다!'** 하는 마음으로 다시 찾아왔습니다. 첫 포스트 주제는 바로 <kbd>도커(Docker)</kbd>입니다!

제가 도커를 사용하게 된 이유는 MySQL 개발 환경 때문이었습니다. 회사에서 개발할 때 `MySQL 5.7`과 `MySQL 8.0`을 모두 사용했거든요. <span style="background-color: #fff5b1">처음에는 두 버전 모두 로컬에 설치해서 사용했는데, 어느 순간부터 두 버전 사이에 충돌이 생겨 먼저 설치한 5.7 버전에 접근할 수가 없더라고요. 그래서 어떻게 할까 고민하던 중에 Docker를 이용하면 이 문제를 해결할 수 있겠다는 생각이 들었습니다!</span> 그런 이유로 도커를 체험해보게 됐어요. 👍 그리고 이 경험을 도커를 처음 접하는 분들과 공유하면 좋겠다는 생각이 들어 이 포스트를 작성하게 되었습니다.

서두가 좀 길었네요. 이번 포스트에서는 도커가 무엇인지 간략히 설명하고, 맥(Mac)에 도커(Docker)를 설치하는 방법에 대해 이야기해보려고 합니다. 그럼 시작해 볼까요?

## **도커 간단 설명**

>
도커(Docker)는 소프트웨어를 컨테이너라는 가상화된 환경에서 실행할 수 있게 해주는 플랫폼입니다. 컨테이너는 애플리케이션과 그 실행에 필요한 모든 것을 포함하는 독립된 패키지입니다. 이를 통해 개발자는 어디서나 동일한 환경에서 애플리케이션을 실행할 수 있어, 개발과 배포가 일관되고 효율적으로 이루어집니다. 도커를 사용하면 여러 버전의 소프트웨어를 충돌 없이 손쉽게 관리할 수 있습니다.

도커 컨테이너와 함께 따라오는 개념 중에 하나가 바로 가상 머신입니다. 도커 컨테이너와 가상 머신에 대해 더 깊게 설명하면 좋겠지만, 그 내용은 매우 길어질 것이 분명하므로 다음을 기약하겠습니다.

## **맥에 도커 설치하는 법**

이제 맥에 도커를 설치하는 과정을 단계별로 살펴보겠습니다.

### **1. brew를 이용한 docker 설치**

```shell
brew install --cask docker
# or
brew install docker 
```

<details>
<summary>출력물</summary>
<div markdown="1">

```console
Running `brew update --auto-update`...
==> Auto-updated Homebrew!
Updated 2 taps (homebrew/core and homebrew/cask).
==> New Formulae
dcp                                      nvimpager
==> New Casks
free-podcast-transcription

You have 13 outdated formulae and 2 outdated casks installed.

==> Downloading https://raw.githubusercontent.com/Homebrew/homebrew-cask/005958c
######################################################################### 100.0%
==> Downloading https://desktop.docker.com/mac/main/amd64/124339/Docker.dmg
######################################################################### 100.0%
==> Installing Cask docker
==> Moving App 'Docker.app' to '/Applications/Docker.app'
==> Linking Binary 'docker' to '/usr/local/bin/docker'
==> Linking Binary 'docker-compose' to '/usr/local/bin/docker-compose'
==> Linking Binary 'docker-credential-desktop' to '/usr/local/bin/docker-credent
==> Linking Binary 'docker-credential-ecr-login' to '/usr/local/bin/docker-crede
==> Linking Binary 'docker-credential-osxkeychain' to '/usr/local/bin/docker-cre
==> Linking Binary 'docker-index' to '/usr/local/bin/docker-index'
==> Linking Binary 'hub-tool' to '/usr/local/bin/hub-tool'
==> Linking Binary 'kubectl' to '/usr/local/bin/kubectl.docker'
==> Linking Binary 'docker.bash-completion' to '/usr/local/etc/bash_completion.d
==> Linking Binary 'docker.zsh-completion' to '/usr/local/share/zsh/site-functio
==> Linking Binary 'docker.fish-completion' to '/usr/local/share/fish/vendor_com
==> Linking Binary 'com.docker.hyperkit' to '/usr/local/bin/hyperkit'
==> Linking Binary 'com.docker.cli' to '/usr/local/bin/com.docker.cli'
🍺  docker was successfully installed!
```

</div>
</details>

### **2. 설치된 도커 버전 확인**

```shell
docker --version
```

<details>
<summary>출력물</summary>
<div markdown="1">

```console
Docker version 24.0.6, build ed223bc
```

</div>
</details>

### **3. 함께 설치된 도커 컴포즈(Docker Compose) 버전 확인**

```shell
docker --version
```

<details>
<summary>출력물</summary>
<div markdown="1">

```console
Docker Compose version v2.22.0-desktop.2
```

</div>
</details>

## **맺음말**

오늘의 포스팅은 여기까지 입니다. 다음엔 도커를 사용하여 MySQL 5.7을 설치하고 실행해보는 과정을 포스팅 하려고 합니다. 다음 포스팅에서 만나요! 꼭~! 

<span style="color: #aaaaaa">*이 체험기는 2023년 10월 18일의 기록을 2024년 5월 30일에 포스팅 한 내용입니다. 최신 정보와는 다를 수 있으니 참고용으로 사용하세요!*</span>
