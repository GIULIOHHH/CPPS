# Reading a problem
- Read the problem slowly.
- Make sure every statement doesnt conflict with your understanding.
- DO NOT IGNORE CONSTRAINTS.
	- Ignoring constraints can make you solve a complex problem trivially $n\le10^{18}$ 
	- Ignoring constraints can make you solve a trivial problem in a complex way $n<10$
	- Constraints turn a general problem into a more special case, which can have a simpler solution.
- Sometimes samples are trivial!
	- Think of all basic cases.
	- Think of special cases.
	- Think of extreme cases:
		- Smallest possible.
		- Largest possible.
- Reread the problem.
# Thinking 
- Have the idea sketched out in your mind or on paper before coding.
- Brainstorm different possible solutions (DP, greedy, ad-hoc)
	- Try to do the most probable idea.
	- If a lot of time passes try to do the next most probable.
	- No idea is a bad idea.

## Problem Abstraction
We redefine a problem in a more generic way.
>Minimum cost to convert string A to string B.
>We can abstract it to the shortest path from node A to node B.

It helps us find the target algorithm faster.
Be careful to not drop important constraints or characteristics. 

## Reversing a Problem.
Often the reverse of a problem is a lot faster to find.
>Given an increasing/decreasing sequence like Fibonacci, given a value, find its index. --->
>Given an index find its value. 
>Then binary search until we match the original value.

## Problem Simplification
This can help building up intuition.

- Most hard problems are made up of sub-problems.
	- If we get stuck on a subproblem we can always try to formulate a different division into subproblems.

- We can think of a special problem, and then try to transition into the main case.
	- We should always have a vision on how to transition to not waste time.
>Given a 2d array -> What if the array is $1*1$? $2*1$? $1*2$? $2*2$?
>Given a polygon -> What if its a triangle? How about a square? How about a convex polygon?

- We can make some assumptions to make intuition easier.
>We have to find X Y and Z but cant think of them together.
>What if we already solved X, how would we solve Y and Z? What if we already solved X and Y, how would we solve Z?

## Thinking incrementally
We can process the input step by step, getting each step from a modification of the previous one.
>Sort N numbers. 
>Lets say we have already sorted the first m numbers, how do we add the m+1 item?

If updating from one state to another is in constant space and follows a pattern, we can use [[Matrix Exponentiation]].

# Domain re-interpretation
Sometimes rethinking a problem in another domain makes solving it faster.

>Given some boxes, each contains a key and is opened by another key. Given a starting key can we open all of them? 
>We can think of keys as nodes. We move between keys by opening boxes. The solution is an euler path.

https://github.com/mostafa-saad/ArabicCompetitiveProgramming/blob/master/05%20Thinking%20Techniques/Algorithms_Practice_13_Thinking_Search_Space_and_Output_Analysis.txt