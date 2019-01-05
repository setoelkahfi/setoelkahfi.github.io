---
layout: post
title: "What Is Buffer Overflow?"
date: 2019-01-04 18:24:37 +0100
comments: true
categories: Computer security 
---
One of many topics in computer security that always comes up is buffer overflows. What is <b>buffer overflow</b> exactly? And how we can relate this topic with principles for the design of security from Saltzer and Schroeder?

<h2>Buffer Overflow and Complete Mediation Principle</h2>
Buffer overflows result from accepting input to a program that does not fit into receiving memory buffers, and yet still continuing to write to that memory. Clearly, the main issue here is that input is not being properly checked.

<!-- more -->

The principle of <b>complete mediatior</b>, from Saltzer and Schroeder's principles of security systems, requires that all accesses to objects (in this case memory buffers) be checked to ensure that they are allowed. It would be a mistake to miss the relevance for this principle. 

<h2>Buffer Overflow and Least Privilege Principle</h2>

Another relevant principle here is that of <b>least privilege</b>. The most serious kind of buffer overflow are those that allow the user to insert code that is subsequently executed. In these situations, foreign code is executed with the same privileges as those that the running program has. For this reason, it is an unnecessary risk to allow the program to run with higher privileges than are strictly necessary for the program to complete its task.

<h2>Buffer overflow and Least Common Mechanism</h2>
For similar reasons one can make a case for observing the principle of least common mechanism. Once a buffer overflow attack is executed the resources of the machine that is under attack are vulnerable. The less resource that are shared, the less the system is vulnerable.

<h2>Buffer Overflow and Economy of Mechanism</h2>
Creating a buffer overflow vulnerability is a programming error. One could therefore propose that if the program code was simple then errors such as this would be less likely. In this case, one would argue for the principle of <b>economy of mechanism</b>.


<h2>What is not related</h2>
We can see no case for the principle of open design. The principle states that the security of a mechanism should not depend on the secrecy of its design or implementation. The buffer overflow vulnerability is not a case of failed dependency on secrecy. Some confuse the principle of open design with that of open source, and suggest that if others ("many eyes") could read the source code then the problem would be discovered. Though we have in general discussed the role of openness in
security, it is a stretch to bake all such implications into the principle of open design.

We also have difficulties making convincing arguments for the relevance of fail-safe defaults, separation of privilege, and psychological acceptability in mitigating buffer overflow vulnerabilities.
