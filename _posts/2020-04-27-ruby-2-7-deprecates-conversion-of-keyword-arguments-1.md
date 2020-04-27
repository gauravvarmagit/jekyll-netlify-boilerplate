---
layout: post
title: Ruby 2.7 deprecates conversion of keyword arguments
meta_description: Ruby 2.7 deprecates conversion of keyword arguments and positional arguments
author: john_doe
date: 2020-04-26T22:36:17.000Z
intro_paragraph: A notable change has been announced for Ruby 3 for which
  deprecation warning has been added in Ruby 2.7. Ruby 2.7 deprecated automatic
  conversion of keyword arguments and positional arguments. This conversion will
  be completely removed in Ruby 3.
---
[Ruby 2.7 NEWS](https://github.com/ruby/ruby/blob/4643bf5d55af6f79266dd67b69bb6eb4ff82029a/doc/NEWS-2.7.0#the-spec-of-keyword-arguments-is-changed-towards-30-)has listed the spec of keyword arguments for Ruby 3.0. We will take the examples mentioned there and for each scenario we will look into how we can fix them in the existing codebase.

#### Scenario 1

*When method definition accepts keyword arguments as the last argument.*

```ruby

```

Passing exact keyword arguments in a method call is acceptable, but in applications we usually pass a hash to a method call.

```ruby

```

In this case, we can add a double splat operator to the hash to avoid deprecation warning.

```ruby

```

#### Scenario 2

*When method call passes keyword arguments but does not pass enough required positional arguments.*

If the number of positional arguments doesn’t match with method definition, then keyword arguments passed in method call will be considered as the last positional argument to the method.

```ruby

```

```ruby

```

To avoid deprecation warning and for code to be compatible with Ruby 3, we should pass hash instead of keyword arguments in method call.

```ruby

```

#### Scenario 3

*When a method accepts a hash and keyword arguments but method call passes only hash or keyword arguments.*

If a method arguments are a mix of symbol keys and non-symbol keys, and the method definition accepts either one of them then Ruby splits the keyword arguments but also raises a warning.

```ruby

```

```ruby

```

To fix this warning, we should pass hash separately as defined in the method definition.

```ruby

```

#### Scenario 4

*When an empty hash with double splat operator is passed to a method that doesn’t accept keyword arguments.*

Passing keyword arguments using double splat operator to a method that doesn’t accept keyword argument will send empty hash similar to earlier version of Ruby but will raise a warning.

```ruby

```

```ruby

```

To avoid this warning, we should change method call to pass hash instead of using double splat operator.

```ruby

```

- - -

### Added support for non-symbol keys

In Ruby 2.6.0, support for non-symbol keys in method call was removed. It is added back in Ruby 2.7. When method accepts arbitrary keyword arguments using double splat operator then non-symbol keys can also be passed.

```ruby

```

##### ruby 2.6.5

```ruby

```

##### ruby 2.7.0

```ruby

```

- - -

### Added support for`**nil`

Ruby 2.7 added support for`**nil`to explicitly mention if a method doesn’t accept any keyword arguments in method call.

```ruby

```

- - -

To suppress above deprecation warnings we can use[`-W:no-deprecated option`](https://github.com/ruby/ruby/blob/4643bf5d55af6f79266dd67b69bb6eb4ff82029a/doc/NEWS-2.7.0#warning-option-).

In conclusion, Ruby 2.7 has worked big steps towards changing specification of keyword arguments which will be completely changed in Ruby 3.

For more information on discussion, code changes and official documentation, please head to[Feature #14183](https://bugs.ruby-lang.org/issues/14183)discussion,[pull request](https://github.com/ruby/ruby/pull/2395)and[NEWS release](https://github.com/ruby/ruby/blob/4643bf5d55af6f79266dd67b69bb6eb4ff82029a/doc/NEWS-2.7.0#the-spec-of-keyword-arguments-is-changed-towards-30-).