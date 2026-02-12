# uv InstallationGuide

`uv`는 Astral에서 개발한 **초고속 Python 패키지 및 가상환경 관리
도구**로, `pip`, `pip-tools`, `virtualenv`를 하나로 대체합니다.

------------------------------------------------------------------------

## 1. uv 설치

### macOS / Linux

``` bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.cargo/env
```

### Homebrew (macOS)

``` bash
brew install uv
```

### Windows (PowerShell)

``` powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

설치 확인:

``` bash
uv --version
```

------------------------------------------------------------------------

## 2. 프로젝트 초기화

``` bash
uv init my-project
cd my-project
```

생성 파일: - `pyproject.toml` - `.python-version`

------------------------------------------------------------------------

## 3. 가상환경 생성 및 활성화

``` bash
uv venv
```

``` bash
source .venv/bin/activate   # macOS/Linux
.venv\Scripts\activate    # Windows
```

------------------------------------------------------------------------

## 4. 패키지 관리

### 패키지 추가

``` bash
uv add fastapi
```

### 개발용 의존성

``` bash
uv add --dev pytest
```

### 패키지 삭제

``` bash
uv remove fastapi
```

------------------------------------------------------------------------

## 5. Python 실행

``` bash
uv run python main.py
uv run pytest
```

------------------------------------------------------------------------

## 6. 의존성 동기화 (권장)

``` bash
uv sync
```

-   `pyproject.toml` + lock 기준
-   정확한 환경 재현

------------------------------------------------------------------------

## 7. requirements.txt 생성하기 ⭐

### 7.1 현재 가상환경 기준 (pip 방식)

``` bash
uv pip freeze > requirements.txt
```

-   빠르고 간단
-   로컬 공유용

------------------------------------------------------------------------

### 7.2 pyproject.toml / lock 기준 (권장)

``` bash
uv export --format requirements-txt > requirements.txt
```

#### 운영 환경용 (dev 제외)

``` bash
uv export --format requirements-txt --without-dev > requirements.txt
```

#### 개발 환경 포함

``` bash
uv export --format requirements-txt --with-dev > requirements.txt
```

------------------------------------------------------------------------

### 7.3 가상환경 없이 생성

``` bash
uv export -o requirements.txt
```

------------------------------------------------------------------------

## 8. requirements.txt로 설치

``` bash
uv pip install -r requirements.txt
```

------------------------------------------------------------------------

## 9. pip vs uv 명령 비교

  pip              uv
  ---------------- -------------------
  pip install      uv add
  pip uninstall    uv remove
  pip freeze       uv pip freeze
  virtualenv       uv venv
  pip install -r   uv pip install -r

------------------------------------------------------------------------

## 10. 권장 사용 시나리오

-   FastAPI / Django 백엔드
-   AI / ML 프로젝트
-   Docker / CI/CD
-   로컬 LLM, Agent 서버

------------------------------------------------------------------------

## 11. 정리

-   개발: `uv add`, `uv run`, `uv sync`
-   배포: `uv export --without-dev`
-   공유: `requirements.txt`

------------------------------------------------------------------------

## 참고

-   Docs: https://docs.astral.sh/uv/
-   GitHub: https://github.com/astral-sh/uv

------------------------------------------------------------------------

Happy coding 🚀
