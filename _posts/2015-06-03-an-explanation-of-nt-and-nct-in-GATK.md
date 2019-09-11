---
published: true
layout: post
title: "An Explanation of nt and nct in GATK"
author: Yu
categories: Bioinformatics
tags:
- GATK
- threads
- CPU
---

I am confused with GATK Multi-threading options, `-nt / --num_threads` controls the number of data threads sent to the processor, `-nct / --num_cpu_threads_per_data_thread` controls the number of CPU threads allocated to each data thread.

What's data threads?

[Here is an answer from Geraldine Van der Auwera, PhD](http://gatkforums.broadinstitute.org/discussion/1849/question-and-suggestion-re-nct-num-threads-options)

> In the meantime, what you need to know is that -nct is the number of CPU threads, ie threads that can be run by different cores if you have a multicore CPU, while -nt is the number of data threads, ie number of "clones" of the GATK that are run in parallel on your machine.

So `nt` is based on how many copies you want to run in the same time.

`nct` can control the CPU threads. Take CB60-G16 Network Server for example, 


|Tested Configuration: | |
|:-----:|:-----:|
|Computer Type:  |Blade Module|
|Mother Board Revision:  |C01|
|BIOS/uEFI:  |uEFI: 1.61 (09/06/2013)|
|CPU: | 2 Intel  Xeon® Processor E5-2690 v2 3.0 GHz|
|RAM:  |32 GB|


Each node has two CPUs (10 cores, 20 threads).

If I use `-nct 10`, it means I use 10 threads on one node. `-nt 10` means I use 10 threads for one data thread(one sample). `nt` might cause an increasing in memory usage, each data thread would be allowed with the same memory size.
