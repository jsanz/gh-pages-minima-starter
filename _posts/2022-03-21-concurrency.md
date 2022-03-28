---
title: "Concurrency"
date: 2022-03-21
layout: post
---
  
For the past few weeks, I've been focusing on my coursework module of CTEC2910 Concurrent and Parallel Algorithms. Absolute madness.
Despite many efforts of searching online for learning materials because the uni-provided materials leave a lot to be desired, and the vague and unhelpful responses from lecturers including the module leader, 
we finally managed to make sense of it all. The day before it was due in. It took 4 people working together when this was set as an individual coursework subject. I should note that we worked together to
understand what we needed to do, but we still did the working ourselves as to not plagiarise or cheat.  

We were reading up on semaphores, non-blocking algorithms, parallel threading and multitasking. The problem we were given consists of the following questions:

```
A cake shop provides a stream of cakes for customers. Cakes are made by a chef in the kitchen  
and placed on a conveyor belt. The conveyor belt carries the cakes to where the customers are  
waiting to buy them.  
This scenario has been simulated using two processes: the chef and the customer, and a shared  
conveyor belt implemented as an array called conveyor with infinite size, where each space in the  
array can hold one cake. In this scenario, there are multiple chefs.  
The pseudo-code for the chef is as follows.  
1. while(true){  
2. cake = makeCake(); // Create a cake  
3. wait(empty);  
4. conveyor[in] = cake; // Put the cake on the conveyor belt  
5. in = (in + 1) mod n;  
6. }  
```

We had to find the fault in the code, and the solution to that was that it was waiting for an infinite buffer to empty, so we removed the wait(empty); call, 
and then signal two semaphores, one that the producer Mutex was now complete and the other that we were ready for the next buffer item.  

The finished code was the following:  

```
Int in = 0;  
while(true)  
{  
    cake = makeCake();  
    wait(producerMutex);  
    conveyor[in] = cake;  
    in++;  
    signal(producerMutex);  
    signal(ready);  
}  
```

It took several weeks to understand what they wanted us to do, as the module leaders and lecturers were no help, the materials they provided didn't cover this multiple-producer/single-consumer scenario, 
and there were little helpful materials on the internet. One of my friends were eventually able to make sense of it through one of the lecture slides that explained the semaphore nonsense. On the day before hand in, I was finally ready to Turnitin (sorry, not sorry).


Lately, I've found the standard of materials provided by the lecturers and module leaders, in addition to the standard of teaching provided to be quite sub-par.
On the whole, I've had a positive experience but I've found this module and another (Agile Team Development) to be severely lacking in the revision and learning materials.
Some modules, like Object-Oriented Design/Development have been very easy to understand and complete the work, but this and Agile are very frustrating and I will be letting the module team know my thoughts and feelings about all of this.