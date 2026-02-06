---
zoom: 0.85
---

# Current Homework Assignments

<script setup>
const timelineSource = `
@ [2026-02-03T12:00~2026-02-11T18:00] #green {Kaggle Competition} 👂Phonemes
* [2026-02-05T23:59] #yellow {Kaggle Competition} Interim deadline (Soft)
* [2026-02-06T18:00] #red {Kaggle Competition} Interim deadline (Hard)
* [2026-02-10T23:59] #yellow {Kaggle Competition} Final deadline (Soft)
* [2026-02-11T18:00] #red {Kaggle Competition} Final deadline (Hard)

@ [2026-01-27~2026-02-18T18:00] #red {🤖STACK+Maxima Tutorial} 🤖STACK+Maxima Tutorial
* [2026-02-17T23:59] #yellow {🤖STACK+Maxima Tutorial} Soft deadline
* [2026-02-18T18:00] #red {🤖STACK+Maxima Tutorial} Hard deadline

@ [2026-01-27~2026-02-18T18:00] #blue {🤖OLS Estimator} 🤖OLS Estimator
* [2026-02-17T23:59] #yellow {🤖OLS Estimator} Soft deadline
* [2026-02-18T18:00] #red {🤖OLS Estimator} Hard deadline
`
</script>

<ChronosTimeline :source="timelineSource" />

---
layout: end
hideInToc: true
---