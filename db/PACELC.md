# PACELC

**[PACELC theorem](https://en.wikipedia.org/wiki/PACELC_theorem)** is an extension to the CAP theorem.

**CAP theorem** basically says that between consistency, availability, and partition tolerance, you can only choose two i.e. in the presence of a network partition, you must choose consistency or availability.

**PACELC** states that, in the event of a network partition, one has to choose between availability (A) and consistency (C), but else (E), even when the system is running in the absence of partitions, one has to choose between latency (L) and consistency (C).

A high availability requirement implies that the system must replicate data. As soon as a distributed system replicates data, a tradeoff between consistency and latency arises.

## Background

> *But my main problem with CAP is that it focuses everyone on a consistency/availability tradeoff, resulting in a perception that the reason why NoSQL systems give up consistency is to get availability.* [Daniel Abadi's 2010 Blog Post](https://dbmsmusings.blogspot.com/2010/04/problems-with-cap-and-yahoos-little.html)

* [Consistency Tradeoffs in Modern Distributed Database System Design](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)