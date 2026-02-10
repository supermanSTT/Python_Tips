# RHEL 9 pyenv 설치 가이드 (RHEL 9 pyenv Installation Guide)

이 문서는 Red Hat Enterprise Linux 9 환경에서 파이썬 버전 관리를 위한 `pyenv` 설치 과정을 단계별로 설명합니다.

---

## 1. 시스템 업데이트 및 필수 의존성 설치
RHEL 9에서 파이썬을 소스 빌드하기 위해 필요한 개발 도구와 라이브러리를 설치합니다.

```bash
# 시스템 업데이트
sudo dnf update -y

# 빌드 도구 및 라이브러리 설치
sudo dnf install -y zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel openssl-devel xz xz-devel libffi-devel findutils
```

## 2. pyenv 설치 스크립트 실행
공식 설치 스크립트를 사용하여 `pyenv` 를 다운로드합니다.

```bash
curl https://pyenv.run | bash
```

## 3. 환경 변수 설정
사용 중인 쉘 리소스 파일(보통 `~/.bashrc` ) 하단에 아래 내용을 추가하여 `pyenv` 가 자동으로 로드되도록 설정합니다.

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```

```bash
source ~/.bashrc
```

## 4. 설치 확인 및 Python 설치

`pyenv`가 정상적으로 로드되었다면, 아래 명령어들을 통해 버전을 확인하고 원하는 파이썬 환경을 구축할 수 있습니다.

```bash
# 1) pyenv 정상 설치 여부 확인
pyenv --version

# 2) 설치 가능한 파이썬 버전 전체 목록 조회
# (목록이 매우 길 수 있으므로 grep 등으로 필터링 권장)
pyenv install -l

# 3) 특정 파이썬 버전 설치 (RHEL 9 환경에서 안정적인 3.11.x 권장)
pyenv install 3.11.5

# 4) 설치된 버전 목록 확인 (현재 사용 중인 버전은 * 표시)
pyenv versions

# 5) 시스템 전역(Global) 파이썬 버전 변경
pyenv global 3.11.5

# 6) 현재 활성화된 파이썬 버전 및 경로 확인
python -V
which python
```

## 💡 참고 사항

`pyenv`를 더 효율적으로 관리하고 발생할 수 있는 문제를 방지하기 위한 팁입니다.

* **최신 버전 목록 갱신**: 새로운 파이썬 버전이 릴리스되었을 때 `pyenv install -l` 목록에 보이지 않는다면 아래 명령어로 업데이트하세요.
    ```bash
    pyenv update
    ```

* **특정 프로젝트 전용 버전 설정**: 특정 디렉토리(프로젝트 폴더) 내에서만 특정 파이썬 버전을 사용하고 싶을 때는 `local` 명령어를 사용합니다.
    ```bash
    # 해당 디렉토리로 이동 후
    pyenv local 3.10.12
    ```

* **컴파일 오류 대응**: RHEL 9에서 빌드 에러가 발생하면 필수 라이브러리(`dnf install` 부분)가 모두 설치되었는지 다시 확인하세요. 특히 OpenSSL 관련 문제는 `openssl-devel` 설치로 대부분 해결됩니다.

* **pyenv 삭제 방법**: `pyenv`를 완전히 제거하려면 설치 폴더를 삭제하고 `.bashrc`에 추가했던 코드를 지우면 됩니다.
    ```bash
    rm -rf ~/.pyenv
    ```
