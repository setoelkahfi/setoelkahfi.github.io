---
layout: post
title: "What Is Buffer Overflow?"
date: 2019-01-04 18:24:37 +0100
comments: true
categories: Computer security 
---

Buffer overflows result from accepting input to a program that does not fit into receiving memory buffers, and yet still continuing to write to that memory. Clearly the main issue here is that input is not being properly checked, while the principle of complete mediation requires that all accesses to objecta (in this case memory buffers) be checked to ensure that they are allowed. It would be a mistake to miss the relevance for this principle.

Another relevant principle here is that of least privilege. The most serious kind of buffer overflow are those that allow the user to insert code that is subsequently executed. In these situations, foreign code is executed with the same privileges as those that the running program has. For this reason, it is am unnecessary risk to allow the program to run with higher privileges than are strictly necessary for the program to complete the task.

Creating a buffer overflow vulnerability is a programming error. One could therefore propose that if the program code was simple then errors such as this would be less likely. In this case, one would argue for the principle of economy of mechanism.
