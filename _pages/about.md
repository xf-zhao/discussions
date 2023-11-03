---
layout: about
title: About
permalink: /
# subtitle: I'm actively looking for collaboration/visiting oppotunities in **LLMs+Robotics**
profile:
  align: right
  image: xufeng.jpg
  image_circular: true # crops the image to make it circular
  address: >

news: true  # includes a list of news items
latest_posts: true  # includes a list of the newest posts
selected_papers: false # includes a list of papers marked as "selected={true}"
social: true  # includes social icons at the bottom of the page
---

<script src="https://unpkg.com/typeit@8.7.1/dist/index.umd.js"></script>

<p id="companionMethods"></p>


- 🕘 3rd year Ph.D student in [University of Hamburg](https://www.inf.uni-hamburg.de/en/inst/ab/wtm.html), 2021-now
- 👔 AI Engineer [JD.COM](jd.com), 2018-2020.
- 🎓 M.Sc in Signal Processing, [University of Chinese Academy of Sciences](https://english.ucas.ac.cn/), 2015-2018.
- 👔 Signal Processing Intern @[Extantfuture startup](https://extantfuture.com/), 2014-2015.
- 🎓 B.E. in Electronic Information Engineering, [Xidian University](https://www.xidian.edu.cn), 2010-2014.


<script>
new TypeIt("#companionMethods", {
  speed: 50,
  waitUntilVisible: true,
})
  .type("👋 Hi, ", { delay: 1800 })
  .type("I'm", { delay: 300 })
  .move(-1)
  .delete(1)
  .type(" a")
  .move(null, { to: "END" })
  .type(" activly")
  .pause(300)
  .move(-2)
  .type("e")
  .move(null, { to: "END" })
  .type(" seeking for collaborations in ")
  .pause(300)
  .type("<b> LLMs + Robotics. </b>")

  .go();
</script>