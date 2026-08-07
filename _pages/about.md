---
title: "About"
permalink: /about/
layout: splash
author_profile: false
---

<div class="chloe-home">

  <section class="chloe-hero chloe-hero--compact">
    <div class="chloe-hero__inner">
      <div class="chloe-hero__intro">
        <span class="chloe-eyebrow">About</span>
        <h1 class="chloe-hero__title">
          <span class="chloe-accent">박현아</span>,<br/>
          AI 팀을 설계합니다.
        </h1>
        <p class="chloe-hero__lead">
          오디오 콘텐츠 스타트업을 창업·운영하며 TTS와 멀티 에이전트 기반
          콘텐츠 자동화를 직접 설계했습니다. 지금은 n8n과 Claude API로
          <strong>1인 기업이 AI 팀과 협업하는 워크플로</strong>를 실험하고 기록합니다.
        </p>
        <div class="chloe-hero__actions">
          <a class="chloe-btn chloe-btn--primary" href="/projects/">Projects <span aria-hidden="true">→</span></a>
          <a class="chloe-btn chloe-btn--ghost" href="/contact/">Contact</a>
        </div>
      </div>

      <div class="chloe-hero__portrait chloe-hero__portrait--sm">
        <div class="chloe-hero__glow" aria-hidden="true"></div>
        <div class="chloe-hero__avatar">
          <img src="https://github.com/ChloePark85.png" alt="박현아 · Chloe Park" loading="lazy" />
        </div>
      </div>
    </div>
  </section>

  <section class="chloe-section chloe-section--muted">
    <header class="chloe-section__head">
      <span class="chloe-eyebrow">Background</span>
      <h2 class="chloe-section__title">학력과 연구</h2>
      <p class="chloe-section__sub">HCI와 대화형 AI에 기반한 학술 훈련</p>
    </header>

    <div class="chloe-facts">
      <div class="chloe-fact">
        <div class="chloe-fact__year">Ph.D. 수료</div>
        <div class="chloe-fact__title">서울대학교 HCI+D 연구실</div>
        <div class="chloe-fact__desc">인간-AI 에이전트 상호작용 연구</div>
      </div>
      <div class="chloe-fact">
        <div class="chloe-fact__year">M.S.</div>
        <div class="chloe-fact__title">KAIST 문화기술대학원</div>
        <div class="chloe-fact__desc">문화·기술 융합 석사</div>
      </div>
      <div class="chloe-fact">
        <div class="chloe-fact__year">B.A.</div>
        <div class="chloe-fact__title">연세대학교 신문방송학과</div>
        <div class="chloe-fact__desc">커뮤니케이션·미디어 학사</div>
      </div>
    </div>
  </section>

  <section class="chloe-section">
    <header class="chloe-section__head">
      <span class="chloe-eyebrow">Publications</span>
      <h2 class="chloe-section__title">논문 이력</h2>
      <p class="chloe-section__sub">
        HCI · 대화형 AI · 미디어 연구 논문 19편 ·
        <a href="https://scholar.google.com/citations?user=OUFAsNIAAAAJ&hl=ko" target="_blank" rel="noopener">Google Scholar에서 보기 →</a>
      </p>
    </header>

    <ol class="chloe-publications">
      {% for publication in site.data.publications %}
      <li class="chloe-publication">
        <div class="chloe-publication__year">{{ publication.year }}</div>
        <div class="chloe-publication__body">
          <a class="chloe-publication__title" href="{{ publication.url }}" target="_blank" rel="noopener">
            {{ publication.title }} <span aria-hidden="true">↗</span>
          </a>
          <div class="chloe-publication__authors">{{ publication.authors }}</div>
          <div class="chloe-publication__venue">{{ publication.venue }}</div>
        </div>
      </li>
      {% endfor %}
    </ol>
  </section>

  <section class="chloe-section">
    <header class="chloe-section__head">
      <span class="chloe-eyebrow">Books</span>
      <h2 class="chloe-section__title">저서</h2>
      <p class="chloe-section__sub">6년간의 대화형 AI · 에이전트 연구를 담은 세 권.</p>
    </header>

    <div class="chloe-book-grid">
      <article class="chloe-book">
        <a class="chloe-book__cover chloe-book__cover--indigo"
           href="https://product.kyobobook.co.kr/detail/S000220511750"
           target="_blank" rel="noopener"
           aria-label="나는 AI 에이전트 팀과 일한다 · 교보문고에서 보기">
          <span class="chloe-book__label">신간</span>
          <img class="chloe-book__image" src="/assets/images/books/ai-agent-team.jpg" alt="나는 AI 에이전트 팀과 일한다 표지" loading="lazy" />
        </a>
        <div class="chloe-book__meta">제이펍 · 2026</div>
        <p class="chloe-book__desc">n8n과 Claude·OpenAI로 고객 지원, 콘텐츠, 데이터, 마케팅 네 AI 팀을 직접 구축하는 실전 가이드.</p>
      </article>

      <article class="chloe-book">
        <a class="chloe-book__cover chloe-book__cover--violet"
           href="https://product.kyobobook.co.kr/detail/S000215723795"
           target="_blank" rel="noopener"
           aria-label="AI 에이전트와 자동화 · 교보문고에서 보기">
          <img class="chloe-book__image" src="/assets/images/books/ai-agents-automation.jpg" alt="AI 에이전트와 자동화 표지" loading="lazy" />
        </a>
        <div class="chloe-book__meta">커뮤니케이션북스 · 2025</div>
        <p class="chloe-book__desc">AI 에이전트 시대의 워크플로 설계와 자동화 개념을 처음부터 정리한 입문서.</p>
      </article>

      <article class="chloe-book">
        <a class="chloe-book__cover chloe-book__cover--rose"
           href="https://product.kyobobook.co.kr/detail/S000001944968"
           target="_blank" rel="noopener"
           aria-label="인공지능, 말을 걸다 · 교보문고에서 보기">
          <img class="chloe-book__image" src="/assets/images/books/talking-ai.jpg" alt="인공지능, 말을 걸다 표지" loading="lazy" />
        </a>
        <div class="chloe-book__meta">스리체어스 · 2020</div>
        <p class="chloe-book__desc">대화형 AI와 인간의 관계를 인문학적 관점으로 풀어낸 에세이.</p>
      </article>
    </div>
  </section>

  <section class="chloe-section chloe-section--muted">
    <header class="chloe-section__head">
      <span class="chloe-eyebrow">Activity</span>
      <h2 class="chloe-section__title">지금 하고 있는 것들</h2>
    </header>

    <div class="chloe-project-grid">
      <a class="chloe-project" href="/blog/">
        <div class="chloe-project__icon">✍️</div>
        <h3 class="chloe-project__title">This Blog</h3>
        <p class="chloe-project__desc">AI 자동화 실험과 솔로프러너 워크플로의 기록.</p>
        <div class="chloe-project__tags"><span>Writing</span></div>
      </a>
      <a class="chloe-project" href="https://www.youtube.com/@chloe.systems" target="_blank" rel="noopener">
        <div class="chloe-project__icon">🎥</div>
        <h3 class="chloe-project__title">Chloe의 자동화연구소</h3>
        <p class="chloe-project__desc">유튜브 — 같은 주제를 영상으로 풀어봅니다.</p>
        <div class="chloe-project__tags"><span>YouTube</span></div>
      </a>
      <a class="chloe-project" href="https://x.com/chloe_systems" target="_blank" rel="noopener">
        <div class="chloe-project__icon">🧵</div>
        <h3 class="chloe-project__title">@chloe_systems</h3>
        <p class="chloe-project__desc">X에서 공유하는 짧은 실험 기록.</p>
        <div class="chloe-project__tags"><span>Social</span></div>
      </a>
    </div>
  </section>

</div>
