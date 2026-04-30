---
title: "Year 2 Computer Science at UNNC: What I Wish I Knew Sooner"
date: 2025-06-09
last_modified_at: 2025-06-09T23:59:00+08:00
categories:
  - Blog
tags:
  - Computer Science
  - UNNC
  - Learning Strategy
excerpt: "An honest, personal guide to Year 2 Computer Science at UNNC — the resources that actually helped, and what each core module really felt like."
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

# Overview

Year 2 in Computer Science at UNNC was a rollercoaster of concepts, bugs, deadlines, and unexpected "Aha\!" moments. This page is a personal reflection and learning log from someone who made it through, with both some late-night panic and genuine curiosity.

This isn’t a textbook or an official guide. It’s the kind of advice I wish I had before starting the year: honest thoughts on each module, how I approached them, what resources actually helped, and what didn’t.

Whether you’re a fellow UNNC student, a curious first-year, or someone just trying to find your rhythm in CS, I hope this helps you feel a little less lost—and maybe even excited.

Let’s debug this journey together ☺️

# Good Resources

## YouTube Channels

[https://www.youtube.com/@Computerphile](https://www.youtube.com/@Computerphile)

![][image1]

Computerphile is a legendary YouTube channel that makes complex computer science topics accessible, visual, and surprisingly fun. What’s extra cool is that many of the presenters—like Dr. Alex Turner and Prof. Mike Pound—are actually lecturers and researchers from the **University of Nottingham**. So, watching this feels like having a sneak peek (or a head start) into what’s taught at UNNC and UoN.

If you're struggling with understanding concepts like **hashing, algorithms, floating-point numbers, neural networks**, or even **encryption**, Computerphile probably has a video on it. Their explanations are visual, well-paced, and grounded in real academic knowledge.

[https://www.youtube.com/@3blue1brown](https://www.youtube.com/@3blue1brown)  
![][image2]

3Blue1Brown is a YouTube channel created by Grant Sanderson that uses **beautiful animations** to explain complex math concepts. If you've ever found linear algebra, calculus, or neural networks hard to grasp, this channel will completely change the way you see them.

The **“Essence of Linear Algebra”** series is especially useful for UNNC CS students—it makes abstract topics like vector spaces, matrix transformations, and eigenvalues feel intuitive and visual. These concepts are foundational in computer science, especially in areas like graphics, AI, and machine learning.

[https://www.youtube.com/watch?v=tpIctyqH29Q&t=2s](https://www.youtube.com/watch?v=tpIctyqH29Q&t=2s)

![][image3]

This series is a fantastic **big-picture overview** of computer science—especially helpful for **second-year CS students at UNNC**. Each video is short (usually under 12 minutes), fast-paced, and packed with animations that make complex topics easy to follow.

It walks you through the entire **evolution of computing**, from hardware fundamentals like **bits, logic gates, and transistors** to high-level concepts like **programming languages, operating systems, and networking**. Even though it doesn't go deep into technical details, it builds a **solid conceptual framework** that connects everything you're learning.

## Online Courses

[https://pll.harvard.edu/course/cs50-introduction-computer-science](https://pll.harvard.edu/course/cs50-introduction-computer-science)

![][image4]

🎓 CS50: Introduction to Computer Science (Harvard University)

If you’re just starting out in computer science, **CS50** is *hands down* one of the best courses in the world to begin with. It’s designed by Harvard for **non-CS majors**, but it ends up teaching many of the **fundamental CS concepts** that even advanced students can benefit from.

Taught by **Professor David J. Malan**, this course is well-structured, engaging, and full of hands-on practice. You’ll start by understanding what algorithms are, then learn **C programming**, move on to **memory management**, **data structures**, **algorithms**, **web development**, **SQL databases**, and even **AI basics**.

**💡 My Tip:**  
 Take it on the [**edX platform**](https://cs50.harvard.edu/x) where it’s totally **free**. Why? Because it allows you to **submit code**, **complete problem sets**, and **track your progress**—which is way more effective than just watching the lectures passively.

If you’re serious about CS, this course will **lay a solid foundation** and help you understand the "magic words" of computer science step by step.

[https://www.coursera.org/specializations/machine-learning-introduction](https://www.coursera.org/specializations/machine-learning-introduction)

![][image5]

This is one of the most **widely recommended and high-quality ML courses** on the internet—taught by **Andrew Ng**, co-founder of DeepLearning.ai and a leading figure in AI education (also a former professor at Stanford University). If you're interested in machine learning but don’t know where to start, this is the course for you.

**📌 Bonus Tip:**  
 After completing this specialisation, you can continue with Andrew Ng’s **Deep Learning Specialisation** to go deeper into:

* Neural Networks and Deep Learning

## Github

[https://github.com/He1mont/CS-Course/tree/main](https://github.com/He1mont/CS-Course/tree/main)

![][image6]

This is one of the most **comprehensive and practical GitHub repositories** for CS students at UNNC (University of Nottingham Ningbo China). It was created and maintained by a student who collected their learning materials from Fall 2021 to Spring 2025—including **lectures, labs, tutorials, textbooks, notes**, and most importantly, **past exam papers**.

# Module Introduction

## Module: Programming and Algorithms (C Language)

This is likely the **first module** in Year 2 that feels like a real **challenge**. In Year 1, most students cover general programming and problem-solving basics, but this module dives deep into the **core foundations of computer science**, using **C** as the primary language.

If you’ve never worked with a lower-level language before, be prepared: **pointers**, **memory management**, and **string manipulation** are waiting for you. But don’t worry—with patience and the right mindset, it’s very rewarding.

📚 What You’ll Learn:

* ✅ **Pointers**: One of the most essential and confusing topics. Understanding how to work with memory addresses and dereferencing is key to later success in systems programming, OS, and more.

* ✅ **Arrays, Strings, and Structures**: Learn how to store and organize data manually.

* ✅ **Functions and Modularization**: Build your own functions and structure your program across multiple files.

* ✅ **File I/O**: Read and write to files in C, which helps prepare for real-world input/output systems.

* ✅ **Advanced Topics**: You’ll touch on performance, external libraries, and even "OOP-style" programming in C.

---

💻 Coursework Info:

This module has **two courseworks**, both of which may look hard at first—especially if you’re not confident in programming. However:

* The **specifications are detailed** and guide you step-by-step.

* As long as you **read the spec carefully** and break the problem down, you can complete them even as a beginner.

* **Automated testing** is used for grading, so your code must:

  * Work under the **Linux environment (CSLinux)**

  * Handle **edge cases** and produce clean output

  * Compile and run without errors or warnings

## Module: Databases and Interfaces (COMP1048 UNNC)

This module is offered in the **autumn semester of Year 2**, and it’s one of the most **practical and beginner-friendly** introductions to working with **databases and web interfaces**. It helps bridge the gap between data storage and web development.

---

📚 What You’ll Learn:

* 🧱 **Database Design Basics**: You’ll start with concepts like **relational algebra** and **entity-relationship (ER) modelling**, which are essential for building a **clean, well-structured database**.

* 🛠️ **SQL Programming**: Learn and practice key commands like `CREATE`, `INSERT`, `SELECT`, and `DROP`. SQL is a **simple but powerful** language that’s widely used in real-world development.

* 🧪 **Database Normalisation**: Understand how to optimise data structure by reducing redundancy and improving consistency.

* 🌐 **Web Integration**: You’ll use **HTML, CSS**, and **Python with Flask** to build and connect a web interface to your database.

---

💻 Coursework Breakdown:

There are two coursework:

* **Coursework 1**: Includes lab quizzes and focuses more on **SQL fundamentals** and your understanding of database structure.

* **Coursework 2**: A full-stack mini project where you:

  * Build a personal or project webpage using **HTML \+ CSS**

  * Develop a back-end using **Python and Flask**

  * Connect it to a real **SQL database**

This gives you hands-on experience with **database-driven web apps**, a skill highly useful in internships, jobs, or personal projects.

---

✅ Why It’s Useful:

* A great introduction for students interested in **backend development**, **data engineering**, or **web applications**.

* You’ll leave the module with a working web app connected to a real database.

* Builds a solid foundation for future modules like **Web Development**, **Software Engineering**, or **Data Science**.

## Module: Computer Fundamentals (COMP1036 UNNC)

This is the **third core module** in the autumn semester of Year 2 and a **must-study** for anyone serious about computer science. Unlike more applied modules, this one takes a **theoretical and structural approach**, guiding you through the **layers of abstraction** that define modern computing systems.

It’s built on the well-known MIT course structure, which gives it a strong foundation and clear progression.

---

📚 What You’ll Learn:

* 🧠 **Boolean Logic & Arithmetic**: Start with logic gates and binary operations—the “math” behind the machine.

* 🧮 **CPU Internals**: Understand how arithmetic logic units (ALUs), memory, and registers interact inside a processor.

* 🏗️ **Sequential Logic & Simulated Hardware**: Learn how computers *really* process instructions using custom-built simulations.

* ⚙️ **Machine Language & Assembly**: Dive into the lowest layer of programming, writing dozens of instructions just to do one thing—but in doing so, **truly grasp how code talks to hardware**.

* 🖥️ **Virtual Machines**: Explore high-level simulations that bridge hardware with modern software systems, and understand their role in modern compilers.

---

🔍 Why It’s Challenging but Worth It:

Many students find this module **difficult**, especially the **assembly language** part. It can be frustrating to write so many lines just to perform basic logic—but that's exactly why it’s powerful.

You'll begin to understand:

* What actually happens behind a `for` loop or a function call

* How **memory addressing** and **pointers** truly work

* Why different layers of abstraction exist—and how they stack from hardware to software

---

💬 My Perspective:

Some people say learning assembly is outdated. But I’d argue the opposite: it’s **not about using assembly in your job**, it’s about gaining the mindset of a computer scientist. This module teaches you to think at every level—**from logic gates to virtual machines**. And that’s a skill that sticks with you, no matter what area of CS you specialise in later.

## Module: Mathematics for Computer Scientists (COMP1046 UNNC)

This is the **final core module in the autumn semester of Year 2**, and also the **only dedicated mathematics module across your 3-year CS degree**. It’s challenging—but it’s also absolutely essential if you want to understand the theory behind computation, algorithms, and even AI.

📚 What You’ll Learn:

🧠 Part 1: Discrete Mathematics & Logic

* **Propositional & Predicate Logic**: The basics of formal reasoning—critical for logic, programming languages, and theoretical CS.

* **Inference Rules & Proof Techniques**: Learn mathematical proofs like **induction**, which is used in formal verification and language theory.

* **Sets, Functions, and Relations**: These are fundamental to almost everything in CS—like databases, programming, and automata.

* **Counting & Probability**: Core tools for **machine learning**, **data science**, and **algorithm analysis**.

These topics may feel abstract, but they are *everywhere*—whether you’re proving a program is correct, calculating time complexity, or modelling uncertainty in data.

📊 Part 2: Linear Algebra

* **Matrices, Vectors, and Systems of Equations**

* **Vector Spaces, Rank, Basis & Dimension**

* **Linear and Geometric Mappings**

* **Optimisation with Calculus**

This section gives you the **mathematical tools to represent and manipulate data**—a must-have for AI, graphics, simulations, and fields like **image processing** (which you might take in Year 3 as the IP module).

You’ll begin to see that **linear algebra is a language** for expressing rules, structures, and transformations—whether it’s a 3D rotation, a neural network, or a graphics engine.

---

---

🧩 Why It Matters (Even If It’s Hard)

Many students underestimate this module because it doesn’t “look” like programming. But computer science is built on **abstractions**, and this module shows you how deep those abstractions go—from logic gates and sets, to vectors and optimisation.

If you want to do well in topics like **AI**, **formal verification**, **compilers**, or **machine learning**, this module gives you the base you’ll need to understand *why* things work—not just *how*.

---

## Final Thoughts

Year 2 is the year computer science stops feeling like “learning to code” and starts feeling like learning to *think* like a computer scientist. C teaches you what your machine is actually doing under the hood. Databases & Interfaces shows you how to ship something real, end-to-end. Computer Fundamentals takes you down to the logic gates and back up. And Maths for CS gives you the language to reason about all of it.

It’s a lot at once — and honestly, it can feel relentless. There were weeks where I felt behind on every module simultaneously. But looking back, the moments that shaped me the most weren’t the lectures or the slides; they were the late nights spent debugging a pointer issue, the small wins of getting an SQL query to finally return the right rows, and the “oh, *that’s* what assembly is doing” realisations that suddenly made everything click.

A few things I’d tell my past self:

* **Start coursework early.** Year 2 deadlines stack on top of each other in a way Year 1 never warned you about. A weekend's head start saves you a week of panic.
* **Don’t skip the maths.** It feels disconnected when you’re studying it, but every later module — AI, graphics, ML, even compilers — quietly leans on it.
* **Use AI tools as study partners, not shortcuts.** Ask them to *explain*, not just generate. The understanding is the part that compounds.
* **Talk to your seniors.** The He1mont/CS-Course repo, the past papers, the casual advice from upper years — these are some of the most useful resources you’ll find at UNNC, and they’re all free.

If you’re heading into Year 2, I hope this guide saves you a few of the mistakes I made. And if you’re already in it and feeling stuck — that’s normal. You’re not behind. You’re just in the middle of the hardest year so far. Keep going.

Let’s debug this journey together ☺️

[image1]: /assets/images/year2-guide-image1.png
[image2]: /assets/images/year2-guide-image2.png
[image3]: /assets/images/year2-guide-image3.png
[image4]: /assets/images/year2-guide-image4.png
[image5]: /assets/images/year2-guide-image5.png
[image6]: /assets/images/year2-guide-image6.png