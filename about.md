---
layout: inner
title: About
seo_title: "About Basit Rahim: AI Researcher, ML Engineer and LUMS Graduate"
description: "About Basit Rahim, an AI Researcher at DARE-RC (LUMS) working on machine learning for education policy in Pakistan. Previously at ShipIn, Turing and SSI; BS Computer Science from LUMS with Distinction."
permalink: /about/
---

<div class="about-page">

  <header class="about-intro">
    <img src="{{ site.baseurl }}/img/portrait.jpg" alt="Basit Rahim" class="about-photo" width="160" height="160">
    <div>
      <p class="hero-kicker"><span class="pulse-dot" aria-hidden="true"></span> AI Researcher @ DARE-RC</p>
      <h1>About me</h1>
      <p class="about-lead">I'm <strong>Basit Rahim</strong>, a researcher and engineer who uses machine learning to answer real questions about people, starting with why children in Pakistan leave school.</p>
    </div>
  </header>

  <section class="about-story">
    <p>I studied Computer Science at the <strong>Lahore University of Management Sciences (LUMS)</strong> and graduated with Distinction, making the Dean's Honor List three years in a row.</p>
    <p>Today I'm an <strong>AI Researcher at DARE-RC</strong>. I design agent-based and machine learning models of student dropout, build the dashboards that put those results in front of government stakeholders, and work with CITY @ LUMS on GIS-based spatial analysis.</p>
    <p>Before research I spent time in industry, which is why I care as much about <em>shipping</em> as about modelling. I built Rust backends for edge AI at ShipIn, trained and evaluated LLMs with RLHF at Turing, and built test infrastructure for factory line-control systems at SSI.</p>
  </section>

  <section>
    <h2 class="about-h2">What I do</h2>
    <div class="focus-grid">
      <div class="focus-card">
        <i class="fa fa-line-chart"></i>
        <h3>Applied ML research</h3>
        <p>Survey and administrative data, leakage audits, grouped CV, SHAP and significance testing.</p>
      </div>
      <div class="focus-card">
        <i class="fa fa-comments"></i>
        <h3>LLM engineering</h3>
        <p>RLHF data, model evaluation and LLM-powered developer tools.</p>
      </div>
      <div class="focus-card">
        <i class="fa fa-server"></i>
        <h3>Research software</h3>
        <p>FastAPI, PostgreSQL, React, Rust and Docker systems that real users rely on.</p>
      </div>
    </div>
  </section>

  <section>
    <h2 class="about-h2">Experience</h2>
    <ol class="about-timeline">
      <li class="is-current">
        <span class="when">Aug 2025 &ndash; Present</span>
        <h3>AI Researcher <span>&middot; DARE-RC</span></h3>
        <p>Agent-based and ML models of student dropout, GIS analysis with CITY @ LUMS, psychometric dashboards and large-scale survey data.</p>
      </li>
      <li>
        <span class="when">Jul 2024 &ndash; Aug 2025</span>
        <h3>AI Engineer <span>&middot; ShipIn</span></h3>
        <p>Scalable Rust backends for an edge-AI platform, real-time data pipelines and database optimisation.</p>
      </li>
      <li>
        <span class="when">Aug 2024 &ndash; May 2025</span>
        <h3>Python &amp; Data Engineer <span>&middot; Turing</span></h3>
        <p>Trained LLMs for coding and data science with RLHF, evaluated model solutions and analysed failure cases.</p>
      </li>
      <li>
        <span class="when">Jul 2023 &ndash; Aug 2024</span>
        <h3>Software Engineer <span>&middot; Strategic Systems International</span></h3>
        <p>Test infrastructure for ASMPT line-control systems, RabbitMQ simulation frameworks and REST APIs.</p>
      </li>
    </ol>
  </section>

  <section>
    <h2 class="about-h2">Education</h2>
    <div class="about-edu">
      <i class="fa fa-university"></i>
      <div>
        <h3>BS Computer Science, LUMS</h3>
        <p>Aug 2019 &ndash; June 2023 &middot; Graduated with Distinction &middot; Dean's Honor List 2019&ndash;20, 2020&ndash;21, 2021&ndash;22</p>
      </div>
    </div>
  </section>

  <section>
    <h2 class="about-h2">Selected work</h2>
    <div class="about-projects">
      {% assign case_studies = site.projects | sort: 'order' %}
      {% for p in case_studies %}
      <a href="{{ site.baseurl }}{{ p.url }}">
        <span class="mini-domain">{{ p.domain }}</span>
        <strong>{{ p.short_title | default: p.title }}</strong>
        <i class="fa fa-arrow-right"></i>
      </a>
      {% endfor %}
    </div>
  </section>

  <section class="about-contact">
    <h2>Let's talk</h2>
    <p>Open to research collaborations, ML engineering roles and interesting data problems.</p>
    <div class="hero-cta">
      <a class="btn-hero btn-hero-primary" href="mailto:{{ site.email }}"><i class="fa fa-envelope"></i> {{ site.email }}</a>
      <a class="btn-hero btn-hero-ghost" href="{{ site.baseurl }}/Basit_Resume.pdf" target="_blank" rel="noopener"><i class="fa fa-download"></i> Download CV</a>
    </div>
    <ul class="hero-social">
      <li><a href="https://github.com/{{ site.github_username }}" target="_blank" rel="noopener" aria-label="GitHub"><i class="fa fa-github"></i></a></li>
      <li><a href="{{ site.linkedin_url }}" target="_blank" rel="noopener" aria-label="LinkedIn"><i class="fa fa-linkedin"></i></a></li>
      <li><a href="https://twitter.com/{{ site.twitter_username }}" target="_blank" rel="noopener" aria-label="Twitter"><i class="fa fa-twitter"></i></a></li>
    </ul>
  </section>

</div>
