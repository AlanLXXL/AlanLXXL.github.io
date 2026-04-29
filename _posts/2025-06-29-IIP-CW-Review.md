---
title: "Why I Struggled with the Introduction to Image Processing Coursework: A Reflection on Unexpectedly Low Marks"
date: 2025-06-29
last_modified_at: 2025-06-29T23:59:00+08:00
categories:
  - Blog
tags:
  - Computer Science
  - Image Processing
  - Coursework Review
  - Reflection
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

# Why I Struggled with the Introduction to Image Processing Coursework: A Reflection on Unexpectedly Low Marks

The purpose of this reflection is to delve into the unexpected challenges I faced during the Introduction to Image Processing coursework, ultimately leading to lower marks than anticipated. This document will explore the specific areas where I encountered difficulties, analyse the contributing factors to these struggles, and outline the key lessons learned from this experience. By examining my performance, I aim to gain a deeper understanding of the complexities inherent in image processing and identify strategies for future academic success in related fields.

[**✅ What Went Well	2**](#✅-what-went-well)

[**❌ What Went Wrong — And Why	2**](#❌-what-went-wrong-—-and-why)

[1\. Too Much Focus on Tuning, Too Little on Methodology	2](#1.-too-much-focus-on-tuning,-too-little-on-methodology)

[2\. Shallow Understanding of Key Concepts	2](#2.-shallow-understanding-of-key-concepts)

[**🧠 Lessons Learned	3**](#🧠-lessons-learned)

[🐢 1\. Slow is Fast	3](#🐢-1.-slow-is-fast)

[🧘 2\. Less is More	3](#🧘-2.-less-is-more)

[⏱️ 3\. Shift the Time Balance	3](#⏱️-3.-shift-the-time-balance)

[**💭 Final Thoughts	3**](#💭-final-thoughts)

When I first started the *Introduction to Image Processing* coursework, I had high hopes. I was confident in my technical ability, comfortable with code, and genuinely interested in computer vision. I expected to perform well — or at least decently. But when the final grade came in, it fell below my expectations. After carefully reviewing the instructor's feedback and reflecting on my process, I now understand what went wrong.

### **✅ What Went Well** {#✅-what-went-well}

Before diving into the mistakes, I want to acknowledge the parts I did right — because understanding *what works* is as important as understanding *what doesn’t*.

According to the feedback:

* I provided a **clear literature review**, especially around using K-Means for segmentation.

* My **pipeline was well-structured and visualized** with appropriate diagrams.

* I included a **quantitative evaluation** of the final method.

* I reflected on the limitations of my solution, like how the pipeline might fail when the flower is near the image corners.

These were encouraging signs. But good structure and results alone aren’t enough in a scientific context.

---

### **❌ What Went Wrong — And Why** {#❌-what-went-wrong-—-and-why}

#### **1\. Too Much Focus on Tuning, Too Little on Methodology** {#1.-too-much-focus-on-tuning,-too-little-on-methodology}

During development, I spent most of my time adjusting parameters to improve the final result or making the model more complex. I thought that enhancing performance directly would lead to better marks. But this mindset was flawed.

What I neglected was the **scientific process** — breaking down the model and evaluating each part separately. The final evaluation only showed the performance of the complete pipeline. It lacked **ablation studies**, which would’ve helped isolate the impact of components like Otsu thresholding and K-means individually. Without that, my results weren’t fully convincing.

This was a big lesson: performance alone is not enough — clarity, transparency, and systematic evaluation matter just as much, if not more.

#### **2\. Shallow Understanding of Key Concepts** {#2.-shallow-understanding-of-key-concepts}

Another weakness was my lack of deep understanding of certain concepts — particularly the idea of "unknown regions" in segmentation. The feedback pointed out that my explanation of these regions was vague and potentially flawed.

To be honest, I didn’t fully understand the **waterflow or subtraction-based segmentation logic** myself. I applied what seemed to work, guided by online tools and AI assistance (like ChatGPT), but I didn’t spend enough time reading core documentation or understanding the foundational principles.

This was a shortcut that cost me — and it showed in both the explanation and the results.

---

### **🧠 Lessons Learned** {#🧠-lessons-learned}

#### **🐢 1\. *Slow is Fast*** {#🐢-1.-slow-is-fast}

I now realize that spending more time understanding concepts deeply — even if it feels slow at first — actually speeds up progress in the long run. Rushing to improve the model without understanding the "why" behind each step led me in circles.

#### **🧘 2\. *Less is More*** {#🧘-2.-less-is-more}

Instead of making the pipeline more complicated, I should have focused on making each component crystal clear. One simple, well-justified model with solid experimental analysis would have been far more convincing than a black-box result with good performance.

#### **⏱️ 3\. *Shift the Time Balance*** {#⏱️-3.-shift-the-time-balance}

In future projects, I want to follow a new ratio:

**80% Research & Understanding \+ 20% Implementation**

This shift will help ensure I’m grounded in the theory, confident in my design choices, and clearer in my explanations — all of which lead to stronger, more honest work.

---

### **💭 Final Thoughts** {#💭-final-thoughts}

This coursework didn’t go as I planned — but it taught me more than I expected. It reminded me that scientific work isn’t about rushing to results. It’s about being rigorous, transparent, and intentional in every step of the process.

Failure is painful, but reflection turns it into growth. And this is one of those moments I won’t forget.