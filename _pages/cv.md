---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
* *M.Sc. in Artificial Intelligence*, University of Edinburgh, 2025
  * Thesis: *Tetricks: An einsum tensor compiler for Coarse Grain Reconfigurable Arrays*
* *B.Tech Honors in Computer Science and Engineering*, Indian Institute of Information Technology Kottayam, 2022
  * Thesis: *Quantum machine learning for cryptanalysis of simple ciphers*
* *Professional Certificate in Quantum Computing & Quantum Internet*, Delft University (EdX), 2020
* *Deep Learning Specialization*, Deeplearning.ai (Coursera), 2020

Work Experience
======
* *2020 - Present: Open Source efforts*
  * JAX, Qiskit, Pennylane, QRAND, PyZX

* *2025 - Present: Staff AI Engineer @ Synopsys*
  * Building LLM-assisted Electronic Design Automation (EDA) tools for physical design in VLSI, as part of the Copilot R&D team in the Technology Production Group.

* *2022 - 2024: Graphics PD Engineer @ Intel India Technologies*
  * Led multi-voltage verification for physical design of graphics IPs in the Visual and Machine Learning IP group.

* *Summer 2021: Summer Research Fellow @ Indian Academy of Sciences*
  * Adapting deep learning computer vision techniques to ultrasound domain.

* *Sep - Oct 2021: Data Science Intern @ Husqvarna AI Labs*
  * Developed real-time data visualization dashboards using PySpark SQL and Hive for global sales teams.

* *Summer 2020: Machine Learning Intern @ AAIWAY India*
  * Built a Flask-based cloud backend for employee face recognition authentication with OpenVINO inference optimization on edge devices.

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

{% comment %}
Talks
======
  <ul>{% for post in site.talks reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>

Teaching
======
  <ul>{% for post in site.teaching reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
{% endcomment %}
