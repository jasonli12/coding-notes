# RSpec

**RSpec**: a TDD, specifically Behavior Driven Development, testing framework for Ruby

The Entry Model
1. Create a directory for models
2. Create a directory for spec (RSpec's name for tests)

3. Create a file entry.rb in the models directory
4. Create a file entry_spec.rb in the spec directory. All tests will go in the spec directory.

spec/entry_spec.rb (basic test file structure)

```ruby

require_relative '../models/entry'

RSpec.describe Entry do

end

```
[`require_relative`](https://ruby-doc.org/core-2.3.1/Kernel.html#method-i-require_relative): loads our entry model for testing using a relative path, relative to the file containing the require_relative statement

Running `rspec` in command line: `$ rspec spec/entry_spec.rb`

```
spec/entry_spec.rb:3:in `<top (required)>': uninitialized constant Entry (NameError)
```
`NameError`: spec failed when executed. In the example above, we reference `Entry` in the spec through `RSpec.describe Entry do...` but `Entry` is not defined anywhere else.


```
No examples found.

Finished in 0.00022 seconds (files took 0.08404 seconds to load)
0 examples, 0 failures
```
`examples` refers to tests. No `examples` means no tests have been added to the `spec` file.

```ruby
require_relative '../models/entry'

RSpec.describe Entry do # The file is a spec file and tests for Entry

  describe "attributes" do # We are testing the Entry attributes
    let (:entry) { Entry.new('Ada Lovelace', '010.012.1815', 'augusta.king@lovelace.com') }

    it "responds to name" do # Each it represents a unique test
      # entry = Entry.new Instead of defining entry in each test, we can just define it once using helper methods like let.
      expect(entry).to respond_to(:name) # Each RSpec test ends with one or more expect method(s). If those expectations are met, the test passes. Otherwise, it fails.
    end
  end
end
```
