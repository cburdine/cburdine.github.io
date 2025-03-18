---
permalink: /
title: "Colin Burdine's Homepage"
excerpt: "About me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

Hi, I'm Colin, a PhD graduate student at Baylor University in the department of electrical and computer engineering. I come from a computer science and mathematics background, but my academic research cuts across many disciplines, ranging from nano-scale computing technologies to machine learning and quantum information.

Research
======

Artificial intelligence and quantum computing are transforming scientific discovery, offering unprecedented capabilities for solving complex problems in the physical sciences. My research focuses on integrating quantum computing techniques and machine learning models to advance the simulation of quantum many-body systems, electronic structure calculations, and the predictive modeling of solid-state materials, nanoscale devices, and open quantum systems. By developing novel computational frameworks, I aim to accelerate the discovery of quantum materials and engineerable electronic systems, bridging the gap between theoretical predictions and experimental realization.

I am currently working in [Dr. E. P. Blair's](http://web.ecs.baylor.edu/faculty/blair/) Lab in the Electrical and Computer Engineering departmentat Baylor University. I have had previous summer appointments at Sandia National Lab (2020) and Argonne National Lab (2021) through the [U.S. Department of Energy SULI program](https://science.osti.gov/wdts/suli).

I have also contributed to the [SAGE](https://sagecontinuum.org/) project, which seeks to develop infrastructure for AI/ML applications on edge devices. In particular, I have contributed to the [Waggle Sensor Project](https://github.com/waggle-sensor) project, which is an open source platform for developing intelligent sensor networks.

Materials + ML Course
------

![Materials + ML](images/matml_logo.svg)

I am in the process of developing an open-source introductory workshop course in Machine Learning and its applications in Materials Science. I led the first iteration of this workshop at Baylor University in Summer 2023, and plan to continue revising and expanding the online content for the course. All course materials including lecture slides, recordings, Python code, etc. are available [here](https://cburdine.github.io/materials-ml-workshop) for free.

About this Website
------
This site serves as a space to document my projects, both academic and beyond. I also plan to share occasional blog posts exploring topics related to my research and adjacent fields.

If you would like to contact me, you can reach me via my email: <a id="email"></a>

<script>
  document.addEventListener("DOMContentLoaded", () => {
      const email = "colin_burdine1" + "@" + "baylor" + ".edu";
      const emailElem = document.getElementById("email");
      if (emailElem) {
          emailElem.textContent = email;
          emailElem.href = "mailto:" + email;
      }
  });
</script>

