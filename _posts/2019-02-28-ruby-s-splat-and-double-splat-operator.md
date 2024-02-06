---
layout: post
title: Ruby's Splat and Double-Splat Operators
categories:
  - Tech
  - Ruby
date: 2018-12-14
---

## Splat `*` 

The splat `*` operator in Ruby is used for converting array elements into individual arguments or collecting arguments into an array.

### Parallel Assignment

When used in parallel assignment the variable with the splat will collect all unassigned values from the right hand side:

```ruby
first, second, *rest = [1, 2, 3, 4, 5]
p first
# 1
p second
# 2
p rest
# [3, 4, 5]
```

```ruby
first, *rest, last = 1, 2, 3, 4, 5
p first
# 1
p last
# 5
p rest
# [2, 3, 4]
```

### Unpacking Arrays

When used with an array, each of the array elements will be treated as an individual argument (similar to Javascript's Spread):

```ruby
ids = [1, 2, 3]
new_ids = [0, *ids]
p new_ids
# [0, 1, 2, 3]
```

This can also be used with method calls to pass each array element as an individual argument:

```ruby
class Person
  def initialize(first, last)
    @first, @last = first, last
  end

  def name
    "#{@first} #{@last}"
  end
end

names = ['James', 'Smith']

person = Person.new(*names)
p person.name
# "James Smith"
```

### Variable Length Argument Lists

When used in method definitions, the method will accept an undetermined amount of arguments and collect them into an array:

```ruby
class Account
  attr_reader :balance

  def initialize
    @balance = 0
  end

  def deposit(*amounts)
    amounts.each { |a| @balance += a }
  end
end

account = Account.new
account.deposit(10)
p account.balance
# 10
account.deposit(150, 25, 23, 18)
p account.balance
# 226
```

## Double-Splat `**`

The double-splat `**` operator has similar behavior to the single-splat but is intended for use with hashes and keyword arguments.

### Unpacking Hashes

Hashes can be "unpacked" into other hashes:

```ruby
name = { first_name: 'Tim', last_name: 'Jones' }
attrs = { **name, address: '100 Main St' }
p attrs
# {:first_name=>"Tim", :last_name=>"Jones", :address=>"100 Main St"}
```

### Variable Keyword Arguments

Methods defined with keyword arguments will raise an error if they receive any other options besides those that are defined. A way to make these methods more flexible is to use a double-splat to capture any undefined arguments passed and collect them into a hash:

```ruby
def format_transaction(amount:, type:, **options)
  {
    amount: amount,
    type: type,
    options: options
  }
end

trx = format_transaction(
  amount: 10.40,
  type: 'deposit',
  client: 'Alo LLC',
  ref: 'Invoice #123'
)
p trx
# {:amount=>10.4, :type=>"deposit", :options=>{:client=>"Alo LLC", :ref=>"Invoice #123"}}
```

### Variable Length Argument Lists with Options

The double-splat can also be combined with a single-splat in method definitions to accept variable length arguments with an (optional) options hash as the last argument:

```ruby
def log(*values, **options)
  output = values.join(' | ')
  puts [options[:service], output].compact.join(' :: ')
end

log('Firefox', 'Mac OSX', '123.131.11.1')
# Firefox | Mac OSX | 123.131.11.1 
log('Firefox', 'Mac OSX', '123.131.11.1', service: 'API')
# API :: Firefox | Mac OSX | 123.131.11.1 
```
