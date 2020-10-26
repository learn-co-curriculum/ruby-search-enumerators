# Ruby Search Enumerators

## Objectives

1. Understand return values for enumerators.
2. Use a truthy or falsey evaluation in a block.
3. Use `#select` to select matching elements from a collection based on a block.
4. Use `#detect` to find a matching element from a collection based on a block.
5. Use `#reject` to filter matching elements from a collection based on a block. 

## Overview

Every method in ruby must return a value. When we iterate or enumerate over a collection with `#each`, the return value is always the original collection. This is an example of a static return value, no matter what we do with `#each`, it will always return the same object that received the call to `#each`.

```ruby
["Red", "Yellow", "Blue"].each do |color|
  puts "There are #{color.length} letters in #{color}"
end #=> ["Red", "Yellow", "Blue"]
```

Often we want to search for elements in a collection based on a condition. Imagine wanting to find all even numbers in a collection of numbers using `#each`.

```ruby
matches = []
[1,2,3,4,5].each do |i|
  matches << i if i.even? # add i to the matches array if it is even
end #=> [1,2,3,4,5]
matches #=> [2,4]
```

Implementing a selection routine with a low-level enumerator like `#each` is costly in a few ways.

1. We have to hang on to the matches within the local array `matches`.
   Programmers use the phrase **maintain state** to refer to this task. Cars
   can be in a state like "Reverse, Drive, Neutral". Our `matches` array has
   states of "Empty, `[2]`, `[2,4]`".
2. Our block is complicated with conditional logic that can be implicit with a better enumerator.
3. Our code lacks intention and clear semantics. If we mean, `#find_all` or `#select`, why don't we just say that?

## `#select`

When you evoke `#select` on a collection, the return value will be a new array containing all the elements of the collection that cause the block passed to `#select` to return true. That means for each iteration, if the block evaluates to true, the element yielded to that iteration will be kept in the return value array.

```ruby
[1,2,3,4,5].select do |number|
  number.even?
end #=> [2,4]
```

In the first iteration of the block above, `number` will be assigned the value `1`. Because `1.even?` will return false, `1` **will not** be in the return array for this call to `#select` (same for `3` and `5`). In the second iteration, `number` will be `2`. Because `2.even?` will return true, `2` **will** be in the return array (same for `4`).

You can see the clarity and expressiveness of this syntax in the short block from below.

```ruby
[1,2,3,4,5].select{|i| i.odd?} #=> [1,3,5]

[1,2,3].select{|i| i.is_a?(String)} #=> []
```

Notice that if no element makes the block evaluate to `true`, an empty array is returned.

## `#detect` or `#find`

*NOTE: `detect` and `find` are two names for the same method. For every example below we'll use `detect`, but you can use them interchangeably.*

Whereas `#select` will return all elements from the original collection that cause the block to evaluate to true, `#detect` will only return the first element that makes the block true.

```ruby
[1,2,3].detect{|i| i.odd?} #=> 1
```

```ruby
[1,2,3].find{|i| i.odd?} #=> 1
```

As you can see, even though both `1` and `3` would cause the block to evaluate to true, because `1` is first in the array, it alone is returned.

```ruby
[1,2,3,4].detect{|i| i.even?} #=> 2
[1,2,3,4].detect{|i| i.is_a?(String)} #=> nil
```

Notice also that `#detect` will always return a single object where `#select` will always return an array.

## `#reject`

`#reject` will return an array with the elements for which the block is false.

```ruby
[1,2].reject{|i| i.even?} #=> [1]
```

## Conclusion

`#select`, `#detect`, and `#reject` are part of a family of search and filter type enumerators whose purpose is to help you refine a collection to only matching elements. They are way easier to manage than using lower-level methods like `#each` and create meaningful return values based on expressions in a block.

## Video Review 

* [Yield and Enumerables](https://www.youtube.com/watch?v=t2A6xPbh0I8&feature=youtu.be)


