# gcrun — Cloud Run TUI & Chrome side panel

A fast terminal UI for Google Cloud Run, plus a Chrome side panel that puts the
service you are looking at next to the console. Drill into services, tail logs
in real time, inspect revisions/traffic/env, view raw YAML, move traffic between
revisions — and drive a guarded canary from the browser. Authentication reuses
your existing `gcloud` login.

GCP Cloud Run을 터미널에서 빠르게 둘러보는 TUI입니다. 기존 `gcon` 계열 도구의 세 가지
불편을 정면으로 고쳤습니다.

- **Esc로는 절대 종료되지 않습니다.** Esc는 항상 한 단계 뒤로만 갑니다. 종료는 최상위에서 `q` 또는 `Ctrl+C`.
- **`?` 도움말이 충실합니다.** 화면별 키 전부를 그룹으로 묶어 정리.
- **전용 로그 뷰어.** 실시간 tail, 심각도 필터, 검색 하이라이트, 줄바꿈/타임스탬프 토글.

이 저장소는 **배포 전용**입니다. 여기에는 실행 바이너리와 릴리즈 노트만 올라오고,
소스 코드는 공개하지 않습니다.

- 제품 페이지: https://zitrocy.github.io/gcrun-releases/
- 개인정보처리방침: https://zitrocy.github.io/gcrun-releases/privacy.html
- 약관·환불: https://zitrocy.github.io/gcrun-releases/terms.html

---

## 설치

`gcrun`은 단일 실행 파일입니다. 아래 한 줄로 내려받아 실행 권한을 주면 끝입니다.
Go 툴체인은 필요 없고, **`gcloud` CLI만 있으면 됩니다.**

### macOS (Apple Silicon, M1 이상)

```sh
curl -L -o gcrun https://github.com/zitrocy/gcrun-releases/releases/latest/download/gcrun-darwin-arm64 && chmod +x gcrun
sudo mv gcrun /usr/local/bin/   # PATH에 넣기 (선택)
```

### macOS (Intel)

```sh
curl -L -o gcrun https://github.com/zitrocy/gcrun-releases/releases/latest/download/gcrun-darwin-amd64 && chmod +x gcrun
sudo mv gcrun /usr/local/bin/
```

### Linux (x86_64)

```sh
curl -L -o gcrun https://github.com/zitrocy/gcrun-releases/releases/latest/download/gcrun-linux-amd64 && chmod +x gcrun
sudo mv gcrun /usr/local/bin/
```

### Linux (arm64)

```sh
curl -L -o gcrun https://github.com/zitrocy/gcrun-releases/releases/latest/download/gcrun-linux-arm64 && chmod +x gcrun
sudo mv gcrun /usr/local/bin/
```

### 설치 확인 · 체크섬

```sh
gcrun --version
```

모든 바이너리의 SHA-256은 릴리즈마다 `checksums.txt`로 함께 올라갑니다.

```sh
curl -LO https://github.com/zitrocy/gcrun-releases/releases/latest/download/checksums.txt
sha256sum -c checksums.txt        # macOS: shasum -a 256 -c checksums.txt
```

---

## Chrome 사이드패널 (선택 · Pro)

Cloud Run 콘솔 옆에 붙는 사이드패널입니다.

```sh
gcrun serve          # 로컬 브릿지 (127.0.0.1:8787), 선택: --token auto
```

1. Chrome 웹 스토어에서 **gcrun — Cloud Run Sidepanel** 설치
2. Cloud Run 서비스 페이지(`console.cloud.google.com/run/...`)를 열고 툴바 아이콘 클릭
3. 패널이 현재 탭의 URL에서 프로젝트·리전·서비스를 읽어 리비전·트래픽·명령어를 보여줍니다

> 스토어 등록 심사 전에는 릴리즈에 첨부된 `gcrun-chrome-extension.zip`을 내려받아 압축을 풀고
> `chrome://extensions` → 개발자 모드 → **압축해제된 확장 프로그램을 로드**로 같은 기능을 쓸 수 있습니다.

**무료**: 패널의 조회 화면, CLI·TUI 전 기능(`canary`·`watch`·`advisor` 포함)
**Pro**: 패널의 원클릭 카나리(10→30→100% + 5xx 가드, 자동 롤백·드라이런), 원클릭 롤백,
실시간 5xx·인스턴스 감시, 멀티 프로젝트 Fleet 뷰, 리포트 내보내기

Pro 라이선스는 Polar에서 판매합니다 — https://buy.polar.sh/polar_cl_t38Ckb64tNBTFleo2AMaJ2U8mfyMM0rmYovcB4ZqHoY

```sh
gcrun activate GCRUN-XXXX-XXXX-XXXX      # 키 활성화 (기기 5대까지)
gcrun license status
```

확장은 **어떤 데이터도 수집·전송하지 않습니다.** 모든 요청은 사용자 컴퓨터의
127.0.0.1 브릿지로만 향하고, GCP 자격증명은 CLI가 쓰는 `gcloud` 로그인 그대로입니다.

---

## 변경 내역

릴리즈마다 GitHub Releases의 **release notes**에 정리되어 있습니다.

## 라이선스

**Proprietary — Copyright (c) 2026 zitrocy. All Rights Reserved.**

- **CLI·TUI** — 이 저장소에서 배포하는 바이너리는 개인 및 내부 업무용으로 무료로 실행할 수 있습니다.
- **Extension Pro** — Chrome 사이드패널의 Pro 기능은 유효한 라이선스 키(기기 5대)가 필요합니다.
- **소스 코드** — 비공개이며, 소스에 대한 사용 권한은 부여되지 않습니다.
- **금지** — 역공학·디컴파일, 라이선스 검사 우회, 무단 재배포, 상업적 재판매.

라이선스·상업적 이용 문의: https://github.com/zitrocy
