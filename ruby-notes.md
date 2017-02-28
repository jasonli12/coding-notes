# Ruby Notes

**IRB (Interative Ruby Shell)**: allows you to run Ruby commands in the command line

+ `irb` : start an IRB session
+ `exit` or `quit`: close an IRB session
+ `control + L`: clear the command line screen


Tips: run `.method` on any ruby object in IRB to obtain a full list of valid methods. You can also chain this with sort, `.method.sort`, to get a sorted list of methods.

`ruby <file_name>`: execute the code in the Ruby file directly without needing to compile it first

**Command-line Arguments**: arguments that we pass to our programs through the command line when we run them

`$ ruby hello_world.rb arg1 arg2 arg3`

+ Command-line variables are stored in an array called ARGV.

```ruby
def hello_world
  ARGV.each do |arg|
    puts "Hello, #{arg}!"
  end
end

hello_world
```

`$ ruby hello_world.rb Jason Kevin George`

Output:

Hello, Jason!

Hello, Kevin!

Hello, George!
