---
theme: seriph
addons:
  - "@twitwi/slidev-addon-ultracharger"
addonsConfig:
  ultracharger:
    inlineSvg:
      markersWorkaround: false
    disable:
      - metaFooter
      - tocFooter
NObackground: >-
  https://images.unsplash.com/photo-1511149755252-35875b273fd6?ixlib=rb-4.0.3&dl=leon-contreras-qpdfU6vehgs-unsplash.jpg&w=1920&q=80&fm=jpg&crop=entropy&cs=tinysrgb
background: /logo/ship2.jpg
highlighter: shiki
routerMode: hash
lineNumbers: false

css: unocss
title: Machine Learning
subtitle: Model Selection & Regularization
date: 30/09/2024
venue: HSE
author: Alexey Boldyrev
---

<br>
<br>
<br>
<br>
<br>

# <span style="font-size:28.0pt" v-html="$slidev.configs.title?.replaceAll(' ', '<br/>')"></span>
# <span style="font-size:24.0pt" v-html="$slidev.configs.subtitle?.replaceAll(' ', '<br/>')"></span>
# <span style="font-size:18.0pt" v-html="$slidev.configs.author?.replaceAll(' ', '<br/>')"></span>

<span style="font-size:18.0pt" v-html="$slidev.configs.date?.replaceAll(' ', '<br/>')"></span>



<div class="abs-tl mx-5 my-10">
  <img src="/logo/FCS_logo_full_L.svg" class="h-18">
</div>

<div class="abs-tl mx-5 my-30">
  <img src="/logo/DSBA_logo.png" class="h-28">
</div>

<div class="abs-tr mx-5 my-5">
  <img src="/logo/ICEF_logo.png" class="h-28">
</div>

<style>
  :deep(footer) { padding-bottom: 3em !important; }
</style>

<!--
NB: This demo uses a custom syntax (using preparser extensions), with all the @@@@.
-->


---
src: ./slides/0_attendance.md
---

---
src: ./slides/0_outline.md
---

---
src: ./slides/1_pitfalls_of_lm.md
---

---
src: ./slides/2_subset_selection.md
---

---
src: ./slides/3_regularization_methods.md
---

---
src: ./slides/4_dimensionality_reduction.md
---

---
src: ./slides/5_bonus.md
---

---
src: ./slides/0_end.md
---