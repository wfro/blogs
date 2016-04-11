> Originally published on October 31, 2014.

A quick intro after the fact, I recently ran a two hour intro to Clojure workshop for my fellow students at [Turing](http://turing.io).  I thought it would be a good idea to post the gist we used as a guide.  This is crafted largely (nearly 100%) from the great content in Daniel Higginbotham's book [Clojure for the Brave and True](http://www.braveclojure.com/).  Read it, it's amazing.

HI.

[Clojure on wikipedia](https://en.wikipedia.org/wiki/Clojure)
[Offical docs](http://clojure.org/)

# Install the JDK/Clojure

So the major acronyms you'll come across are the JVM, the JRE, and the JDK.  Not confusing at all.  From the top:

##### JVM - Java Virtual Machine; JRE - an implementation of the JVM

According to wikipedia the JVM is the 'code execution component of the Java platform'. Clojure is written in and tightly integrated with Java.  Both languages compile into bytecode that the JVM understands and executes.

##### JDK - Java Development Kit

A bundle of software used to develop Java applications.  It includes the JRE, a bunch of Java classes, a compiler, and a bunch of other stuff you need to run java/clojure programs.  On the Java side of things this will be the only thing you'll need to install.

[Click here, follow those instructions (for the most part it's just like installing any other program on os x)](http://docs.oracle.com/javase/7/docs/webnotes/install/mac/mac-jdk.html)

# Install Clojure/Leiningen

```shell
$ brew update
$ brew install leiningen
```

Annnnnnnnnnd hopefully it just works :D

Now you'll want to use leiningen (similar to tools like bundle) to create a new project and get a REPL running.

```shell
$ lein new app clojure_noob
$ cd clojure_noob
$ lein repl
```

# Functional programming

[Wikipedia is smart](https://en.wikipedia.org/wiki/Functional_programming):

```hi
In computer science, functional programming is a programming paradigm, a style of building the structure and elements of computer programs, that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data.
```

The main high level things to take away:

- Clojure is a lisp dialect and has a uniform structure of `(operand operator1 operator2 ...)`
- Clojure's data structures are immutable
- Clojure is very good at concurrency; Mutation is problematic for programs accessing shared data structures.
- 'Pure functions' always return the same value given the same input.
- The goal in functional programming is to make it easy to predict how a program
  will behave based on functions not caring about the 'state of the world'.
- Clojure is used on the web, and ClojureScript allows you to write clojure code
  and compile it to javascript.
- Functional programming is a tool, just like Object-oriented programming is a
  tool.  They are constructs designed to reason about programs effectively.


# Forms and Atoms

Atoms in clojure are the more basic, or 'primitive' types.  Numbers, strings, that sort of thing.

```clojure
(+ 3 3)
; => 6

(str "You know nothing " "Jon Snow") ;
; => "You know nothing Jon Snow"
```
A 'form' is just another way to describe an expression: any syntactically valid code.  Literal data representations like numbers and strings are valid forms, but you'll be using them in functions, the main building block of clojure programs.

By convention symbols/vars are written in 'kebab-case', or with dashes as delimiters.  So in javascript you'd use camelcase: longVariableName, in ruby you'd use underscores: long_variable_name, and in clojure you'd use: long-variable-name.  Also, get used to double quotes, single quotes to identify a string aren't valid syntax in clojure.

One exciting thing about clojure and all lisps is their uniform structure.  All operations take the form of open parentheses, operand, operators, close parentheses.  This includes things like arithmetic operations.

```clojure
(+ 3 3)
; => 6

(println "I'm a little teapot")
; => "I'm a little teapot"
```

# Data structures in clojure are immutable, meaning they can't change in place

You can do this in ruby:

```ruby
stuff = ['snape', 'killed', 'harry potter']
stuff[0] = 'roger rabbit'

stuff
; => ['roger rabbit', 'killed', 'harry potter']
```

In clojure, state is immutable, meaning any data you have in memory can't be modified after it's been created.  This one takes quite a bit of getting used to.  But there are some clear advantages, an example being that immutable structures are thread-safe by default (Clojure is known for handling concurrency extremely well).

# More Complex Types

##### Maps

Similar to hashes in ruby, maps are a collection of name-value pairs.  Map keys are often keywords, which are symbolic identifiers that can also be used as functions to look up things in maps.  Map values can be atoms like numbers and strings, as well as more complex datatypes like vectors and other maps.  Oh yeah, also commas aren't used in clojure maps as delimiters, you'll use whitespace
to separate forms instead.  So `{:a 1 :b "a string" :c []}` is syntactically the same as the following example.

```clojure
;; a map
{:a 1
 :b "a string"
 :c []}

;; use get to look up values in a map
;; To give you a better idea of everything that is happening, here
;; we're 1) calling the get function, 2) passing in a first argument
;; of the map {:a 1 :b 2}, and 3) passing in a second argument :a
(get {:a 1 :b 2} :a)
; => 1

;; Use a keyword as a function to get values in a map.
;; This is the same as the above example using the get function
(:a {:a 1 :b 2})
; => 1
```

##### Vectors

Like arrays in ruby, they are indexed collections.  Similar to maps they can contain primitive and complex types including maps, other vectors, etc.

```clojure
;; vector literal
[1 2 3 4]

;; accessed via the get function
(get [1 2 3 4] 0)
; => 1

;; vector's can contain other vectors as well as data
;; structures like maps
(get [1 "a string" [12 20 "another string"]] 2)
; => [12 20 "another string"]
```

##### Lists

Lists are like vectors in that they are linear/ordered collections of values.  Notice they are quoted, that's actually kind of complicated so let's google that together :D.

```clojure
;; list literal
'(1 2 3 4)
```

##### Sets

Sets are collections of unique values.

```clojure
#{1 223 "higginbotham"}
```

##### Symbols and naming things

Symbols are identifiers used to refer to some value.  You can bind a symbol to some value with `def`.  One important thing to notice is that we used the word "bind" to refer to associated a name with a value.  This is in contrast to "assigning" a variable, which is a construct you'd use in a language like Ruby.  Assignment implies that a value in memory can be "reset".  From [Wikipedia](https://en.wikipedia.org/wiki/Assignment_(computer_science)), Assignment "...copies the value into the variable".  Remember the whole immutable state thing?  Clojure doesn't allow us to modify values in memory in place, hence why it's more accurate to describe giving names to values in clojure as bindings.

```clojure
(def my-map {:a 10 :b 20})

my-map
; => {:b 20, :a 10}
```

# The almighty function

As mentioned all 'operations' (functions) are uniform: adding two numbers is the same as calling map or reduce.  Here are a few examples, keep an eye on a few new concepts: anonymous functions and the defn macro.

```clojure
(+ 10 20) ; => 30

;; passing map an anonymous function which will square each element
;; of the vector [1 2 3 4]
(map (fn [x] (* x x)) [1 2 3 4])
; => (1 4 9 16)

;; this can also be achieved as:
(defn square
  [x]
  (* x x))

(map square [1 2 3 4])
```

As seen above, to define a function in clojure, you'll use defn, the function name, a list of arguments within square brackets, and a function body.

##### Control flow

If statements in clojure look like this:

```clojure
(if some-condition
  execute-me-if-true
  execute-me-if-false)

;; So:
(if true
  (println "Yes")
  (println "No"))
; => "Yes"

;; By default if only takes one form, use do to wrap multiple forms
(if true
 (do (println "I'm a little teapot")
     ("I'm also a little teapot"))
 (do (println "This won't get executed")
     (println "That's sad")))
```

##### Arity

Arity refers to the number of arguments a function accepts.  You can 'overload by arity' in clojure, meaning you invoke different behavior based on the number of arguments passed into a function.

```clojure
(defn multi-arity
  ([arg-one arg-two]
     (println (str "you know nothing " arg-one " " arg-two)))
  ([arg-one]
     (println (str "I like " arg-one))))

(multi-arity "jon" "snow")
; => "you know nothing jon snow"
(multi-arity "bananas")
; => "I like bananas"
```

##### Rest params

Say you have a collection ["word" "another word" "and another word"].  Often you'll want access to the first element (the head) to do some work with it, and pass the rest of the collection (the tail) back into the function using recursion.  You can do that using 'rest params', saying 'take the rest of these arguments and give it a name for me.'

```clojure
(defn oregon-trail
  [useful-item & non-useful-items]
  (str "I def need my " useful-item)
  (str "I def don't need this stuff: " (clojure.string/join ", " non-useful-items)))

(oregon-trail "guns" "malaria" "broken wheels")
; => "I def need my guns"
; => "I def don't need this stuff: malaria, broken wheels"
```

##### Let

Ok last one I promise.  Let allows you to bind names to values within a particular scope.  They are human-friendly constructs, meant to make your code more readable by reusing names that refer to values instead of the values themselves.

A super simple example:

```clojure
(let [x 1
	  y 2]
  (+ x y))
; => 3
```

# Some stuff to check out

- [ClojureScript](http://github.com/clojure/clojurescript) A compiler that
  targets JavaScript (meaning it produces/emits/whatevers JavaScript code).
- [Compojure](https://github.com/weavejester/compojure) is a web framework similar
  to sinatra
- [Ring](https://github.com/ring-clojure) is an HTTP server abstraction similar to
  WSGI in python and Rack.

# Check out Clojure for the Brave and True

So this workshop was essentially summarized from a chapter called 'Do Things' in
Daniel Higginbotham's (what a last name) amazing book 'Clojure for the Brave and
True'.  [The book is available for free online, but if you like it I recommend
you buy it.](http://braveclojure.com/)
