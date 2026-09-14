---
layout: post
author: Philipp Schröppel
title: "How I run and organize experiments"
date: 2026-09-10
description: "How I run and organize experiments."
---

This post describes how I run and organize (deep learning) experiments. I have been asked multiple times about this, so maybe it is worth sharing.

The following first describes how I run experiments and then how I organize them. 

## Running experiments with lmbrun
{% comment %}
For running experiments, I use a tool called [lmbrun](https://github.com/pschroeppel/lmbrun). I've developed the tool throughout my PhD and at the core it combines multiple experimenting tools from members of our computer vision group.
{% endcomment %}
For running experiments, I use a tool called `lmbrun` (will be published soon). I've developed the tool throughout my PhD and at the core it combines multiple experimenting tools from members of our computer vision group.

The typical workflow is to create a config file that specifies metadata, compute requirements, and commands. An experiment can then simply be started by executing `lmbrun config.yaml`. `lmbrun` then can:

- start experiments via Slurm, Torque, SSH, or locally on the current machine,
- snapshot the current codebase in form of a git tag (helpful for reproducibility),
- give the experiment an ID and create an output folder in a structured format,
- re-use the git tag to run the code snapshot (which allows to continue working on the codebase without affecting the experiment),
- deal with clusters with a walltime via job-chains.

Once the experiment is running, `lmbrun` brings additional tools to check the status of the experiment, stop it, or start follow-up jobs. Based on the git tag that `lmbrun` created, it is easy to compare what changed between the current codebase and any previous experiment.

The only requirement for `lmbrun` is that the experiment code must live in a git repository. Personally I have used it mostly with `python`, but in principle `lmbrun` can execute any commands.

For more details, see the following video:

<figure class="video">
  <video controls preload="metadata" playsinline width="1920" height="1080"
         poster="{{ '/images/lmbrun-demo-poster.jpg' | relative_url }}">
    <source src="{{ '/data/lmbrun-demo.mp4' | relative_url }}" type="video/mp4">
    <p>Your browser cannot play this video.
       <a href="{{ '/data/lmbrun-demo.mp4' | relative_url }}">Download it instead.</a></p>
  </video>
</figure>

## Organizing experiments

My experiment organization builds on: giving each experiment a description, automatic logging, and keeping a journal.

When starting an experiment, I give it a short informal free-form description (I found this easier than trying to log specific attributes in a structured way, as those attributes keep constantly changing). In the actual code, I set up Weights & Biases such that it logs the experiment using its ID (which comes from `lmbrun`) and its free-form description:
![Weights & Biases run list: each run named by its lmbrun experiment ID, with the free-form description in the Notes column]({{ '/data/wandb_example.png' | relative_url }}){: .narrow}

{% comment %}
I try to log all important information, such that ideally I have everything I need directly available in the logs and do not have to manually execute commands post-hoc. For an example of how to set up `lmbrun` with Weights & Biases logging, see [lmbrun-mnist-demo](https://github.com/pschroeppel/lmbrun-mnist-demo).
{% endcomment %}
I try to log all important information, such that ideally I have everything I need directly available in the logs and do not have to manually execute commands post-hoc. 

The second part of organizing experiments is manual journaling: I document what I wanted to answer, what I tried, which experiments make sense to compare, and what the findings were. For this, I use my research journal in Obsidian (each journal entry is a markdown file), or the really nice "Reports" feature in Weights & Biases:
- [a typical Weights & Biases report](https://wandb.ai/pschroeppel/reproduce-adaslot/reports/29-10-2025-Comparing-AdaSlot-Implementations--VmlldzoxNDg3Nzg4NQ?accessToken=ki988qtunegffy91wpwonkq48rzz3d6ljnik7wc3e4u0p22czgg99p77hnn5bfpt)
- [a typical journal entry in Obsidian]({{ '/data/journal-entry-example.pdf' | relative_url }}) (exported as .pdf)

What I found helpful for journaling:

- set up the journal *before* running the experiments and use it to document the research question and plan the experiment (a bit like test-driven software development, where you also write the tests before the actual code),
- when looking at results, *always* have the journal available and write down what you see (otherwise you will later anyway look at the same results again).

Also, I find journaling helpful to keep a bigger picture overview of what I am doing.

## Summary

That's it, nothing special. What I think is indeed quite nice:

- using the git tags to create snapshots for experiments,
- having a tool that just runs the experiment with a single command, and provides simple tools to check job statuses and kill or start additional jobs,
- using free-form descriptions for experiments,
- writing journal entries extensively and already before running the experiments.
