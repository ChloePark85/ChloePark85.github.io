---
title: "AI 에이전트가 한국 사업자를 직접 검증하게 만들었습니다"
date: 2026-08-27 10:00:00 +09:00
categories:
  - projects
tags:
  - MCP
  - AI-agent
  - 공공데이터
  - Korea-Ground-Truth
excerpt: "국세청·DART·행안부·국토부·법제처 API를 하나의 MCP로 묶고, 고객을 사람이 아니라 AI 에이전트로 가정하고 설계한 SaaS 이야기."
toc: true
toc_sticky: true
---

에이전트에게 "이 업체랑 계약해도 돼?"라고 물으면, 답을 내려면 국세청 키 발급하고, DART 붙이고, 주소 API 붙이고… 기관 다섯 곳 API를 각각 연동해야 합니다. 인증키·XML·한글 필드명·법정동코드까지 전부 다릅니다. 그게 귀찮아서 하나로 묶었습니다.

**Korea Ground-Truth (KGT)** — Official Korean data for AI agents. One API. One MCP.
👉 <https://kr-groundtruth-mcp.vercel.app>

## 에이전트가 실제로 하는 일

Claude Code에 한 줄 붙이고 이렇게 물으면:

> "삼성전자(124-81-00998)랑 계약하려는데 사업자 상태랑 법인정보, 주소 확인해줘."

에이전트가 툴 4개를 순서대로 호출해서 이렇게 답합니다 (실제 응답):

> ✅ 사업자 상태: 계속사업자 (일반과세자) — 국세청
> ✅ 법인: 삼성전자(주), DART 00126380, KOSPI 005930
> ✅ 대표자: 전영현, 노태문 · 법인등록번호 1301110006246 · 설립일 1969-01-13
> ✅ 사업자번호 일치: DART 등록번호 = 조회한 번호
> ✅ 본점 주소 정규화: 경기도 수원시 영통구 삼성로 129 (매탄동), 16677

비용은 6 credits(60원). 툴 호출 하나하나의 실제 요청·응답은 [전체 trace](https://kr-groundtruth-mcp.vercel.app/examples/company-verification)에 그대로 올려두었습니다. 부동산([단지명 → 실거래가](https://kr-groundtruth-mcp.vercel.app/examples/real-estate))과 법령([현행 법령·시행일 확인](https://kr-groundtruth-mcp.vercel.app/examples/legal)) 예시도 있습니다.

## 툴 8개

| 툴 | 출처 | 비용 |
|---|---|---|
| `verify_business_registration` — 사업자 상태(계속/휴업/폐업) + 진위확인 | 국세청 | 2 credits |
| `search_address` — 도로명주소 정규화, 우편번호, 법정동코드 | 행정안전부 | 1 |
| `search_corporation` / `lookup_corporation` — 법인 검색, 기업개황 | 금융감독원 DART | 1 / 2 |
| `apartment_trade_prices` — 아파트 매매 실거래가 (원 단위 정수) | 국토교통부 | 3 |
| `search_law` — 현행법령 검색 (시행일 포함) | 법제처 | 2 |
| `get_balance` / `get_pricing` | — | 무료 |

1 credit = 10원. 모든 응답의 `meta.source`에 출처 기관이 명시되고, 가공은 정규화(영문 snake_case + 원문 `_raw`)뿐입니다.

## 고객을 에이전트로 가정하고 만든 부분

이 프로젝트에서 제일 재미있었던 지점입니다. 사람이 들어와서 UI를 써보는 SaaS가 아니라, **개발자가 한 번 연결하면 에이전트가 계속 호출하는 인프라**라서 설계가 달라집니다.

- **가입 폼이 없습니다.** `POST /v1/accounts {"email"}` 한 번이면 키 + 무료 200 credits (런치 기간). 사람 개입 없이 에이전트가 스스로 가입할 수 있습니다.
- **가격이 툴 설명 안에 있습니다.** 모든 툴 설명 첫 줄이 `Cost: 2 credits/call`. 에이전트가 가격을 읽고 계획합니다.
- **응답마다 `meta.cost`, `meta.balance_remaining`.** 잔액이 부족하면 402와 함께 충전 URL을 돌려주고, 에이전트가 그 링크를 사람(운영자)에게 건네면 사람이 카드로 충전합니다. "에이전트가 사람에게 결제를 요청하는" 루프가 제품 안에 들어 있습니다.
- **업스트림 실패는 자동 환불**, 장부(ledger)는 append-only.
- **툴 목록은 키 없이 조회 가능.** `llms.txt` / `openapi.json` / `pricing.json`을 제공해 에이전트가 문서를 직접 읽습니다.
- 공식 MCP Registry(`io.github.ChloePark85/kr-groundtruth`)와 Smithery(`hapark85/kr-groundtruth`)에 등재.

연결은 한 줄입니다:

```bash
curl -X POST https://kr-groundtruth-mcp.vercel.app/v1/accounts -H 'content-type: application/json' -d '{"email":"you@example.com"}'
claude mcp add --transport http kgt https://kr-groundtruth-mcp.vercel.app/api/mcp --header "Authorization: Bearer kgt_live_..."
```

## 어떻게 만들었나

스택은 Next.js + Vercel `mcp-handler`(stateless Streamable HTTP) + Supabase Postgres + 토스페이먼츠. 과금은 Postgres 함수 하나에서 행 잠금(`SELECT … FOR UPDATE`)으로 처리해서, 동시호출 50개 race 테스트에서 정확히 잔액만큼만 차감되는 것을 확인했습니다.

만들면서 배운 것 몇 가지:

- 에이전트용 문서는 마케팅 카피가 아니라 `llms.txt` + OpenAPI + 복붙 가능한 curl이었습니다.
- MCP 레지스트리 등록은 생각보다 쉬웠지만, 스키마 버전·네임스페이스 대소문자·인증 방식에서 여러 번 튕겼습니다. 특히 401 응답에 OAuth 메타데이터를 광고하면 레지스트리가 OAuth 서버로 오인합니다. 툴 목록 조회는 키 없이 열어두는 편이 레지스트리 스캔에도, 에이전트 온보딩에도 낫습니다.
- 공공 API 다섯 곳은 인증 방식(query key / POST body / OC 이메일 ID), 포맷(JSON/XML), 승인 절차(자동/1~2일)가 전부 달랐습니다. 이 마찰이 곧 제품입니다.

코드는 공개되어 있습니다: <https://github.com/ChloePark85/kr-groundtruth-mcp>

## 솔직한 한계

공공데이터 래핑이라 해자는 얇습니다. 지금 파는 건 "정규화 + 묶음 + 에이전트 친화적 과금"이고, 반응이 있으면 휴폐업 변경 알림, 법인등기처럼 무료로는 못 얻는 데이터를 얹을 생각입니다. 에이전트가 자율적으로 결제하는 경우도 아직은 드뭅니다 — 실제 구매자는 그 에이전트를 만드는 개발자입니다. 그래서 지표도 홈페이지 방문자가 아니라 **주간 활성 API 키**와 "첫 호출까지 걸린 시간"으로 보고 있습니다.

## 부탁

한국 시장용 에이전트나 자동화를 만드시는 분, 워크플로 하나만 붙여보고 어디서 깨지는지 알려주세요. 크레딧 넉넉히 드리겠습니다. 메일(hapark85@gmail.com)이나 [GitHub 이슈](https://github.com/ChloePark85/kr-groundtruth-mcp/issues)로 주시면 됩니다.
