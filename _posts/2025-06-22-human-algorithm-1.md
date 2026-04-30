---
title: "You Can’t Compare Apples to Bananas: Why Life Isn’t a Fair Benchmark"
date: 2025-06-22
last_modified_at: 2025-06-22T23:39:00+08:00
categories:
  - Blog
tags:
  - Computer Science
  - Human Algorithm
excerpt: "Why comparing kids — or anyone — without controlling for environment is not just emotionally unfair, but logically invalid. A reflection through the lens of algorithm benchmarking."
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
toc: true
toc_sticky: true
toc_label: "Contents"
---


# Introduction:

In computer science, especially in algorithm analysis, comparing two methods or systems fairly requires strict controls, like fixing the random seed, controlling external variables, or using statistical methods like the Mann–Whitney U test. Yet, in real life, we often ignore all of that. Parents, with the best of intentions, compare their children’s progress, personalities, and achievements as if they’re running the same program under the same conditions. But we’re not.

# Section 1: Why Comparison Feels Unfair

I’ve often found myself stuck in a loop of comparison, not by choice, but because of how my parents measure my worth. They want me to be excellent, not just for myself, but in the way they see other children being excellent. Children of their friends. Kids they hear about on the internet. Classmates whose successes seem to shine brighter, no matter how far I’ve come.

To them, it seems simple: "**If they can do it, why can’t you?**"  
But to me, that question feels like a wound.

They don’t see the full picture — the quiet differences in our family environments, the emotional climate I grew up in, or the unique way our brain works. They don’t see the nights I’ve stayed up thinking, the mornings I’ve pushed through exhaustion, or the countless small victories that go unnoticed simply because they don’t look like someone else’s definition of success.

It hurts — deeply — to be treated like a model that should function exactly like another, as if we were built on the same architecture, trained on the same data, or raised under the same conditions. In truth, we’re all running different programs in different environments. Comparing us directly ignores every unique variable that shapes who we are.

And yet, when you're constantly told you're not enough — not fast enough, smart enough, successful enough — something inside begins to break. You start questioning your own progress. You wonder if you’re defective, like a broken algorithm, even though you're working harder than anyone can see.

There are moments when I feel so tired, not because I haven’t tried, but because my efforts are being measured with a scale that was never meant for me. That’s what makes it painful. Not failure, but being misunderstood while doing your best.

And that’s when I started to think — maybe the reason this hurts so much is because it’s not just emotionally unfair, it’s logically flawed too. As someone who studies computer science, I can’t help but notice how careless these comparisons are from a systems point of view. In computing, we’d never compare two algorithms running in totally different environments without accounting for variables. It would be considered invalid — even misleading. Yet in life, these flawed comparisons happen all the time. So maybe it’s time to look at this through a different lens — the lens of fairness, logic, and systems thinking.

![][image1]

# Section 2: Computer Science Analogy

However, in computer science, it's crucial to be rigorous and systematic, just like in scientific experiments.

We need to compare two algorithms step by step.

## 1. Defining the Problem and Objectives

A problem is something that you want to solve.   
For example:

* Classifying emails as spam or not spam.

And the objectives are something that we want to achieve.  
For example:

* **Objective:** Maximize the number of correctly identified spam emails while minimizing false positives (legitimate emails marked as spam).

## 2. Choosing Performance Metrics

Then we need to choose the metrics to compare algorithms  
Here are some common metrics we use in computer science.

1. Accuracy:

Misleading Accuracy Example:  
Imagine a dataset where 99% of emails are not spam. An algorithm that always predicts “not spam” will be 99% accurate — but it's useless. This shows how **accuracy alone** can be misleading.

In life, people often judge based on one flawed metric: grades, salary, university ranking, or job title. But just like in algorithm evaluation, no single number captures the whole picture.

2. Precision:

![][image2]

![][image3]

3, Recall  
![][image4]

**Example 2: Medical Diagnosis (Detecting a Disease)**

* **Problem:** A machine learning model detects a serious, highly contagious disease.

* **Actual Situation:** In a population, 100 people *actually have* the disease.

* **Model's Performance:**

  * The model correctly identifies 95 of the 100 infected individuals (True Positives).

  * It *misses* 5 infected individuals, classifying them as healthy (False Negatives).

  * **Calculation:** Recall \= 95 / (95 \+ 5) \= 95 / 100 \= 0.95 or 95%

# Section 3: Establishing Strict Controls and Experimental Design

**Same Dataset:** Both algorithms *must* be tested on the exact same dataset. This dataset should be representative of the real-world data the algorithm will encounter.

* Wilcoxon Signed-Rank Test  
  * This method is used to test for the same dataset

* Mann-Whitney U Test  
  * This method could be used to test for different datasets

The core is whether to reject the null hypothesis or not

## 🌱 Life Analogy: Why This Matters

In life, people often compare students, siblings, or friends without “controlling the data.”  
 Different family backgrounds, different emotional environments, different support systems — yet we act as if it’s a fair test.

Without shared conditions, comparisons are not just **emotionally unfair**, they’re **logically invalid**.

# Conclusion: Life Deserves a Fairer Benchmark

In this blog, we began by looking at a painful but familiar experience — the way people, especially parents, often compare their children without considering the deeper context. These comparisons may feel personal, but they’re also structurally flawed. They assume a shared environment, shared challenges, and shared resources — when in fact, every individual is running on a different “system,” trained on different “data.”

To highlight how misleading this is, we turned to computer science, where comparisons are handled with care. We explored core evaluation metrics like accuracy, precision, and recall — each of which offers only part of the story. We saw that even a high accuracy score can be deceptive without understanding the bigger picture.

Finally, we looked at how researchers use statistical tests like the Wilcoxon Signed-Rank Test and the Mann–Whitney U Test to ensure their conclusions are valid. These methods reinforce a powerful idea: you don’t get to claim something is better just because it looks that way — you have to test it fairly, under controlled conditions.

So the next time someone says, “Why can’t you be more like them?” — remember that this question ignores everything a good algorithm designer would demand: clearly defined goals, fair metrics, and controlled environments. You are not a flawed version of someone else. You are a unique system running in a unique context, and that deserves recognition, not comparison. 

[image1]: /assets/images/human-algorithm-1-image1.png
[image2]: /assets/images/human-algorithm-1-image2.png
[image3]: /assets/images/human-algorithm-1-image3.png
[image4]: /assets/images/human-algorithm-1-image4.png