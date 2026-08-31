---
title: "블로그 이미지, 에이전트가 저작권까지 확인하고 넣게 만들었습니다"
date: 2026-08-31 22:30:00 +09:00
categories:
  - projects
tags:
  - MCP
  - AI-agent
  - 저작권
  - picright
excerpt: "이미지 검색·라이선스 검증·출처 표기·증빙 기록까지 한 번에 하는 MCP 서버. 스톡 사진, 스크린샷, 다이어그램 — 세 가지 사용 근거를 구분해서 기록합니다."
toc: true
toc_sticky: true
---

글을 다 쓰고 나면 이미지가 문제입니다. 아무 이미지나 가져다 쓰면 저작권이 걱정되고, 제대로 하려면 검색하고 → 라이선스 확인하고 → 출처 표기 달고 → 나중을 위해 어디서 뭘 가져왔는지 기록까지 해야 합니다. 글 쓰는 시간보다 이게 더 걸릴 때도 있습니다. 그래서 에이전트에게 통째로 시켰습니다.

**picright** — license-aware image search & insert MCP server.

```bash
claude mcp add picright -- npx -y picright-mcp
```

## 이미지를 쓰는 근거는 하나가 아닙니다

만들다 보니 핵심은 검색이 아니라 **"이 이미지를 쓸 수 있는 근거(basis)가 무엇인가"**였습니다. 블로그에 들어가는 이미지는 사실 세 종류입니다.

| basis | 무엇 | 근거 | 의무 |
|---|---|---|---|
| `license` | 스톡/CC 사진 (Openverse·Pixabay·Pexels) | 라이선스 | 라이선스에 따라 출처 표기 |
| `quotation` | 웹페이지 스크린샷 | 인용 (저작권법 §28) | 출처 URL + 캡처 날짜 필수 |
| `original` | 글 내용으로 그린 다이어그램 | 내 창작물 | 없음 |

`search_images`는 정책(상업적 사용, 수정 가능, SA 제외…)에 걸리는 라이선스를 후보에서 미리 떨어뜨리고, `screenshot_url`은 그 페이지를 **본문에서 다루고 있을 때만** 쓰라고 툴 설명에 못 박은 뒤 출처+날짜 크레딧을 강제로 답니다. `render_diagram`은 문단의 구조를 Mermaid로 그려서 저작권 문제가 아예 없는 이미지를 만듭니다. 개념 설명 글에는 어울리지도 않는 스톡 사진을 억지로 넣는 것보다 다이어그램이 나은 경우가 많았습니다.

![문단별 이미지 타입 결정 흐름](/assets/images/picright/picright-basis-decision.png)

*illustrate 워크플로우가 문단마다 내리는 판단*

## 실제로 돌린 결과

바로 이전 글([KGT 소개](/projects/korea-ground-truth-mcp/))에 `illustrate` 워크플로우를 돌렸습니다. 에이전트가 문단마다 타입을 골랐습니다:

- 도입부 → **스톡** (검증 도장 사진, Pexels, 출처 표기 불요)
- 서비스 링크 다음 → **스크린샷** (KGT 홈페이지, 출처+캡처일 크레딧 자동)
- 툴 표 다음 → **다이어그램** (기관 다섯 곳 → One MCP 구조도)
- 설계 원칙 다음 → **다이어그램** (에이전트가 사람에게 결제를 요청하는 시퀀스)

한글 라벨 다이어그램도 문제없이 렌더됩니다. 삽입 순서는 마지막 블록부터 — 앞에서부터 넣으면 블록 인덱스가 밀리기 때문인데, 이런 것까지 `illustrate` 프롬프트에 들어 있어서 에이전트가 알아서 합니다.

## 증빙이 남습니다

모든 삽입은 이미지 폴더의 `provenance.json`에 기록됩니다: 어떤 이미지를, 어떤 근거로, **삽입 시점에 어떤 라이선스를 확인했는지**. 라이선스는 나중에 바뀔 수 있으니 "그때 검증했다"는 기록 자체가 증빙입니다. 출처 표기가 필요한 라이선스인데 크레딧을 빼달라고 하면 도구가 거부합니다.

```json
{
  "file": "images/kgt-hero.jpg",
  "basis": "license",
  "license": { "id": "Pexels License", "attribution_required": false },
  "checked_at": "2026-08-31T..."
}
```

## 만들면서 배운 것

- **로컬에선 되는데 배포하면 죽는 버그**: Mermaid를 CDN에서 로드했더니 Fly.io에서만 렌더가 실패했습니다. Chromium은 네트워크가 느리다고 판단하면 `document.write`로 삽입된 cross-origin 스크립트를 실제로 차단합니다 — Playwright의 `setContent`가 내부적으로 `document.write`를 씁니다. 로컬은 네트워크가 빨라서 경고만 내고 통과한 겁니다. mermaid를 npm 의존성으로 번들해서 인라인 주입하는 걸로 해결했습니다.
- **원격 클라이언트는 파일시스템이 없습니다**: Grok·ChatGPT 같은 remote MCP 클라이언트를 위해 `render_snippet`(크레딧 포함 마크다운 조각 반환)을 따로 만들었고, 스크린샷·다이어그램도 `target_file` 없이 부르면 PNG를 인라인 이미지로 돌려줍니다. "문서에 삽입"이라는 핵심 가치는 로컬 stdio에서만 완성됩니다.
- MCP Registry는 npm 패키지의 `mcpName` 필드로 소유권을 검증합니다. publish 순서를 틀리면 다시 올려야 합니다.

## 솔직한 한계

이 도구는 저작권 책임을 없애주지 않습니다. 특히 스크린샷의 인용 판단 — "본문이 그 페이지를 실제로 다루는가" — 은 결국 사람 몫이고, 도구는 근거와 출처를 기록할 뿐입니다. Openverse는 키 없이 되는 대신 검색 품질이 들쭉날쭉해서 Pixabay/Pexels 키를 넣는 편이 좋습니다. 원격(HTTP) 모드는 만들어두긴 했지만 사용자별 키 발급이 없어서 아직 개인용입니다 — 반응이 있으면 KGT에서 만든 계정 발급 구조를 옮겨 붙일 생각입니다.

## 써보세요

```bash
claude mcp add picright -- npx -y picright-mcp
npx playwright install chromium   # 스크린샷·다이어그램 쓸 때 한 번만
```

세션에서 *"이 글에 어울리는 이미지를 저작권 확인해서 넣어줘"* 라고 하거나 `/picright:illustrate`를 쓰면 됩니다.

- 코드: <https://github.com/ChloePark85/picright-mcp>
- npm: <https://www.npmjs.com/package/picright-mcp>
- MCP Registry: `io.github.ChloePark85/picright`

어디서 깨지는지, 어떤 라이선스 정책이 더 필요한지 알려주세요. 메일(hapark85@gmail.com)이나 [GitHub 이슈](https://github.com/ChloePark85/picright-mcp/issues)로 주시면 됩니다.
