# seq

Defines a Seq class, which cycles over elements in an array.

```ruby
s = Seq.new([1,2])
s.next #=> 1
s.next #=> 2
s.next #=> 1
# and so on forever
```

They can be constrained to only return `n` items,

```ruby
s = Seq.new([1,2,3], 2)
s.next #=> 1
s.next #=> 2
s.next #=> nil
s.next #=> nil
```

and can start from an offset.

```ruby
s = Seq.new([1,2,3], 2, 1)
s.next #=> 2
s.next #=> 3
s.next #=> nil
s.next #=> nil
```

You can also provide a different default (as opposed to `nil`).

```ruby
list = %w(a b c d)
s = Seq.new(list, 3, 0, list.last)
s.next #=> a
s.next #=> b
s.next #=> c
s.next #=> d
s.next #=> d
```

Seq has `#each` defined and includes the `Enumerable` module so the usual stuff applies.

```ruby
s = Seq.new([1,2,3,4], 10)
s.map {|i| i * 2 } #=> [2, 4, 6, 8, 2, 4, 6, 8, 2, 4]
s.next #=> nil

# Standard methods will increment the number of items that have been returned meaning
# you have to call #reset
s.reset 
s.next #=> 1

# You can use special ! versions to avoid incrementing the number of cycles, but note
# this will start and finish based on the current index and items returned. So here 
# we are on the second item in the original list, ie. "2".

s.map! {|i| i * 2 } #=> [4, 6, 8, 2, 4, 6, 8, 2, 4]

# Note this only returned 9 items as we had already called #next, and it started on 2
# which was mapped to 4.

s.reset
s.entries #=> [1, 2, 3, 4, 1, 2, 3, 4, 1, 2]
s.reject! {|i| i < 3 } #=> [3, 4, 3, 4]

s.take(4) #=> [1, 2, 3, 4]
s.entries #=> [1, 2, 3, 4, 1, 2]
s.reject! {|i| i < 3 } #=> [3, 4]
```
