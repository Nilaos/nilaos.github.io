---
layout: default
title:  SMT, or Why is My Core Count Different?
date:   2023-09-09 23:23:03 +1000
categories: jekyll update
---

## Introduction

A commonly quoted term in CPU capabilities is the number of cores or threads available. This initially makes sense to provide, because in the world of multiprocessing and multithreading, it (mostly) governs how many tasks can be run truely parallel (TODO: Insert reference to CUDA blog).

These numbers often appear to have multiple values, however. Let's look at a recent AMD CPU, for instance.

<!-- Insert graphic of recent-ish Ryzen CPU with some details -->

This is a Ryzen X CPU made by AMD, and released in 20XX. It uses their XXXXX architecture and fits an AM4 motherboard socket.
Commonly, a CPU will be advertised with the number of CPU cores (and their speed) proudly displayed in the centre of the copy.
This is no exception: It's advertised as having

<!-- Dig into where the cores are on the die-shot. Cite! -->
This lines up very nicely with the details we can get from this nice annotated die-shot.

However, when we look at the actual CPU in task manager, the values displayed can be somewhat different and this ties into another element of the advertising which is common amongst modern x86-64 (AMD64) processors. This is an extra feature called Hyperthreading by Intel, or Simultaneous Multithreading (SMT) by AMD.

## Hyperthreading & SMT

Hyperthreading
<!-- TODO: Write more -->
