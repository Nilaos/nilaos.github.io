---
layout: default
title:  "SMT: Or, Why is My Core Count Different?"
date:   2023-09-09 23:23:03 +1000
categories: jekyll update
---

# SMT: Or, Why is My Core Count Different?

## Introduction

A commonly quoted term in CPU capabilities is the number of cores or threads available. This initially makes sense to provide, because in the world of multiprocessing and multithreading, it (mostly) governs how many tasks can be run truely parallel (TODO: Insert reference to CUDA blog).

These numbers often appear to have multiple values, however. Let's look at something somewhat recent, for instance. 

<!-- Insert graphic of recent-ish Ryzen CPU with some details -->

Commonly, a CPU will be advertised with the number of CPU cores (and their speed) proudly displayed in the centre of the copy.

<!-- Dig into where the cores are on the die-shot. Cite! -->
This lines up very nicely with the details we can get from this nice annotated die-shot.

However, when we look at the actual CPU in task manager, the values displayed can be somewhat different.
