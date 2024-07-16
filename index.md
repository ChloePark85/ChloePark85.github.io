---
layout: splash
permalink: /
hidden: true
header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/header-image.jpg
  actions:
    - label: "Learn More"
      url: "/about/"
excerpt: >
  Welcome to my personal website where I showcase my projects and share my thoughts on web development and design.
feature_row:
  - image_path: /assets/images/project-1.jpg
    alt: "Project 1"
    title: "Project 1"
    excerpt: "Description of Project 1"
    url: "/projects/project-1/"
    btn_class: "btn--primary"
    btn_label: "Learn more"
  - image_path: /assets/images/project-2.jpg
    alt: "Project 2"
    title: "Project 2"
    excerpt: "Description of Project 2"
    url: "/projects/project-2/"
    btn_class: "btn--primary"
    btn_label: "Learn more"
  - image_path: /assets/images/project-3.jpg
    alt: "Project 3"
    title: "Project 3"
    excerpt: "Description of Project 3"
    url: "/projects/project-3/"
    btn_class: "btn--primary"
    btn_label: "Learn more"
---

{% include feature_row %}

## Latest Blog Posts

<div class="grid__wrapper">
  {% for post in site.posts limit:4 %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>