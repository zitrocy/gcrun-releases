# gcrun — Cloud Run TUI

A fast terminal UI for Google Cloud Run. Drill into services, tail logs in real
time, inspect revisions/traffic/env, view raw YAML, and move traffic between
revisions — without leaving the terminal. Authentication reuses your existing
`gcloud` login.

GCP Cloud Run을 터미널에서 빠르게 둘러보는 TUI입니다. 기존 `gcon` 계열 도구의 세 가지
불편을 정면으로 고쳤습니다.

- **Esc로는 절대 종료되지 않습니다.** Esc는 항상 한 단계 뒤로만 갑니다. 종료는 최상위에서 `q` 또는 `Ctrl+C`.
- **`?` 도움말이 충실합니다.** 화면별 키 전부를 그룹으로 정리.
- **전용 로그 뷰어.** 실시간 tail, 심각도 필터, 검색 하이라이트, 줄바꿈/타임스탬프 토글.

이 저장소는 **배포 전용**입니다. 여기에는 실행 바이너리와 릴리즈 노트만 올라오고,
소스 코드는 공개하지 않습니다.

---

## 설치

`gcrun`은 단일 실행 파일입니다. 아래 한 줄로 내려받아 실행 권한을 주면 끝입니다.
Go 툴체인은 필요 없고, **`gcloud` CLI만 있으면 됩니다.**

### macOS (Apple Silicon, M1 이상)

```sh
curl -L -o gcrun https://github.com/zitrocy/gcrun-releases/releases/download/v0.1.0/gcrun-darwin-arm64 && chmod +x gcrun
sudo mv gcrun /usr/local/bin/   # PATH에 넣기 (선택)
```

### macOS (Intel)

```sh
curl -L -o gcrun https://github.com/zitrocy/gcrun-releases/releases/download/v0.1.0/gcrun-darwin-amd64 && chmod +x gcrun
sudo mv gcrun /usr/local/bin/   # PATH에 넣기 (선택)
```

### Linux (x86_64)

```sh
curl -L -o gcrun https://github.com/zitrocy/gcrun-releases/releases/download/v0.1.0/gcrun-linux-amd64 && chmod +x gcrun
sudo mv gcrun /usr/local/bin/   # PATH에 넣기 (선택)
```

### Linux (arm64)

```sh
curl -L -o gcrun https://github.com/zitrocy/gcrun-releases/releases/download/v0.1.0/gcrun-linux-arm64 && chmod +x gcrun
sudo mv gcrun /usr/local/bin/   # PATH에 넣기 (선택)
```

### 설치 확인 · 체크섬

```sh
gcrun --version     # gcrun v0.1.0
```

모든 바이너리의 SHA-256은 릴리즈마다 `checksums.txt`로 함께 올라갑니다.

```sh
curl -LO https://github.com/zitrocy/gcrun-releases/releases/download/v0.1.0/checksums.txt
sha256sum -c checksums.txt        # macOS: shasum -a 256 -c checksums.txt
```

> ⚠ macOS에서 브라우저로 직접 내려받은 경우(터미널 `curl`은 해당 없음) 격리 속성이
> 붙어 실행이 막힐 수 있습니다. 그때는 `xattr -d com.apple.quarantine ./gcrun` 후 실행하세요.
> Apple Silicon용 바이너리는 ad-hoc 서명되어 있어 별도 서명 없이 실행됩니다.

---

## 요구 사항

- Google Cloud CLI (`gcloud`) — Cloud Run 조회 권한이 있는 계정으로 로그인되어 있어야 함
- (선택) bastion 진입: `roles/iap.tunnelResourceAccessor`, OS Login 권한, IAP 방화벽(`35.235.240.0/20:22`)

```sh
gcloud auth login
gcloud config set project YOUR_PROJECT_ID
```

## 사용법

```sh
gcrun                 # gcloud 활성 프로젝트 사용
gcrun -p my-project   # 프로젝트 지정
gcrun --version
gcrun serve           # TUI 대신 JSON API 서버 (127.0.0.1:8787)
gcrun serve --addr 0.0.0.0:8787 --origin http://localhost:3000
```

### 화면 흐름

```
Services (목록)
  ├─ Enter ─→ Detail (개요·스케일·컨테이너·env·트래픽)
  │            ├─ v ─→ Revisions (R로 롤백)
  │            ├─ L ─→ Logs
  │            ├─ y ─→ YAML
  │            └─ t ─→ 트래픽 100% 최신으로
  ├─ L ─→ Logs
  └─ y ─→ YAML
```

### 주요 키 (전체는 앱에서 `?` — 스크롤 가능)

| 화면 | 키 | 동작 |
|------|----|------|
| 전역 | `?` / `q` / `Esc` / `r` | 도움말 / 종료 / 뒤로 / 새로고침 |
| 목록 | `Enter` `L` `y` `o` `/` | 상세 / 로그 / YAML / 브라우저 / 필터 |
| 상세 | `v` `L` `y` `t` | 리비전 / 로그 / YAML / 트래픽→최신 |
| 리비전 | `R` | 선택 리비전으로 롤백 (확인 후) |
| 로그 | `f` `s` `/` `w` `T` `c` | tail / 심각도 / 검색 / 줄바꿈 / 시간 / 비우기 |
| 셸/접속 | `b` `p` `S` | bastion 진입 / 프록시 / 컨텍스트 셸 |
| VPC 접속 | `Enter` | 선택 인스턴스로 IAP SSH |

롤백·트래픽 이동 같은 변경 작업은 모두 실행 전 확인 다이얼로그를 거칩니다.

---

## 릴리즈

| 버전 | 날짜 | 바이너리 |
|------|------|----------|
| [v0.1.0](https://github.com/zitrocy/gcrun-releases/releases/tag/v0.1.0) | 2026-11-18 | darwin-arm64 · darwin-amd64 · linux-amd64 · linux-arm64 |

## 라이선스

**Proprietary — Copyright (c) 2026 zitrocy. All Rights Reserved.**

- **무료 티어** — 이 저장소에서 배포하는 바이너리는 개인 및 내부 업무용으로 무료로 실행할 수 있습니다.
- **Pro 기능**(canary·watch·advisor 리포트) — 유효한 라이선스 키가 필요합니다.
- **소스 코드** — 비공개이며, 소스에 대한 사용 권한은 부여되지 않습니다.
- **금지** — 역공학·디컴파일, 라이선스 검사 우회, 무단 재배포, 상업적 재판매.

라이선스·상업적 이용 문의: https://github.com/zitrocy
