# SEO/GEO 검증 가이드

이 문서는 구현된 SEO 및 GEO(Generative Engine Optimization) 개선사항을 검증하기 위한 단계별 가이드입니다.

## 1. 로컬 빌드 테스트

### Jekyll 사이트 빌드
```bash
bundle exec jekyll build
bundle exec jekyll serve
```

사이트가 정상적으로 빌드되는지 확인합니다. 빌드 오류가 없어야 합니다.

### 확인 사항
- [ ] 빌드 오류 없음
- [ ] http://localhost:4000 접속 가능
- [ ] 모든 페이지 로드 정상
- [ ] 스타일 깨짐 없음

## 2. 구조화 데이터 검증

### Google Rich Results Test
URL: https://search.google.com/test/rich-results

테스트할 페이지:
- [ ] 홈페이지 (https://chloepark85.github.io/)
- [ ] About 페이지 (https://chloepark85.github.io/about/)
- [ ] 블로그 포스트 (https://chloepark85.github.io/ai-agent/can-grok-bot-make-money/)

### Schema Markup Validator
URL: https://validator.schema.org/

확인할 스키마:
- [ ] Person schema (박현아/Chloe Park)
- [ ] WebSite schema
- [ ] Book schema (3권의 저서)
- [ ] BlogPosting schema (개별 블로그 포스트)

### 예상 결과
- Person: name, alternateName, url, image, jobTitle, alumniOf, address, email, sameAs
- WebSite: name, url, description, inLanguage, author, publisher
- Book: name, author, publisher, datePublished, description, url
- BlogPosting: headline, author, datePublished, dateModified, description, url

## 3. Open Graph 및 Twitter Cards 검증

### Facebook Sharing Debugger
URL: https://developers.facebook.com/tools/debug/

테스트할 페이지:
- [ ] 홈페이지
- [ ] About 페이지
- [ ] Projects 페이지
- [ ] 블로그 포스트

확인 사항:
- [ ] og:title 표시
- [ ] og:description 표시
- [ ] og:image 표시 (이미지 미리보기)
- [ ] og:url 정확함
- [ ] og:type (website 또는 article)
- [ ] og:locale (ko_KR)

### Twitter Card Validator
URL: https://cards-dev.twitter.com/validator

테스트할 페이지:
- [ ] 홈페이지
- [ ] 블로그 포스트

확인 사항:
- [ ] twitter:card (summary_large_image)
- [ ] twitter:site (@chloe_systems)
- [ ] twitter:title 표시
- [ ] twitter:description 표시
- [ ] twitter:image 표시

### LinkedIn Post Inspector
URL: https://www.linkedin.com/post-inspector/

테스트할 페이지:
- [ ] 홈페이지
- [ ] About 페이지

## 4. 메타 태그 확인

브라우저 개발자 도구(F12)에서 `<head>` 섹션 확인:

### 모든 페이지에서 확인할 사항
```html
<!-- Language -->
<html lang="ko">
<link rel="alternate" hreflang="ko" href="..." />

<!-- Canonical -->
<link rel="canonical" href="https://chloepark85.github.io/..." />

<!-- Description -->
<meta name="description" content="..." />

<!-- Open Graph -->
<meta property="og:locale" content="ko_KR" />
<meta property="og:site_name" content="Chloe Park · AI 자동화 실험실" />
<meta property="og:title" content="..." />
<meta property="og:description" content="..." />
<meta property="og:url" content="..." />
<meta property="og:type" content="website" />
<meta property="og:image" content="..." />

<!-- Twitter -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:site" content="@chloe_systems" />
<meta name="twitter:title" content="..." />
<meta name="twitter:description" content="..." />
<meta name="twitter:image" content="..." />

<!-- JSON-LD -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  ...
}
</script>
```

## 5. 크롤러 파일 확인

### robots.txt
URL: https://chloepark85.github.io/robots.txt

확인 사항:
- [ ] 파일 접근 가능 (200 OK)
- [ ] User-agent: * 존재
- [ ] AI 크롤러 허용 (GPTBot, ChatGPT-User, CCBot, anthropic-ai, Claude-Web, PerplexityBot, Google-Extended)
- [ ] Sitemap 위치 명시

### sitemap.xml
URL: https://chloepark85.github.io/sitemap.xml

확인 사항:
- [ ] 파일 접근 가능 (200 OK)
- [ ] 모든 페이지 포함
- [ ] XML 형식 유효

### llms.txt
URL: https://chloepark85.github.io/llms.txt

확인 사항:
- [ ] 파일 접근 가능 (200 OK)
- [ ] 저자 정보 정확
- [ ] 책 정보 3권 모두 포함
- [ ] 프로젝트 정보 포함
- [ ] 학력 정보 포함
- [ ] 소셜 링크 모두 포함

### about.txt
URL: https://chloepark85.github.io/about.txt

확인 사항:
- [ ] 파일 접근 가능 (200 OK)
- [ ] 간결한 프로필 정보
- [ ] 주요 링크 포함

## 6. 이미지 alt 텍스트 확인

브라우저 개발자 도구 또는 페이지 소스에서 모든 `<img>` 태그 확인:

- [ ] 프로필 사진: "박현아 · Chloe Park"
- [ ] 책 표지 이미지:
  - [ ] "나는 AI 에이전트 팀과 일한다 표지"
  - [ ] "AI 에이전트와 자동화 표지"
  - [ ] "인공지능, 말을 걸다 표지"

## 7. 시맨틱 HTML 확인

### 제목 계층 구조
페이지 소스에서 확인:
- [ ] `<h1>` 페이지당 1개만 존재
- [ ] `<h2>` 섹션 제목으로 사용
- [ ] `<h3>` 하위 항목으로 사용
- [ ] 계층 구조 논리적 순서 (h1 → h2 → h3)

### HTML5 시맨틱 태그
- [ ] `<header>`, `<footer>`, `<main>`, `<nav>` 적절히 사용
- [ ] `<article>`, `<section>` 콘텐츠 구조화

## 8. AI 검색 엔진 테스트

### ChatGPT 테스트
프롬프트 예시:
```
"박현아" 또는 "Chloe Park AI agent researcher"에 대해 알려줘
```

확인 사항:
- [ ] 정확한 이름 인식
- [ ] 서울 기반 언급
- [ ] 저서 3권 정보
- [ ] GitHub 프로젝트 언급
- [ ] 사이트 URL 인용

### Perplexity 테스트
검색어:
```
Hyunah Park AI automation Seoul
```

확인 사항:
- [ ] 사이트 인용
- [ ] 정확한 직함 및 설명
- [ ] 저서 정보

### Google Search
검색어:
```
"박현아" AI 에이전트
```

확인 사항:
- [ ] 사이트 검색 결과 노출
- [ ] 제목 및 설명 정확
- [ ] 리치 스니펫 표시 (가능한 경우)

## 9. 모바일 테스트

### Google Mobile-Friendly Test
URL: https://search.google.com/test/mobile-friendly

테스트할 페이지:
- [ ] 홈페이지
- [ ] About 페이지
- [ ] 블로그 포스트

확인 사항:
- [ ] 모바일 친화적
- [ ] 텍스트 읽기 쉬움
- [ ] 탭 타겟 크기 적절
- [ ] 뷰포트 설정 올바름

## 10. 페이지 속도 테스트

### Google PageSpeed Insights
URL: https://pagespeed.web.dev/

테스트할 페이지:
- [ ] 홈페이지

Core Web Vitals 확인:
- [ ] LCP (Largest Contentful Paint)
- [ ] FID (First Input Delay)
- [ ] CLS (Cumulative Layout Shift)

## 11. Google Search Console (배포 후)

사이트 등록:
- [ ] Search Console에 속성 추가
- [ ] sitemap.xml 제출
- [ ] 소유권 확인

모니터링:
- [ ] 인덱싱 상태 확인
- [ ] 검색 성능 모니터링
- [ ] 구조화 데이터 오류 확인
- [ ] 모바일 사용성 확인

## 12. 링크 확인

모든 외부 링크 테스트:
- [ ] 교보문고 책 링크 (3개)
- [ ] GitHub 프로필 및 프로젝트 링크
- [ ] YouTube 채널
- [ ] X (Twitter) 프로필
- [ ] LinkedIn 프로필
- [ ] Instagram 프로필
- [ ] Google Scholar 프로필

## 문제 해결

### 구조화 데이터 오류
- JSON-LD 문법 확인 (쉼표, 따옴표)
- 필수 필드 누락 확인
- URL 절대 경로 사용

### Open Graph 이미지 미표시
- 이미지 URL 절대 경로 확인
- 이미지 크기 권장 사항 (1200x630px)
- 이미지 접근 권한 확인

### robots.txt 인식 안 됨
- 파일 위치 확인 (루트 디렉토리)
- 파일명 정확히 "robots.txt"
- 대소문자 구분 확인

### 메타 태그 중복
- jekyll-seo-tag와 수동 태그 충돌 확인
- 중복 태그 제거

## 추가 자료

- [Schema.org Documentation](https://schema.org/)
- [Open Graph Protocol](https://ogp.me/)
- [Twitter Cards Guide](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards)
- [Google Search Central](https://developers.google.com/search)
- [llms.txt Specification](https://llmstxt.org/)
- [robots.txt Tester](https://www.google.com/webmasters/tools/robots-testing-tool)

---

마지막 업데이트: 2026-08-31
