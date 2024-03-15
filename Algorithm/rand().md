https://www.reddit.com/r/learnprogramming/comments/5uu9eu/generating_random_number_in_c/

What does that mean? Lets check our example assuming RAND_MAX=32767. What does rand()%100 do for all possible return values of rand()?

{0,100,200,300,...,32700} -> 0

{1,101,201,301,...,32701} -> 1

...

{67,167,267,367,...,32767} -> 67

{68,168,268,368,...,32668} -> 68

Have you seen it? Since RAND_MAX < 32668 there is one number less being projected to 68 than to 67. And this effect continues for bigger numbers in the interval [0,100[. This is no uniform distribution and therefore some numbers are more likely to appear than other! Thats now what you want from a random number generator.