# RHEL 9 podman 설치 가이드 (RHEL 9 podman Installation Guide)

RHEL 9(Red Hat Enterprise Linux 9)에서 Podman은 기본 컨테이너 엔진으로 포함되어 있어 설치 과정이 매우 간단합니다. 별도의 레포지토리를 추가하지 않아도 기본 AppStream 레포지토리를 통해 바로 설치할 수 있습니다.

## 1. 기본 설치 방법가장 표준적인 방법은 dnf 패키지 관리자를 사용하는 것입니다.

```bash
# 1. 시스템 업데이트 (권장)
sudo dnf update -y

# 2. Podman 설치
sudo dnf install -y podman

# 3. 설치 확인 및 버전 체크
podman --version
```

**컨테이너 도구 세트로 설치하기 (추천)** 단순히 podman만 설치하는 대신, 컨테이너 빌드(buildah), 이미지 검사(skopeo) 등이 포함된 도구 모음을 한 번에 설치하는 것이 실무에서 더 유용합니다.

```bash
sudo dnf install -y container-tools
```

## 2. Rootless 설정
Podman의 가장 큰 장점은 루트 권한 없이 컨테이너를 실행할 수 있다는 점입니다. RHEL 9에서는 기본적으로 활성화되어 있으나, 사용자 범위가 설정되어 있는지 확인이 필요합니다.

```bash
# 사용자 하위 UID/GID 범위 확인
cat /etc/subuid
cat /etc/subgid
```

만약 자신의 사용자 이름이 보이지 않는다면 아래 명령어로 추가합니다.

```bash
sudo usermod --add-subuids 100000-165535 --add-subgids 100000-165535 $USER
```

## 3. Docker 호환성 설정 (Docker 명령어 사용하기)
Docker 명령어에 익숙하다면 podman-docker 패키지를 설치하여 docker 명령어를 입력했을 때 내부적으로 podman이 동작하도록 설정할 수 있습니다.

```bash
# Docker 별칭 패키지 설치
sudo dnf install -y podman-docker

# 확인
docker ps
```

(패키지 설치 대신 단순히 alias docker=podman을 .bashrc에 추가해도 무방합니다.)

## 4. Podman Compose 설치
여러 컨테이너를 한꺼번에 관리해야 한다면 podman-compose가 필요합니다. RHEL 9 기본 레포지토리에 없는 경우 EPEL 레포지토리를 통해 설치하는 것이 가장 깔끔합니다.

```bash
# 1. EPEL 레포지토리 활성화
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm

# 2. podman-compose 설치
sudo dnf install -y podman-compose
```

## 5. 기본 명령어 퀵스타트
설치가 완료되었다면 다음 명령어로 테스트해 보세요.

| 작업 | 명령어 |
|----|----|
| 이미지 검색 | podman search nginx |
| 이미지 풀(Pull) | podman pull nginx |
| 컨테이너 실행 | podman run -d --name web -p 8080:80 nginx |
| 실행 중인 컨테이너 확인 | podman ps |
| 컨테이너 중지/삭제 | podman stop web / podman rm web |

## 💡 추가 팁
RHEL 9에서는 podman.socket을 활성화하면 Docker API와 호환되는 툴(예: 가상 도우미, 대시보드)을 그대로 사용할 수 있습니다. sudo systemctl enable --now podman.socket 명령어를 사용해 보세요.
