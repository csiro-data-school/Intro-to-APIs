---
title: "What is an API?"
teaching: 15
exercises: 5
questions:
- "What is an API?"
- "Why do we need APIs?"
- "How APIs work?"
objectives:
- "Understand the need for an API."
- "Understand the main components of an API."
- "Expand Library API knowledge to Web API use."
keypoints:
- "An API is a way for two or more computer programs or components to communicate with each other, enabling efficient data exchange and functionality sharing."
- "APIs allow for selective data retrieval from large, dynamic datasets, eliminating the need to download entire datasets for accessing small segments of data, thereby saving time and computational resources."
- " Both library APIs and web APIs share a fundamental principle of abstracting complexity, allowing developers to leverage pre-defined functions or data over the internet"
---

Imagine you regularly work with a large, frequently updated online dataset. How do you access the latest data each time you need it? What if you work with only a small segment of the data?

In such a case, accessing up-to-date information would traditionally mean downloading the entire dataset each time—a process that is far from efficient.

Additionally, let's say your task involves mapping longitude and latitude values from the dataset on to a global map, which is a different size each time. Manually scaling these values to accurately place them on a map not only invites redundancy but is also prone to error. Performing these calculations repeatedly for common, globally recognized locations is an unnecessary drain on resources and time.

This is where APIs come into play. APIs are designed to eliminate such redundancies and boost efficiency. They allow you to request and receive only the data you need, even if it's just a small portion of a larger dataset. For tasks like mapping, APIs can also facilitate automatic handling of the complex calculations and data rendering, presenting you with ready-to-use results.

## What is an API
An Application Programming Interface (API) defines how different software components should interact, allowing them to communicate and share data and functionalities efficiently.

APIs provide incredible amounts of structured data, as well as the ability to control things that may previously have required specialist proprietary software or even hardware. In particular, the data available via web APIs is particularly useful for data scientists; many data are now only made available via these APIs, and even in cases where data are made available in other formats, using an API is frequently more convenient.

## How APIs work

![Illustration of APIs as a Barista](../assets/img/HowAPIWorks.png)

## Understanding APIs Through Python Libraries: An Analogy
You might already be familiar with the concept of APIs from using Python libraries. A library is a set of reusable code that developers can integrate into their programs to add functionality without starting from scratch. An API is the method or protocol by which these pieces of code interact with each other. 

Think of an API as the logical representation of the library. It provides a consistent format that explains what a developer can do with the library and how to do it. It is essentially the “visible” part of the code that developers use. While the library refers to the code itself, the API refers to the interface that makes it accessible and usable.

For instance, suppose you need to calculate the square root of a number. Instead of writing the code to implement the square root algorithm, you can use the sqrt() function from the math library. The math library contains reusable code for various mathematical operations. The API of the math library includes the sqrt() function, which defines how you interact with the underlying code to compute the square root. This approach saves time and simplifies your code. APIs abstract away complexity, allowing you to focus on higher-level problem-solving.

## Web APIs
While Python library APIs and web APIs both provide interfaces for accessing and using code or services, their key differences lie in how they are accessed, the environments they operate in, and how data is exchanged. 

- Python Library APIs are local to your development environment. You import and use them directly in your codebase, such as import math or import statistics.
- Web APIs  are hosted remotely and accessed over the internet. They involve sending HTTP requests to endpoints and receiving responses, often in JSON format.

Although library APIs in Python, such as the math library, differ from web APIs, the principle remains the same.

- Both are designed to facilitate interaction between different pieces of code or systems.
- Both provide an interface that abstracts complex functionality, allowing users to perform tasks without needing to understand or write the underlying code. 

