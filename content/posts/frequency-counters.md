---
title: "Algorithm Patterns: Frequency Counter"
date: 2019-04-15T20:43:04+01:00
draft: false
description: "If you're working on strings or arrays problems this one is a good candidate for that kind of problems."
tags: [computer-science, javascript, algorithms]
---

Along side with my other post about [Problem Solving Techniques]([https://taquelim.netlify.com/blog/problemsolving]https://taquelim.netlify.com/blog/problemsolving), in this one I will explain in more detail a pattern to solve algorithms, **this is the first part from a collection of posts about algorithm patterns**

What I'm about to talk here is mostly based on the [Colt Steele](https://www.udemy.com/js-algorithms-and-data-structures-masterclass/#instructor-1) course on JavaScript algorithms which I highly recommend.

## Frequency Counters

If you're working on strings or arrays problems this one is a good candidate for that kind of problems.

It works by utilizing one or more objects to store data and record it's occurrences in order to avoid a O(n^2).

Objects are good for this kind of task because **storing and accessing from data in objects is O(1)** then all we need to be worried about is to loop it.

Common problems are anagrams strings, intersections, duplicates, count unique values, and more...

### Problem

> _Given two arrays, write a function to compute their intersections._

-   Each element in the result should appear as many times as it shows in both arrays.
-   The result can be in any order.

### Example #1

**Input:** `nums1 = [1,2,2,1], nums2 = [2,2]`
**Output:** `[2,2]`

### Example #2

**Input:** `nums1 = [4,9,5], nums2 = [9,4,9,8,4]`
**Output:** `[4,9]`

```js
function intersectionArr(nums1, nums2) {
	const freq1 = {};
	const freq2 = {};
	const outpt = []; // intersected values

	// Transform the Input arrays into frequencies
	for (let num of nums1) {
		freq1[num] = freq1[num] ? (freq1[num] += 1) : 1;
	}
	for (let num of nums2) {
		freq2[num] = freq2[num] ? (freq2[num] += 1) : 1;
	}

	// Calculate the intersections
	for (let num in freq1) {
		if (num in freq2) {
			// if they have the same frequency, add n times
			const lowestCommon = Math.min(freq1[num], freq2[num]);
			outpt.push(...new Array(lowestCommon).fill(num));
		}
	}
	return outpt;
}
```

The frequencies will look something like this:

```js
{ '1': 2, '2': 2 }
{ '2': 2 }
```

As you can see this solution's complexity is something like O(n), which would have been O(n^2) if we did a nested loop.

Although this approach is not perfect, it uses more memory then others, but it's 94% faster than most.
