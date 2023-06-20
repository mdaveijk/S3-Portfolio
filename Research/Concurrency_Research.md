# How can concurrency be effectively implemented in a distributed software web application to improve performance and scalability?


## Table of contents
- [1. Introduction](#1-introduction)
- [2. What is concurrency?](#2-what-is-concurrency)


## 1. Introduction

I became interested in the topic of concurrency after reading chapters about concurrency in the books Clean Code and The Pragmatic Programmer. Both of these books will be used as professional sources of literature for this research. As concurrency has always been a challenging subject for me to implement effectively, I decided to dedicate this research to gaining a better understanding of it. As a result, I have a working microservices architecture for "CineMatch": my distributed web application.

Concurrency is widely recognized as a complex and challenging aspect of software development, even among experts in the field. They agree that there is no 'perfect' way to handle concurrency, but there are best practices that we can follow. We also often hear that concurrency does not always *guarantee* performance improvement. If not used with the appropriate resources or implemented correctly, concurrency can instead create complexity and potentially hinder performance. So, this begs the question: **How can concurrency be effectively implemented in a distributed software web application to improve performance and scalability?**

This research consists of two parts: a theoretical part and a practical part. I will start by looking at literature written by experts and explore design patterns related to concurrency. After that, I will conduct performance tests on my own application and use a static program analyser to find potential bugs and other issues that may impact concurrency.

In order to help me and find an answer to the main question, I will be using these DOT-framework methods:
- Literature study (Library)
- Design pattern research (Library)
- Non-functional test (Lab)
- Static program analysis (Showroom)

## 2. What is concurrency?

Let's start with the basics: What is concurrency? To understand concurrency, we have to understand parallelism. This quote explains it in the most clear way:

> "_Concurrency_ is when the execution of two or more pieces of code act as if they run at the same time. _Parallelism_ is when they _do_ run at the same time. 
> To have concurrency, you need to run code in an environment that can switch execution between different parts of your code when it is running. This is often implemented using things such as fibers, threads, and processes.
> To have parallelism, you need hardware that can do two things at once. This might be multiple cores in a CPU, multiple CPUs in a computer, or multiple computers connected together."

(Thomas & Hunt, 2019, p. 169)



Let's look at some examples of concurrency and parallelism to get a better understanding.


## 7. References

Thomas, D., & Hunt, A. (2019). _The Pragmatic Programmer: your journey to mastery, 20th Anniversary Edition_. Addison-Wesley Professional.