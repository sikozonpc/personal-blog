---
title: "Problem Solving Techniques"
date: 2019-05-22T20:43:04+01:00
draft: false
description: "How to solve and use logical thinking in software related problems."
tags: [computer-science, javascript, algorithms]
---

In my humble opinion I think **problem solving is the single most desired skill in a developer or an engineer** and odds are if you came from a self-taught path like me, algorithms and complex data structures might be **daunting**.

At the start I couldn't even solve easy problems on Hackerrank, even though I knew how to make well rounded applications, and that made me frustrated and anxious.

So, at the time, I decided to learn the basics of computer science and also took a course on algorithms and data structures with JavaScript from [Colt Steele](https://www.udemy.com/js-algorithms-and-data-structures-masterclass/#instructor-1) which I highly recommend taking if you relate to me.

## 1.) Understanding the problem

Either you're given a problem to solve within x amount of time on an interview or you're solving Hackerrank challenges for fun or even to practice for interviews, **you will need to read carefully and draft a plan** to solve the exercise.

This is the most important step of all, if you don't draft a plan and start coding like a headless chicken you might get lost in the problem and panic.

> **Step 1: Understand the problem in hand, read it well and use your own words to describe it.**

Let's say the given problem is (the typical FizzBuzz):

-   "Write a program that prints the numbers from 1 to 100. But for multiples of three print “Fizz” instead of the number and for the multiples of five print “Buzz”. For numbers which are multiples of both three and five print “FizzBuzz”."

So we want something like this: _to print "Fizz" for the multiples of 3, for the multiples of 5 "Buzz" and for both "FizzBuzz", otherwise print the current number._

## 2.) Explore examples

> **Step 2: Explore the given examples (IF given) for different inputs, edge cases and possible inputs that might cause bugs.**

In the FizzBuzz exercise they don't give examples but in some problems and most of the Hackerrank ones do, and this is a BIG help to understanding the problem.

Sometimes you think **the input is an array but it's an object**, that's something to look for, if they don't explicitly say, Ask them.

## 3.) Elaborate Pseudo Code

> **Step 3: Write how it fits best for you pseudo code (can be comments inside the code too), the important note to take here is to BREAK THE CODE INTO STEPS**

For our example it can be:

```javascript
// ## Pseudo code
loop from (1 to 100):
	if currNum % 3 === 0 AND curNum % 5 === 0:
		print("FizzBuzz")
	else if curNum % 3 === 0:
		print ("Fizz")
	else if currNum % 5 === 0:
		print("Buzz")
	else:
		print (currNum)
end loop
```

## 4.) Solve by pieces

Now that you broke into blocks the approach, **solve by pieces and if you get stuck or don't know how to implement you can skip that part** and do what you know, because you now have a plan.

> **Step 4: Solve step by step according to your pseudo code.**

```javascript
function fizzBuzz() {
	for (let i = 1; i <= 100; i++) {
		if (i % 3 === 0 && i % 5 === 0) {
			console.log("FizzBuzz");
		} else if (i % 3 === 0) {
			console.log("Fizz");
		} else if (i % 5 === 0) {
			console.log("Buzz");
		} else {
			console.log(i);
		}
	}
}
```

## 5.) Refactor

If you couldn't solve the problem **it's fine because you logically approached it** and you **demonstrated your thought process**, that's very important for you and the interviewer.

Also, if you are practicing and you can't solve it, **try again later**, take a break and come again later, most of the time the solution will popup in your head. If it doesn't you probabily know where you need to improve.

But if you successfully solved the problem and you still have time I highly recommend to **refactor the code**. Reduce it's time and space complexity, make it more readable, add comments and see other people's submissions.

Although this step is OPTIONAL but if you have time I would **dive deeper into the code** you just wrote and make it better.

Thank you very much for reading, I wish you the best in your coding journey.
