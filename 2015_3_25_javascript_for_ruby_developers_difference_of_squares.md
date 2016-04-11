I found early on at [Turing](http://turing.io) that I happened to like JavaScript.  So much so that I now have the privilege (or unfortunate task, depending on who you ask) of writing it professionally.  It has some serious quirks, it can be a nightmare to debug (don't forget those `var`'s!), but as a language it's relatively compact and has the necessary tools that make programming enjoyable for me.  It also happens to have everything you could want in a language platform.

As time went on, I ended up writing a lot of JavaScript with students.  One of the biggest hurdles people had was how unfamiliar the syntax was.  JavaScript at first glance seemed different enough to warrant having to mentally 'start over' so to speak -- as opposed to realizing and taking advantage of some of the core similarities it has with Ruby, their most familiar language.  Many just assumed there was some core piece, some fundamental nature of JavaScript they had yet to unlock.  In reality this was never the case.  Once we explored a few key shared patterns, the lightbulbs began to pop on.

The goal of this post is to encourage those learning Ruby and picking up JavaScript as a second language to use your existing knowledge of Ruby to your advantage.  Ruby espouses a lot of great things that make your code readable, concise, flexible, and generally awesome:

* small, focused functions/methods
* use of powerhouse enumerables like `map`, `reduce`, and `select` (`filter` in JavaScript) for readability and expressiveness
* frequent use of [higher-order functions](http://en.wikipedia.org/wiki/Higher-order_function) (known as blocks in Ruby, and anonymous functions in JavaScript)

If you're comfortable with blocks and enumerables in Ruby, you're already a better JavaScript programmer than you may give yourself credit for (yay!).

At the end of the day programming languages are just syntax, a way to express your thoughts in computer compatible form.  Languages certainly color the way you think, but the leap to JavaScript from Ruby in my opinion is a small one.

One pattern we'll look at here is frequent use of higher-order functions (that is, functions that can be be passed as arguments to and returned from other functions).  Both languages share this idea.  As mentioned, in Ruby you may know them as blocks, in JavaScript we'd refer to them as anonymous functions.  Each language's implementation of this concept may look different on the surface, but conceptually they are the same: they allow you to package code and pass it around your programs.

Here's a quick example of a function that doubles each number in a collection:

```ruby
def double(nums)
  nums.map { |num| num * 2 }
end
```

```javascript
function double(nums) {
  return nums.map(function(num) {
    return num * 2;
  });
}
```

Here's another larger example we'll do in both languages which uses blocks/anonymous functions to produce concise, declatarive code.

Our task: [Project Euler #6](https://projecteuler.net/problem=6).
The short version: **Find the difference between the sum of the squares of the first one hundred natural numbers and the square of the sum.**

How we think about the problem at a high level is identical.

Here are some high level tasks:

* Find the sum of all numbers in the collection and then square the result (square of sum).
* Square each number in the collection and then calculate the sum (sum of squares).
* Calculate the difference between the two.

In Ruby:

```ruby
def square_of_sum(nums)
  nums.reduce(:+) ** 2
end

def sum_of_squares(nums)
  nums.reduce do |sum, num|
    sum + (num ** 2)
  end
end

def difference(nums)
  square_of_sums(nums) - sum_of_squares(nums)
end

nums = (1..100)
puts difference(nums)
```

With JavaScript, there is no doubt the syntax is noisier.  Ruby gives us some neat shortcuts (like reduce(:+)), but the code, though slightly more verbose, is fundamentaly similar:

```javascript
function squareOfSum(nums) {
  var total = nums.reduce(function(sum, num) {
    return sum + num;
  });

  return total * total;
}

function sumOfSquares(nums) {
  return nums.reduce(function(sum, num) {
    return sum + (num * num);
  });
}

function difference(nums) {
  return squareOfSum(nums) - sumOfSquares(nums);
}

nums = range(1, 100);
console.log(difference(nums));

// Helper to generate an array of numbers between 1-100
function range(start, end) {
  var array = [];
  for (var i = start; i <= end; i++) {
    a.push(i);
  }
  return array;
}
```

In the end, I hope these examples were helpful in your journey to slay the JavaScript dragon.  Think of learning new languages like learning a new instrument in the string family.  Your skills on the guitar will make learning the banjo all that much easier.
