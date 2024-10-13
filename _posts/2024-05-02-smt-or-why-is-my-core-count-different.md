---
layout: default
title:  SMT, or Why is My Core Count Different?
date:   2024-10-13 17:16:03 +1000
categories: OS
---


<!-- ## Introduction -->

A commonly quoted term in CPU capabilities is the number of cores or threads available. This initially makes sense to provide, because in the world of multiprocessing and multithreading, it (mostly) governs how many tasks can be run truely parallel. These numbers often appear to have multiple values, however. Common jargon might say that one CPU can have 4 cores and 8 threads.

Why?

## CPU Package vs CPU Cores

The top layer here is the **CPU**. This normally refers to the entire chip you put into a computer and is the only element of these sold individually at stores. You're probably familiar with this already, as it's what most media refer to as a CPU and fits everything on it: CPU Cores, Cache memory, sometimes a mini GPU engine, and all the data buses and other parts needed to make our modern PCs work.

An example of this is the 2017 AMD Ryzen CPUs, which used the AMD Zen 1 architecture. In this case, let's look at a Ryzen 7 1700.

![Image]({{ site.baseurl }}/assets/blog_images/J4taYqR3QeLrwyGDRK9xoNdi.png)

Thanks to the power of modern imaging tools, we can also look at exactly what's inside this chip, using what's known as a Die-Shot.

![Image]({{ site.baseurl }}/assets/blog_images/RZ6Fdcc1KuvwXEh6dybKbiJC.png)

The die-shot above lets us see the second layer down, and is essentially the central piece of the CPU, zoomed in and with the top removed. The CPU Cores are the main computational units of the computer, and modern CPU chips tend to have multiple (8, in the above).

Due to the age of Kernel-level systems, it's not uncommon for the individual CPU Cores to be called just "CPUs" in some OS documentation, because, well, originally CPU chips only had one CPU Core on them. As computers can (almost always) only mount one CPU chip this naming makes an "assumption" there without explaining it, unfortunately. This is probably the first bit that's getting you confused.

If you're interested in looking at die shots more, this is an individual CPU core from the above image. You may recognise certain components here now - in particular, we'll talk about the Arithmetic Logic Unit and Floating Point Unit later. (Note: It is upside-down compared to the above shot)

![Image]({{ site.baseurl }}/assets/blog_images/Z73AtdD9siB7BBNiZc55wAP0.png)

## Simultaneous MultiThreading (SMT, or HyperThreading)

Now the third layer of this is "how do we have 4 cores and 8 CPU threads". Historically, there was a 1:1 relationship here - each core could only run one CPU thread. Where things get slow is when that certain tasks - particularly ones with intense floating point calculations or when filling the cache from memory - take multiple clock cycles to complete and underutilise the CPU's resources. Cache loading can take hundreds of cycles, for instance.

In an attempt to improve performance without just adding more cores, Intel (as HyperThreading) or AMD (as SMT) have made a split between "physical" and "logical" processors. What this does is change the 1:1 relationship of core:thread to a 1:2 relationship, so that each physical CPU core is supporting two Logical CPU Cores/Threads at once. AMD have produced a nice overview of how these kernel structures are shared (or not) below, and how the hardware manages them.

![Image]({{ site.baseurl }}/assets/blog_images/G2mzAS2Oz7YUUa4rE0rZVdfv.png)

Now take for instance we have two different processes running. One has a lot of Integer operations taking place on the ALU, and the other has many Floating-point operations taking place on the FPU. If only one was running at a time, then either the FPU or ALU would be tragically underutilised, wasting valuable clock cycles. With SMT, we can run these two processes simultaneously and get much more utilisation. Essentially, the two logical cores/threads make the single physical core getting more work done, if not quite as much as two separate non-SMT physical cores. This does, of course, depend on the two hyperthreads using different parts of the CPU.

Schedulers are also able to leverage this though, and will do smart things as part of their load-balancing to try to maximise this by keeping all the cores busy, or minimise power consumption by placing more tasks onto less cores. More modern CPU cores, such as Intel's Alder Lake 'P-core' CPUs, can be much larger and even have two sets of registers to support this fully.

![Image]({{ site.baseurl }}/assets/blog_images/1WZhOirR2Tm58f5oBwriUBJK.png)

## Read more

Thanks for reading! If you're still interested in learning more about SMT, then the [Wikipedia page](https://en.wikipedia.org/wiki/Simultaneous_multithreading) is a strong start and the two papers by Tullsen referenced by it go into more technical depth:

- [D. M. Tullsen, S. J. Eggers and H. M. Levy, "Simultaneous multithreading: Maximizing on-chip parallelism," Proceedings 22nd Annual International Symposium on Computer Architecture, Santa Margherita Ligure, Italy, 1995, pp. 392-403.](https://ieeexplore.ieee.org/document/524578)
- [H. M. Levy, Jack L. Lo, J. S. Emer, R. L. Stamm, S. J. Eggers and D. M. Tullsen, "Exploiting Choice: Instruction Fetch and Issue on an Implementable Simultaneous Multithreading Processor," 23rd Annual International Symposium on Computer Architecture (ISCA'96), Philadelphia, PA, USA, 1996, pp. 191-191, doi: 10.1145/232973.232993.](https://ieeexplore.ieee.org/document/1563047)

*This first blog is based off a forum response I provided to an interested student while working for Operating Systems (COMP3231) @ UNSW. My thanks to my friends who suggested I share knowledge like this more broadly.*

*I hope to do a short blog in the future on CUDA, and multithreading. Hope to see you there!*
