# Ruby Exception Handling

**Exception**: a special kind of Ruby object that represents some kind of exceptional or error condition. It indicates that something has gone wrong during the execution of the program (i.e. raising/throwing an exception).

Default Ruby behavior: terminate when an exception occurs

There are cases where we suspect an exception may occur, but we do not want our program to terminate, so we use exception handling.


``` ruby
Example:

def raise_exception
  puts 'Before exception.'
  raise 'An exception has occurred!'
  puts 'After exception.'
end

raise_exception

Output:
'Before exception.'
'An exception has occurred!'

```
Note that `puts 'After exception.` is never reached before Ruby terminates after the exception has been thrown.

**Implementing Exception Handling**

Enclose the code that could raise an exception with a `begin` and `end` block and use one or more `rescue` clauses to tell Ruby the type of exceptions to handle and what to do.

```ruby
Exmaple:

def rescue_rangers
  begin
    puts 'Before exception.'
    raise 'An exception has occurred!'
    puts 'After exception.'
  rescue
    puts 'Phew, rescued!'
  end
  puts 'After the begin block.'
  end
end

rescue_rangers

Output:
'Before exception.'
'Phew, rescued!'
'After the begin block.'

```

When an exception is raised, Ruby looks into the `rescue` clause to see how to handle the exception and continue to run the program. Once the code in the rescue clause is complete, the program continues to run after the `begin` and `end` block. Note that the code after the `raise` clause is not executed because the exception has been thrown.
