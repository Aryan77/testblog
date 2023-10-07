---
layout: default
---

# Project portfolio

## Distributed Database

![](/images/distdb.png)

[Github](https://github.com/Aryan77/distributed-database)

A database system built from scratch which has the following features: 

1. Distributed across multiple servers - can store a lot of data and so that if one fails the others can handle the load (thus persistent and fault-tolerant).
2. Reliable and correct - Consensus between the nodes of the distributed system has been implemented correctly using the Raft protocol - so we can be sure that despite many concurrent connections changing data at the same time there is no unexpected behavior.
3. Extremely performant - implements sharding using a shard controller, this architecture was inspired by Google's BigTable which implements sharding to make their enormous database very fast.

## Minimal Browser

![](/images/mapreduce.png)

[Github](https://github.com/Aryan77/minimal-browser)

A browser engine built from scratch in a single file of Python; I built my own HTML/CSS parser and layout engine to render everything manually to a `tkinter` window. It can: 

1. Connect to a server and download web pages.
2. Render this data as text onto a graphical window.
3. Parse the HTML into a tree layout and recognize and handle the different attributes on the different elements; recursively navigate this tree and render the page layout correctly.
4. Correctly read linked `CSS` files and the inline `style` attribute in HTML tags, and support cascading and inheritance. Browser settings themselves can be set in a global `CSS` file: `browser.css`.
5. Provide a UI similar to most modern browsers today - including multiple tabs, an address bar to navigate to specific URLs and a back button to go back in history. 

## Ten-Tac-Toe

![](/images/tentactoe.png)

[Github](https://github.com/ten-tac-toe) - [Live](https://ten-tac-toe-kzaqvz5yyq-ue.a.run.app)

Ten-Tac-Toe, more commonly known as [Ultimate Tic Tac Toe](https://en.wikipedia.org/wiki/Ultimate_tic-tac-toe), is a deeply strategic and significantly more complicated extension of regular Tic-Tac-Toe. This web app allows players to play this game against a very strong AI of varying difficulties, built using the minimax algorithm with alpha-beta pruning. It also allows players to play against other players online. 

## OCaml Data Science Library

![](/images/ocaml.png)

[Github](https://github.com/Aryan77/ocaml-data-science-ml)

A data science + machine learning library for OCaml that replicates and combines the most commonly used functionality of three famous Python libraries - Numpy, Pandas, Sklearn. Integrates well with the Archimedes library for graphing, and can thus be used for every step of a regular machine learning workflow.

## MapReduce

![](/images/mapreduce.png)

[Github](https://github.com/Aryan77/MapReduce)

This project was an attempt to better understand Google's MapReduce pattern, by implementing it from scratch in C++. The code essentially takes as input the path to a directory full of text files, counts and sorts the Ngrams (contiguous sequence of n words - n is also a constant provided by the user) in those texts by frequency, and reports back the most popular Ngrams found by each of the worker threads.

The fact that the worker threads maintain their own local data structures and only update the global data structure once they have all the data provides a huge advantage: there is very little need for locking any large data structures - making code both significantly easier to write and less prone to errors and slow-downs. It is clear why MapReduce has been so successfully used in big data processing.

## CoursePlan

![](/images/courseplan.png)

[Github](https://github.com/cornell-dti/course-plan) - [Live](https://courseplan.io/)

CoursePlan is a four-year academic planner for Cornell undergraduates developed by the Design & Tech Initiative. CoursePlan helps undergraduates track their courses and their requirements automatically depending on their college, major, and minor. It aims to allow students view the big picture of their time at Cornell.

I primarily worked on a feature to add templates to CoursePlan - so students will be able to see a recommended 4-year structure of the classes they will need to take in order to complete their major. 