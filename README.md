# DaleStudy
달레 스터디 운영 저장소

## 서비스 배포 현황

마지막 검증일: 2026-09-24 (직접 HTTP 요청 및 GitHub API로 확인)

| 저장소 | 배포 도메인 | 실제 호스팅 | Cloudflare 계정 | 배포 방식 | 의존성 |
| --- | --- | --- | --- | --- | --- |
| `dalestudy.com` | `dalestudy.com` | Cloudflare Workers | org (`94480614...`) | Workers Builds (main push 자동) | - |
| `graphql` | `graphql.daleseo.workers.dev` | Cloudflare Workers + Containers | 개인 (`86aa2271...`) | 미확인 | - |
| `github` | `github.dalestudy.com` | Cloudflare Workers | org (`94480614...`) | 미확인 | `graphql` |
| `schedule` | `schedule.dalestudy.com` | Cloudflare Workers (D1) | org (`94480614...`) | 미확인 | - |
| `feedback` | `feedback.dalestudy.com` | Cloudflare Workers (D1) | org (`94480614...`) | 미확인 | - |
| `coffee` | `coffee.dalestudy.com` | Cloudflare Workers | org (`94480614...`) | Workers Builds (main push 자동) | Discord API |
| `daleui.com` | `daleui.com`, `www.daleui.com` | Cloudflare Workers (assets) | org (`94480614...`) | 미확인 | - |
| `daleui` | `dalestudy.github.io/daleui/` | GitHub Pages (Storybook) | - | GitHub Actions | - |
| `leaderboard` | `leaderboard.dalestudy.com` | GitHub Pages (Cloudflare는 DNS 프록시만) | - | GitHub Actions | `graphql` (하드코딩된 엔드포인트, `src/api/infra/gitHub/gitHubClient.ts`) |
| `chat` | `chat.dalestudy.com` (프론트) | GitHub Pages (Cloudflare는 DNS 프록시만) | - | GitHub Actions | 백엔드: `dalestudy-chat-backend.fly.dev` (Fly.io, `/health` 정상) |
| `homepage` | `leetcode.dalestudy.com` | GitHub Pages (legacy build) | - | 미확인 | - |

"배포 방식 미확인"은 대시보드에서 수동 배포인지 Workers Builds GitHub 연동인지 wrangler 설정만으로 판단이 안 되는 항목입니다. 해당 계정 접근 권한자 확인이 필요합니다.

### 배포되지 않는 저장소

`leetcode-study`, `community-manager`, `ai-study`, `DaleStudy`, `blog-study`, `hackathon-blog`, `coffee-chat`, `english-interview`, `.github`, `mcp`(삭제됨)는 자체 배포가 없거나 콘텐츠/자동화 전용 저장소입니다. `skills`는 `skills.sh` 서드파티 플랫폼에 게시되며 우리 인프라가 아닙니다.

## 알려진 불일치 (조치 필요)

- [ ] `github` 저장소 About 링크가 `graphql.daleseo.workers.dev`(다른 저장소 주소)로 설정되어 있음. `github.dalestudy.com`으로 수정 필요
- [ ] `daleui` 저장소 About 링크가 `www.daleui.com`(`daleui.com` 저장소 주소)로 설정되어 있음. 실제 배포처(`dalestudy.github.io/daleui/`)로 수정 필요
- [ ] `dalestudy.com` 저장소 About 링크가 죽은 `beta.dalestudy.com`을 가리킴. `dalestudy.com`으로 수정 필요
- [ ] `leetcode-study` 저장소 About 링크(`www.dalestudy.com`)가 배포되지 않는 저장소에 붙어있음. 제거 필요
- [ ] `homepage` 저장소의 GitHub Pages가 `leetcode.dalestudy.com`에 연결되어 있는데 실제 서빙되는 콘텐츠는 옛 홈페이지(`<title>DaleStudy</title>`)임. 도메인 이름과 콘텐츠가 서로 맞지 않아 의도된 상태인지 사고인지 확인 필요
- [ ] `graphql`이 org 공용 Cloudflare 계정이 아닌 개인 계정에 배포되어 있음. org 계정으로 이전할지 결정 필요

## Orphan / 정리 후보 (임의 종료 금지)

살아있지만 참조가 없어 보이는 배포입니다. 실제로 쓰이고 있을 수 있으므로 여기 기록만 하고, 정리는 별도 이슈에서 논의합니다.

- `dalestudy.fly.dev` — `graphql.daleseo.workers.dev`와 동일한 GraphQL 스키마로 응답. 구버전 Fly 배포로 추정되며 org 코드에서 참조하는 곳 없음
- `graphql.dalestudy.com` — 커스텀 도메인 미설정(DNS 레코드 없음). 필요 여부 재검토 필요

## 갱신 방법

이 표는 수동으로 작성되었으며 낡기 쉽습니다. 다음 방식의 자동 검증을 제안합니다.

- 이 저장소에 정기 실행(cron) GitHub Actions 워크플로우를 추가해, 표에 적힌 도메인들에 주기적으로 요청을 보내고 상태 코드/응답 지문이 바뀌면 이슈를 열거나 코멘트로 알림
- 최초 버전은 복잡한 모니터링 스택 없이, 이번 조사에 쓴 curl 스크립트를 워크플로우로 옮기는 정도로 충분함
- 문서가 우선 안정화된 뒤 자동화를 추가하는 순서를 권장

관련 이슈: [DaleStudy/dalestudy.com#8](https://github.com/DaleStudy/dalestudy.com/issues/8)
