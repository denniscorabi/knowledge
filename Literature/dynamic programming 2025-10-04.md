# Dynamic programming

**Dynamic programming** is a programming technique that aims to build **optimised** algorithms by breaking a problem in subproblems in a recursive fashion. 

> [!important]
> When DP is applicable, execution time decreases drastically, usually from exponential to polynomial time.

There are two prerequisites to verify before a problem is treatable as a DP problem:
1. **Optimal substructure**
A problem has an optimal substructure whenever the optimal solutions to subproblems are needed to compute the optimal solution of the full problem.
This implies that the computational problem can be described in a recursive fashion.

2. **Overlapping subproblems** 
To determine the optimal solution to the subproblems, many, many of their subproblems overlaps (i.e. the same subproblem is computed more than once). This caveat of a pure recursive implementation of an algorithms is what makes the execution time grow exponentially.
DP comes to the rescue with two techniques:
	1. **Memoization**
	2. Bottom-up implementation

## Subproblems graph

An alternative way to visualise the optimal substructure of a problem is to draw the **subproblems graph**, where each node is a subproblem and each vertex a possible choice

[![subproblem](https://gsourcecode.wordpress.com/wp-content/uploads/2012/04/screenshot-at-2012-04-14-164607.png)

The subproblem graph is on the right, meanwhile the complete recursion tree is shown on the left.
The former clearly visualises the *dependencies* of, for instance, subproblem 3. To determine its optimal solution, it requires the optimal solutions of subproblems 2,1 and 0.

> [!important] 
> The execution time of a problem depends of the execution time to solve all its subproblems, plus the time to determine the optimal solution.

