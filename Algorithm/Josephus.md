# [Josephus Problem](https://writings.sh/post/josephus)

Josephus and other 40 (n) Jewish soldiers were gonna to be captured by Roman army. They decided to kill themselves. They had an idea that to stand in a circle and each man kill the one next (k) to him, one by one. The one who stand last would commit suicide then. How could Josephus live to last?

## Solutions
1. Double Linked List: time: k*n, space: n
2. Josephus Algorithm: time: n, space: 1

## Josephus Algorithm
The key here is a formula: f(n) = (f(n-1) + k) % n 
It means, how people's id in total number of Case(n-1) map to Case(n). 
And, if n is equal to 1, the answer is 0.
Finally, we can get the finally alive one in a recursive way.

### How the formula comes?

![Josephus](https://github.com/vinland-avalon/Readings/blob/main/images/Josephus.png?raw=true)