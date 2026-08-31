---
title: "Blog"
permalink: /blog/
layout: splash
author_profile: false
excerpt: "AI 에이전트와 자동화 실험 기록. n8n·Claude·TTS·멀티 에이전트 시스템으로 1인 기업이 AI 팀과 협업하는 방법을 탐구합니다."
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
---

<div class="chloe-home">

  <section class="chloe-hero chloe-hero--compact chloe-hero--center">
    <div class="chloe-hero__inner chloe-hero__inner--single">
      <span class="chloe-eyebrow">Blog</span>
      <h1 class="chloe-hero__title">
        AI 자동화 <span class="chloe-accent">실험 노트</span>
      </h1>
      <p class="chloe-hero__lead">
        n8n · Claude · TTS · 멀티 에이전트 실험의 결과와 배운 점을 정리합니다.<br/>
        1인 기업이 AI 팀과 협업하는 방법에 관한 기록.
      </p>
    </div>
  </section>

  <section class="chloe-section">
    {% if site.posts.size > 0 %}
      <header class="chloe-section__head">
        <span class="chloe-eyebrow">Latest</span>
        <h2 class="chloe-section__title">최근 글</h2>
      </header>

      <div class="chloe-post-list">
        {% for post in site.posts %}
          <a class="chloe-post" href="{{ post.url | relative_url }}">
            <div class="chloe-post__meta">
              <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y.%m.%d" }}</time>
              {% if post.categories.size > 0 %}
                <span class="chloe-post__cat">{{ post.categories | first }}</span>
              {% endif %}
            </div>
            <h3 class="chloe-post__title">{{ post.title }}</h3>
            {% if post.excerpt %}
              <p class="chloe-post__excerpt">{{ post.excerpt | strip_html | truncate: 140 }}</p>
            {% endif %}
            <span class="chloe-post__more">읽기 <span aria-hidden="true">→</span></span>
          </a>
        {% endfor %}
      </div>
    {% else %}
      <div class="chloe-empty">
        <div class="chloe-empty__icon">✍️</div>
        <h2 class="chloe-empty__title">첫 글을 준비 중입니다</h2>
        <p class="chloe-empty__desc">
          곧 실험 기록을 하나씩 풀어갈 예정입니다.<br/>
          그 사이엔 <a href="https://www.youtube.com/@chloe.systems" target="_blank" rel="noopener">유튜브</a>나
          <a href="https://x.com/chloe_systems" target="_blank" rel="noopener">X</a>에서 짧은 기록을 볼 수 있어요.
        </p>
        <div class="chloe-empty__actions">
          <a class="chloe-btn chloe-btn--primary" href="/projects/">Projects 보기 <span aria-hidden="true">→</span></a>
          <a class="chloe-btn chloe-btn--ghost" href="/about/">About Me</a>
        </div>
      </div>
    {% endif %}
  </section>

</div>
