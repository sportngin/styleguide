---
layout: page
title: Ruby
header: Ruby Style Guide
group: navigation
---
{% include JB/setup %}

Much of this was taken from https://github.com/bbatsov/ruby-style-guide and https://github.com/styleguide/ruby.

## Table of Contents

* [Source Code Layout](#source-code-layout)
* [Syntax](#syntax)
* [Naming](#naming)
* [Comments](#comments)
* [Comment Annotations](#comment-annotations)
* [Classes](#classes--modules)
* [Exceptions](#exceptions)
* [Collections](#collections)
* [Strings](#strings)
* [Regular Expressions](#regular-expressions)
* [Percent Literals](#percent-literals)
* [Misc](#misc)
* [Never Ever](#never-ever)

## Source Code Layout


* Use `UTF-8` as the source file encoding.

* Use two **spaces** per indentation level. No hard tabs.

    ```ruby
    # bad - four spaces
    def some_method
        do_something
    end

    # good
    def some_method
      do_something
    end
    ```

* Avoid single-line methods.

    ```ruby
    # bad
    def too_much; something; something_else; end

    # okish - notice that the first ; is required
    def no_braces_method; body end

    # okish - notice that the second ; is optional
    def no_braces_method; body; end

    # okish - valid syntax, but no ; make it kind of hard to read
    def some_method() body end

    # good
    def some_method
      body
    end
    ```

* One exception to the rule are empty-body methods.

    ```ruby
    # good
    def no_op; end
    ```

* Use spaces around operators, after commas, colons and semicolons, around `{`
  and before `}`. Whitespace might be (mostly) irrelevant to the Ruby
  interpreter, but its proper use is the key to writing easily
  readable code.

    ```ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

* The only exceptions, are the exponent operator and string interpolation:

* The exponent operator should have no spaces around it.

    ```ruby
    # bad
    e = M * c ** 2

    # good
    e = M * c**2
    ```

* String interpolation should have no spaces after `#{` or before `}`.

    ```ruby
    # bad
    "string#{ expr }"

    # good - no spaces
    "string#{expr}"
    ```

* No spaces after `(`, `[` or before `]`, `)`.

    ```ruby
    some(arg).other
    [1, 2, 3].length
    ```

* Indent `when` as deep as `case`.

    ```ruby
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end
    ```

* Use empty lines between `def`s and to break up a method into logical
  paragraphs.

    ```ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_method
      result
    end
    ```

* Don't use spaces around the `=` operator when assigning default values to method parameters:

    ```ruby
    # bad
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end

    # good
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end
    ```


* Avoid line continuation `\` where not required. In practice, avoid using
  line continuations at all.

    ```ruby
    # bad
    result = 1 - \
             2

    # good (but still ugly as hell)
    result = 1 \
             - 2
    ```


* When continuing a chained method invocation on another line keep the `.` on the second line.

    ```ruby
    # bad - need to consult first line to understand second line
    one.two.three.
      four

    # good - it's immediately clear what's going on the second line
    one.two.three
      .four
    ```


* Align the parameters of a method call if they span more than one line.

    ```ruby
    # starting point (line is too long)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # bad (normal indent)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end

    # bad (double indent)
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # good
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end
    ```

* Add underscores to large numeric literals to improve their readability.

    ```ruby
    # bad - how many 0s are there?
    num = 1000000

    # good - much easier to parse for the human brain
    num = 1_000_000
    ```

* Limit lines to 120 characters.
* Remove all trailing whitespace.

* Don't use block comments. They cannot be preceded by whitespace and are not as easy to spot as regular comments.

    ```ruby
    # bad
    == begin
    comment line
    another comment line
    == end

    # good
    # comment line
    # another comment line
    ```

* Leave an empty line at the end of files.

## Syntax

* Use `::` only to reference constants(this includes classes and
modules). Never use `::` for method invocation.

    ```ruby
    # bad
    SomeClass::some_method
    some_object::some_method

    # good
    SomeClass.some_method
    some_object.some_method
    SomeModule::SomeClass::SOME_CONST
    ```

* Use `def` with parentheses when there are arguments. Omit the
  parentheses when the method doesn't accept any arguments.

    ```ruby
    # bad
    def some_method()
     # body omitted
    end

    # good
    def some_method
     # body omitted
    end

    # bad
    def some_method_with_arguments arg1, arg2
     # body omitted
    end

    # good
    def some_method_with_arguments(arg1, arg2)
     # body omitted
    end
    ```

* Never use `then` for multi-line `if/unless`.

    ```ruby
    # bad
    if some_condition then
      # body omitted
    end

    # good
    if some_condition
      # body omitted
    end
    ```

* Favor the ternary operator(`?:`) over `if/then/else/end` constructs

    ```ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* Use one expression per branch in a ternary operator. This
  also means that ternary operators must not be nested. Prefer
  `if/else` constructs in these cases.

    ```ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* Use `!` instead of `not`.

    ```ruby
    # bad - braces are required because of op precedence
    x = (not something)

    # good
    x = !something
    ```


* Always use `&&` and `||` instead of `and` or `or` for logical
  expressions. Use `and` and `or` for control flow.

    ```ruby
    # bad
    # boolean expression
    if some_condition and some_other_condition
      do_something
    end

    # control flow
    document.saved? || document.save!

    # good
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.saved? or document.save!

    document.save and document.shipit
    ```

* Do not use multi-line `?:` (the ternary operator); use `if/unless` instead.

* Favor modifier `if/unless/while/until` usage when you have a single-line
  body. Another good alternative is the usage of control flow `and/or`

    ```ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition and do_something

    # bad
    while some_condition
      do_something
    end

    # good
    do_something while some_condition
    ```

* Favor `unless` over `if` for negative conditions (or control
  flow `or`) Also Favor `until` over `while` for negative conditions.

    ```ruby
    # bad
    do_something if !some_condition

    # bad
    do_something if not some_condition

    # good
    do_something unless some_condition

    # another good option
    some_condition or do_something

    # bad
    do_something while !some_condition

    # good
    do_something until some_condition
    ```

* Never use `unless` with `else`. Rewrite these with the positive case first.

    ```ruby
    # bad
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # good
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

* Don't use parentheses around the condition of an `if/unless/while`,
  unless the condition contains an assignment.

    ```ruby
    # bad
    if (x > 10)
      # body omitted
    end

    # good
    if x > 10
      # body omitted
    end

    # ok
    if (x = self.next_value)
      # body omitted
    end
    ```

* Don't use the return value of `=` (an assignment) in conditional expressions.

    ```ruby
    # bad (+ a warning)
    if (v = array.grep(/foo/))
      do_something(v)
      ...
    end

    # bad (+ a warning)
    if v = array.grep(/foo/)
      do_something(v)
      ...
    end

    # good
    v = array.grep(/foo/)
    if v
      do_something(v)
      ...
    end
    ```



* Use Kernel#loop with break rather than `begin/end/until` or `begin/end/while` for post-loop tests.

    ```ruby
    # bad
    begin
     puts val
     val += 1
    end while val < 0

    # good
    loop do
     puts val
     val += 1
     break unless val < 0
    end
    ```


* Omit parentheses around parameters for methods that are part of an
  internal DSL (e.g. Rake, Rails, RSpec), methods that have
  "keyword" status in Ruby (e.g. `attr_reader`, `puts`) and attribute
  access methods. Use parentheses around the arguments of all other
  method invocations.

    ```ruby
    class Person
      attr_reader :name, :age

      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)

    bowling.score.should == 0
    ```

* Omit parentheses for method calls with no arguments.

    ```ruby
    # bad
    Kernel.exit!()
    2.even?()
    fork()
    'test'.upcase()

    # good
    Kernel.exit!
    2.even?
    fork
    'test'.upcase
    ```


* Prefer `{...}` over `do...end` for single-line blocks.  Avoid using
  `{...}` for multi-line blocks (multiline chaining is always
  ugly). Always use `do...end` for "control flow" and "method
  definitions" (e.g. in Rakefiles and certain DSLs).  Avoid `do...end`
  when chaining.

    ```ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # bad
    names.each do |name|
      puts name
    end

    # good
    names.each { |name| puts name }

    # bad
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # good
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }
    ```

* Avoid `return` where not required for flow of control.

    ```ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```


* Avoid `self` where not required. (It is only required when calling a self write accessor)

    ```ruby
    # bad
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # good
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```

* Avoid shadowing methods with local variables unless they are both equivalent.

    ```ruby
    class Foo
      attr_accessor :options

      # ok
      def initialize(options)
        self.options = options
        # both options and self.options are equivalent here
      end

      # bad
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # good
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```

* Use `||=` freely to initialize variables.

    ```ruby
    # set name to Bozhidar, only if it's nil or false
    name ||= 'Bozhidar'
    ```

* Don't use `||=` to initialize boolean variables. (Consider what
  would happen if the current value happened to be `false`.)

    ```ruby
    # bad - would set enabled to true even if it was false
    enabled ||= true

    # good
    enabled = true if enabled.nil?
    ```

* Prefer `proc` over `Proc.new`.

    ```ruby
    # bad
    p = Proc.new { |n| puts n }

    # good
    p = proc { |n| puts n }
    ```

* Use `_` for unused block parameters.

    ```ruby
    # bad
    result = hash.map { |k, v| v + 1 }

    # good
    result = hash.map { |_, v| v + 1 }
    ```

* Use `[*var]` or `Array()` instead of explicit `Array` check, when dealing with a
  variable you want to treat as an Array, but you're not certain it's
  an array.

    ```ruby
    # bad
    paths = [paths] unless paths.is_a? Array
    paths.each { |path| do_something(path) }

    # good
    [*paths].each { |path| do_something(path) }

    # good (and a bit more readable)
    Array(paths).each { |path| do_something(path) }
    ```

* Use ranges instead of complex comparison logic when possible.

    ```ruby
    # bad
    do_something if x >= 1000 && x < 2000

    # good
    do_something if (1000...2000).include?(x)
    ```

* Prefer the `->` operator over `lambda` for creating Procs

    ```ruby
    # bad
    be_awesome = lambda {|word| puts "#{word} is awesome!" }

    # good
    be_awesome = ->(word) { puts "#{word} is awesome!" }

* Never put a space between `->` and the leading parenthesis for it's parameters

    ```ruby
    # bad
    print_something = -> (something) { puts something }

    # good
    print_something = ->(something) { puts something }

## Naming

* Name identifiers in English.

* Use `snake_case` for symbols, methods and variables.

    ```ruby
    # bad
    :'some symbol'
    :SomeSymbol
    :someSymbol

    someVar = 5

    def someMethod
      ...
    end

    def SomeMethod
     ...
    end

    # good
    :some_symbol

    def some_method
      ...
    end
    ```

* Use `CamelCase` for classes and modules.  (Keep acronyms like HTTP,
  RFC, XML uppercase.)

    ```ruby
    # bad
    class Someclass
      ...
    end

    class Some_Class
      ...
    end

    class SomeXml
      ...
    end

    # good
    class SomeClass
      ...
    end

    class SomeXML
      ...
    end
    ```


* Use `SCREAMING_SNAKE_CASE` for other constants.

    ```ruby
    # bad
    SomeConst = 5

    # good
    SOME_CONST = 5
    ```


* The names of predicate methods (methods that return a boolean value)
  should end in a question mark.
  (i.e. `Array#empty?`).

* The names of potentially *dangerous* methods (i.e. methods that
  modify `self` or the arguments, `exit!` (doesn't run the finalizers
  like `exit` does), etc.) should end with an exclamation mark if
  there exists a safe version of that *dangerous* method.

    ```ruby
    # bad - there is not matching 'safe' method
    class Person
      def update!
      end
    end

    # good
    class Person
      def update
      end
    end

    # good
    class Person
      def update!
      end

      def update
      end
    end
    ```


* When using `reduce` with short blocks, name the arguments `|a, e|`
  (accumulator, element), or the singular form of the mapped variable.

* When using `map`, name the argument `|e|` (element),
or the singular form of the mapped variable.

* When defining binary operators, name the argument `other`(`<<` and
  `[]` are exceptions to the rule, since their semantics are different).

    ```ruby
    def +(other)
      # body omitted
    end
    ```

* Prefer `map` over `collect`, `reduce` over `inject`, `detect` over `find`, `select` over
  `find_all`, and `size` over `length`.

* Use `flat_map` instead of `map` + `flatten`.
  This does not apply for arrays with a depth greater than 2, i.e.
  if `users.first.songs == ['a', ['b','c']]`, then use `map + flatten` rather than `flat_map`.
  `flat_map` flattens the array by 1, whereas `flatten` flattens it all the way.

    ```ruby
    # bad
    all_songs = users.map(&:songs).flatten.uniq

    # good
    all_songs = users.flat_map(&:songs).uniq
    ```

## Comments

* Write comments sparingly, make code self-documenting whenever possible.
* Write comments in English.
* Use one space between the leading `#` character of the comment and the text
  of the comment.
* Comments longer than a word are capitalized and use punctuation. Use [one
  space](http://en.wikipedia.org/wiki/Sentence_spacing) after periods.
* Avoid superfluous comments.

    ```ruby
    # bad
    counter += 1 # increments counter by one
    ```

* Keep existing comments up-to-date. An outdated comment is worse than no comment
at all.

* Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory.

### Comment Annotations

* Annotations should usually be written on the line immediately above
  the relevant code.
* The annotation keyword inis followed by a colon and a space, then a note
  describing the problem.
* If multiple lines are required to describe the problem, subsequent
  lines should be indented two spaces after the `#`.

    ```ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #   be related to the BarBazUtil upgrade.
      baz(:quux)
    end
    ```

* In cases where the problem is so obvious that any documentation would
  be redundant, annotations may be left at the end of the offending line
  with no note. This usage should be the exception and not the rule.

    ```ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* Use `TODO` to note missing features or functionality that should be
  added at a later date.
* Use `FIXME` to note broken code that needs to be fixed.
* Use `OPTIMIZE` to note slow or inefficient code that may cause
  performance problems.
* Use `HACK` to note code smells where questionable coding practices
  were used and should be refactored away.
* Use `REVIEW` to note anything that should be looked at to confirm it
  is working as intended. For example: `REVIEW: Are we sure this is how the
  client does X currently?`
* Use other custom annotation keywords if it feels appropriate, but be
  sure to document them in your project's `README` or similar.

## Classes & Modules

* Use a consistent structure in your class definitions.

    ```ruby
    class Person
      # extend and include go first
      extend SomeModule
      include AnotherModule

      # constants are next
      SOME_CONSTANT = 20

      # afterwards we have attribute macros
      attr_reader :name

      # followed by other macros (if any)
      validates :name

      # public class methods are next in line
      def self.some_method
      end

      # followed by public instance methods
      def some_method
      end

      # protected and private methods are grouped near the end


      def some_protected_method
      end
      protected :some_protected_method

      def some_private_method
      end
      private :some_private_method
    end
    ```

* Prefer modules to classes with only class methods. Classes should be
  used only when it makes sense to create instances out of them.

    ```ruby
    # bad
    class SomeClass
      def self.some_method
        # body omitted
      end

      def self.some_other_method
      end
    end

    # good
    module SomeClass
      module_function

      def some_method
        # body omitted
      end

      def some_other_method
      end
    end
    ```

* Favor the use of `module_function` over `extend self` when you want
  to turn a module's instance methods into class methods.

    ```ruby
    # bad
    module Utilities
      extend self

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end

    # good
    module Utilities
      module_function

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end
    ```


* Use the `attr` family of functions to define trivial accessors or
mutators.

    ```ruby
    # bad
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* Avoid the usage of class (`@@`) variables due to their "nasty" behavior
in inheritance.

    ```ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

* Assign proper visibility levels to methods (`private`, `protected`)
in accordance with their intended usage. Don't go off leaving
everything `public` (which is the default).

* Pass the method name to `public`, `protected`, and `private` methods one line below the visibility controlled method

    ```ruby
    #bad
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end

    #good
    class SomeClass
      def public_method
        # ...
      end

      def private_method
        # ...
      end
      private :private_method

      def another_private_method
        # ...
      end
      private :private_method
    end
    ```

* Avoid class << self except when necessary, e.g. single accessors and aliased attributes.

    ```ruby
    class TestClass
      # bad
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end

      # good
      class << self
        attr_accessor :per_page
        alias_method :nwo, :find_by_name_with_owner
      end

      def self.first_method
        # body omitted
      end

      def self.second_method_etc
        # body omitted
      end
    end
    ```

## Exceptions

* Never return from an `ensure` block. If you explicitly return from a
  method inside an `ensure` block, the return will take precedence over
  any exception being raised, and the method will return as if no
  exception had been raised at all. In effect, the exception will be
  silently thrown away.

    ```ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* Use *implicit begin blocks* where possible.

    ```ruby
    # bad
    def foo
    begin
      # main logic goes here
    rescue
      # failure handling goes here
    end
    end

    # good
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

* Mitigate the proliferation of `begin` blocks by using
  *contingency methods* (a term coined by Avdi Grimm).

    ```ruby
    # bad
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # good
    def with_io_error_handling
       yield
    rescue IOError
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* Don't suppress exceptions.

    ```ruby
    # bad
    begin
      # an exception occurs here
    rescue SomeError
      # the rescue clause does absolutely nothing
    end

    # bad
    do_something rescue nil
    ```

* Avoid using `rescue` in its modifier form.

    ```ruby
    # bad - this catches all StandardError exceptions
    do_something rescue nil
    ```

* Don't use exceptions for flow of control.

    ```ruby
    # bad
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # good
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```


* Avoid rescuing the `Exception` class.  This will trap signals and calls to
  `exit`, requiring you to `kill -9` the process.

    ```ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # good
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # also good
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```

* Favor the use of exceptions for the standard library over
introducing new exception classes.

* Favor error classes over exception classes.

* Favor inheriting from `StandardError` over `Exception` when defining new errors/exceptions.

* Basic errors should be defined on a single line using `Class.new`.

    ```ruby
    #bad
    class AwesomeError < StandardError; end

    #good
    AwesomeError = Class.new(StandardError)
    ```

## Collections

* Prefer literal array and hash creation notation (unless you need to
pass parameters to their constructors, that is).

    ```ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    hash = {}
    ```

* Prefer `%w` to the literal array syntax when you need an array of
words(non-empty strings without spaces and special characters in them).
Apply this rule only to arrays with two or more elements.

    ```ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w(draft open closed)
    ```

* Avoid the creation of huge gaps in arrays.

    ```ruby
    arr = []
    arr[100] = 1 # now you have an array with lots of nils
    ```

* When accessing the first or last element from an array, prefer `first` or `last` over `[0]` or `[-1]`.

* Prefer symbols instead of strings as hash keys.

    ```ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* Avoid the use of mutable objects as hash keys.

* Use the hash literal syntax when your hash keys are symbols.

    ```ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```


* Use `fetch` when dealing with hash keys that should be present.

    ```ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # bad - if we make a mistake we might not spot it right away
    heroes[:batman] # => "Bruce Wayne"
    heroes[:supermann] # => nil

    # good - fetch raises a KeyError making the problem obvious
    heroes.fetch(:supermann)
    ```

* Use `fetch` with second argument to use a default value

    ```ruby
    batman = { name: 'Bruce Wayne', is_evil: false }

    # bad - if we just use || operator with falsy value we won't get the expected result
    batman[:is_evil] || true # => true

    # good - fetch work correctly with falsy values
    batman.fetch(:is_evil, true) # => false
    ```

* Rely on the fact that as of Ruby 1.9 hashes are ordered.

## Strings

* Prefer string interpolation instead of string concatenation:

    ```ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Prefer double-quoted strings. Interpolation and escaped characters will always work without a delimiter change, and ' is a lot more common than " in string literals.

    ```ruby
    # bad
    name = 'Bozhidar'

    # good
    name = "Bozhidar"
    ```

* Don't leave out `{}` around instance and global variables being
  interpolated into a string.

    ```ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # bad - valid, but awkward
      def to_s
        "#@first_name #@last_name"
      end

      # good
      def to_s
        "#{@first_name} #{@last_name}"
      end
    end

    $global = 0
    # bad
    puts "$global = #$global"

    # good
    puts "$global = #{$global}"
    ```

* Don't pass herdoc strings to methods. Use a temporary variable.

    ```ruby
    # bad
    execute(<<-QUERY)
      UPDATE users SET username = "Bob" WHERE id=1;
      UPDATE users SET username = "Billy" WHERE id=2;
    QUERY

    # good
    query = <<-QUERY
      UPDATE users SET username = "Bob" WHERE id=1;
      UPDATE users SET username = "Billy" WHERE id=2;
    QUERY
    execute(query)
  ```


## Regular Expressions


* Don't use regular expressions if you just need plain text search in string:
  `string['text']`


* Use non-capturing groups when you don't use captured result of parentheses.

    ```ruby
    /(first|second)/   # bad
    /(?:first|second)/ # good
    ```

* Avoid using $1-9 as it can be hard to track what they contain. Named groups
  can be used instead.

    ```ruby
    # bad
    /(regexp)/ =~ string
    ...
    process $1

    # good
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

## Percent Literals

* Use `%()`(it's a shorthand for `%Q`) for single-line strings which require both interpolation
  and embedded double-quotes. For multi-line strings, prefer heredocs.

    ```ruby
    # bad (no interpolation needed)
    %(<div class="text">Some text</div>)
    # should be '<div class="text">Some text</div>'

    # bad (no double-quotes)
    %(This is #{quality} style)
    # should be "This is #{quality} style"

    # bad (multiple lines)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # should be a heredoc.

    # good (requires interpolation, has quotes, single line)
    %(<tr><td class="name">#{name}</td>)
    ```

## Misc

* Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will be shorter than
  5 LOC. Empty lines do not contribute to the relevant LOC.
* Avoid parameter lists longer than three or four parameters.
* If you really need "global" methods, add them to Kernel
  and make them private.
* Use module instance variables instead of global variables.

    ```ruby
    # bad
    $foo_bar = 1

    #good
    module Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* Avoid `alias` when `alias_method` will do.
* Use `OptionParser` for parsing complex command line options and
`ruby -s` for trivial command line options.
* Code in a functional way, avoiding mutation when that makes sense.
* Do not mutate arguments unless that is the purpose of the method.
* Avoid more than three levels of block nesting.
* Be consistent. In an ideal world, be consistent with these guidelines.
* Use common sense.


## Never Ever

* Never use `if x: ...` - as of Ruby 1.9 it has been removed. Use the ternary operator instead.

* Never use `if x; ...`. Use the ternary operator instead.

* Use `when x then ...` for one-line cases. The alternative syntax
  `when x: ...` has been removed as of Ruby 1.9.

* Never use `when x; ...`. See the previous rule.

* Don't use `;` to separate statements and expressions. As a
  corollary - use one expression per line. In general, don't use semicolons.

    ```ruby
    # bad
    puts 'foobar'; # superfluous semicolon

    puts 'foo'; puts 'bar' # two expression on the same line

    # good
    puts 'foobar'

    puts 'foo'
    puts 'bar'

    puts 'foo', 'bar' # this applies to puts in particular
    ```

* Never use `for`

* Avoid using Perl-style special variables (like `$0-9`, `$`, etc. )

* Never put a space between a method name and the opening parenthesis.

    ```ruby
    # bad
    f (3 + 2) + 1

    # good
    f(3 + 2) + 1
    ```

* Use `sprintf` instead of `String#%` method.

    ```ruby
    # bad
    '%d %d' % [20, 10]
    # => '20 10'

    # good
    sprintf('%d %d', 20, 10)
    # => '20 10'
    ```

* Use `Array#join` instead of `Array#*` with
  a string argument.

    ```ruby
    # bad
    %w(one two three) * ', '
    # => 'one, two, three'

    # good
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```
