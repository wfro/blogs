> Originally published on November 5, 2014.

I've been trying to improve my algorithmic thinking and thought it would be a perfect time to revisit the awesomeness that is Project Euler.  When I was first starting out I did the first few problems in python, but abandoned PE in favor of other learning adventures.  Now that I've been programming for a while I feel like I'm at a good place to tackle the more difficult algorithms (and now in multiple languages!).  

Each post will wholly contain everything related to a particular problem, with a combination of solutions in Ruby, Javascript, and Clojure (I'd rank my language expertise in that order).  So, I won't consider myself victorious until we reach post 50!  Hopefully I'll do better than Sufjan Stevens and the 50 states.  The posts themselves I'd like to evolve over time as I come across new interesting ways to solve a particular problem.

Project Euler is a fun way to see how different hammers can be used for the same nail.  I like seeing the way different languages and paradigms let you express yourself as a programmer, so there will a healthy amount of compare and contrast (especially with javascript being as flexible as it is).

The first few problems are suited to exploring some fundamental ideas including mutable/immutable state, and different techniques for iteration.  You can find [Problem 1](https://projecteuler.net/problem=1) here.  Our goal:

```
If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000.
```

So not so bad, right?  First we'll use an [imperative](en.wikipedia.org/wiki/Imperative_programming) approach.  From wikipedia:

```
In computer science terminologies, imperative programming is a programming paradigm that describes computation in terms of statements that change a program state.
```

Both procedural (C) and object-oriented (java, ruby) programming fall under the umbrella of imperative programming.  There are two main things to think about: imperative programming describes "how" to accomplish some goal, and it's big concern is tracking changes in state.  Essentially you tell the computer "how" to solve a problem by giving the computer very specific tasks which achieve some desired goal.

With problem 1, it's apparent we'll need to track some kind of running total.  We'll write a function that builds up the value of a variable based on some conditions, and returns the final value.

Pseudocode ahoy:

```
# Generate an array from 1 up to 1000 exclusive
# Initialize a variable to store our running total
# Iterate over each element in the collection
# If the current element is divisible by 3 or 5
# 	Add it's value to the running total
# else continue with the loop
# Return the total
```

Now we have a general outline for the tasks that need to get accomplished and we can begin to translate them into actual code.

The ruby solutions here highlight something that's true in many languages but especially true in ruby: there is always more than one way to do things.  This applies in lots of different contexts, in this context it's the difference between using more basic constructs like for loops and `#each`, and more complex enumerables like `#reduce`.  In my experience it's generally preferred to use more specific enumerables as they often allow you write less code, but always keep in mind there are exceptions.  One of the most valuable lessons you can learn as a developer is that there are no hard and fast rules.  Every problem is unique and has its own set of circumstances.  The first solution here uses `Enumerable#each`:

```ruby
def sum_of_multiples(up_to)
  nums = (3...up_to).to_a
  total = 0

  nums.each do |num|
    if num % 3 == 0 || num % 5 == 0
      total += num
    end
  end

  total
end
```

Ruby also has a bunch of powerful enumerable methods, on the conveniently named Enumerable module.  `Enumerable#reduce` in particular is perfectly suited for this problem.  `reduce` in my opinion is one of the hardest to understand, but also one of the coolest.  The idea is that you 'reduce' (some languages use 'fold') a collection into a single value.  For our purposes here we'll use it in one of the most straightforward ways possible, by computing a sum from a collection (array) of numbers.  You could also use `reduce` to: find the min/max value from a collection (though ruby provides you additional abstractions for that), and building up the value of a hash.  Here is that solution:

```ruby
def sum_of_multiples_reduce(up_to)
  nums = (3...up_to).to_a
  nums.reduce(0) do |sum, num|
    if num % 3 == 0 || num % 5 == 0
      sum + num
    else
      sum
    end
  end
end
```

It's worth mentioning why I had to have `sum` return as part of the else condition.  Here's a simple example of using reduce to find the sum of a collection:

```ruby
nums = [1, 2, 3, 4]

nums.reduce(0) { |sum, element| sum + element }
# => 10
```

The trick to understanding `reduce` is the 'accumulator'.  Here the accumulator is named `sum`, but that's only because it makes sense in this context.  You'll no doubt find it named other things in other problem domains. `reduce` passes each number (`element`) to the block, and stores the result of that block in the sum variable.  This is the reason why we needed to return `sum` from the else condition.  If we didn't have that if/else branch and the number wasn't divisible by 3 or 5, the block would return nil (thus setting the sum variable, the accumulator, to nil).  The next iteration will execute, and ruby will try to execute `sum + num`, raising an error as ruby will look for the method `+` on a `nil` object.

Here is a javascript example which is nearly identical to the first ruby example.  Here we're using a for loop instead of an enumerable method like `Enumerable#each`.  The principles are still the same though: build up the value of a variable over however many iterations and return the final value.

```javascript
function sumOfMultiples(up_to) {
  var total = 0;

  for (var i = 0; i < up_to; i++) {
    if (i % 3 === 0 || i % 5 === 0)
      total += i;
  }

  return total;
}
```

Next week on Project Euler, all the (functional) things!
