---
layout: post
title: Understanding Ruby's Symbol/Block Syntax
categories:
  - Tech
  - Ruby
date: 2018-12-10
---

If you have been using Ruby for any period of time you will be familiar with the following syntax:

```ruby
["1", "2", "4"].map(&:to_i)
=> [1, 2, 4]
```

Calling `map` (a method expecting a block) with `&:to_i` calls the `to_i` method on each of the array elements, but why does this work?

### The & operator

The & operator when used with method calls allows passing a `Proc` object as if it were passed as a block.

Example:

```ruby
times2 = Proc.new { |num| num * 2 }
[1, 2, 4].map(&times2)
=> [2, 4, 8]
```

The reason that this also works when passed a `Symbol` (such as `:to_i` in the example above) is because the & operator also works with objects that implement a `to_proc` method.

### Symbol#to_proc

The `Symbol` class implementation of `to_proc` returns a `Proc` that will call the method matching the symbol name on the object that is passed.

Example:

```ruby
upcase_proc = :upcase.to_proc # Proc.new { |object| object.send(:upcase) }
upcase_proc.call('test')
=> "TEST"
```

Another example:

```ruby
sort_proc = :sort.to_proc
sort_proc.call([4, 1, 6, 2])
=> [1, 2, 4, 6]
```

So what is actually happening in the `map(&:to_i)` example above is that the `:to_i` symbol is being converted to a `Proc` by calling its `to_proc` method and then being passed to the `map` function as if it were a block.

