---
title:  "Can computers be truly random?"
date:   2016-03-07 2:50:00
categories: [jekyll]
tags: [javscript]
---
         


How does a computer generate a random number? To answer this question let's first try to understand why random numbers are so important. Besides the obvious things like creating a random outcome in a video game, randomness is important for cryptography. 

Cryptography is crucial to some of the most important tasks that are performed on the internet such as HTTPS connections and data encryption. An algorithm cannot just use the same numbers, therefore it must generate them in an unpredictable manner, so that no other algorithm or hacker can crack it.

Therefore, we must distinguish the differences between a truly random number and one that is created by a computer algorithm. Before we explain how a truly random is generated let's examine the following simple code:

``` ruby
  _.shuffle = function(array) {
    var newArr = array.slice(0);
      _.each(newArr, function(element, index) {
        var random = Math.floor(Math.random() * index);
        var temp = newArr[index];
        newArr[index] = newArr[random];
        newArr[random] = temp;
      });
      return newArr;
   };
```

Let's look at a simple shuffling function that:

1. Takes an array as an argument
2. Copies the array into newArr
3. Iterates over the newArr using _.each
4. Generates a random number using Math.random()
5. Assigns the temp variable to a random index in the newArr
6. Returns the Array

If we compare these two arrays, what are the chances that our newArr matches the original array? That depends on the amount of elements in that array of course, but due to the nature of this function it's not as random as we would hope. This function's randomness depends solely on the  built in JavaScript function Math.random(). 

Now its randomness also depends on the engine that compiles it, but all in all it is a pseudo random generator. For small numbers and daily use, it is perfectly fine, but for larger numbers one can see a pattern. 

John von Neumann once said that: "Anyone who considers arithmetical methods of producing random digits is, of course, in a state of sin." 

This is where it gets interesting, for a computer to generate a 'true' random number, it has to measure an outside physical phenomenon, like the radioactive decay of an atom, where according to quantum theory there is no way to know when the radioactive decay will occur, therefore it is essentially pure randomness created by the universe. 

Most common cases for generating random numbers is to rely on atmospheric noise or use some source of unpredictable data which is called entropy. Entropy can be collected in the means of random keystrokes and their timing.

By now it is obvious to us that generating random numbers is important, because when we use pseudo random numbers, an attacker could match it with an algorithm and predict the results.

To generate a random number you can checkout:    
[random.org] 

[random.org]:      https://www.random.org/

