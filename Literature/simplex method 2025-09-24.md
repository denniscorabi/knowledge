# Simplex method

The simplex method is an important algorithm in Operational Research that is used to solve linear programming problems *normally* in a reasonable amount of time.
This algorithm was created by **George Dantzig** in 1946 while working for the US Army Air Force.
## Foundations

A linear programming problem can always be expressed as a linear equation that needs to be maximised (or minimised). Being this problem a mathematical model of a real world problem, there are usually constrains, that can be roughly divided into two categories
- functional constrains
- non-negative constrains

The set of solutions of the objective problem that satisfy the constrains is called **feasible region**.
It has been proved that the optimal solution is (if present) always the value the objective function assumes in at least of the vertex points of the polytope that is shaped after the feasible region.

>[!important]
>If a vertex is not the optimal solution, it means that along at least one of the edges that intersect to make the vertex the value of the objective function is increasing.

This consequence is very important: if we find ourselves in front of a polytope with a finite surface, traversing an edge means that we inevitably approach a different vertex, an **adjacent vertex**, or in terms of the simplex algorithm, another possible optimal solution.
## Procedure

Computing the simplex algorithm means walking along the edges of the feasible region until one the value assumed by the objective function on one of the vertex is greater that the value it assumes on all its adjacent vertices.

> [!important] 
The algorithm continues until the optimum value is reached, or an unbounded edge is found, implying that there exists no optimal solution.

