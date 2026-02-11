---
# You can also start simply with 'default'
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

background: /logo/ship2.jpg

# some information about your slides (markdown enabled)
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
# transition: slide-down
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true

title: Machine Learning 1
hideInToc: true
subtitle: Model Selection & Regularization
date: 10/02/2026
venue: HSE
author: Alexey Boldyrev
---

<br>
<br>
<br>

# <span style="font-size:32.0pt" v-html="$slidev.configs.title?.replaceAll(' ', '<br/>')"></span>
# <span style="font-size:32.0pt" v-html="$slidev.configs.subtitle?.replaceAll(' ', '<br/>')"></span>
# <span style="font-size:18.0pt" v-html="$slidev.configs.author?.replaceAll(' ', '<br/>')"></span>
<span style="font-size:18.0pt" v-html="$slidev.configs.date?.replaceAll(' ', '<br/>')"></span>
<div>

<span style="color:#b3b3b3ff; font-size: 11px; line-height: 1.5em; float: right;">Image credit: ‘The Mayﬂower at Sea’<br> by Granville Perkins, 1876<br>
Wallach Division Picture Collection<br> The New York Public Library.
</span>
</div>

<div class="abs-tl mx-5 my-10">
  <img src="/logo/FCS_logo_full_L.svg" class="h-18">
</div>

<div class="abs-tr mx-5 my-5">
  <img src="/logo/DSBA_logo.png" class="h-28">
</div>

<style>
  :deep(footer) { padding-bottom: 3em !important; }
</style>


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