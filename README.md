# beginning-rspec
Free sample for the book https://www.gitbook.com/book/checkraiser/beginning-rspec/details

# Working with RSpec matchers

RSpec matchers can be combined with either should or should_not as a part of validation.
We'll step back from the sample application for this section and review each matcher class and how to use it.

1. The various ways to determine equality and inequality are:
```
1 describe "Equal" do
2 let(:address) { "123 Main Street" }
3 subject { "123 Main Street" }
4 it { should eq '123 Main Street' }
5 it { should == "123 Main Street" }
6 it { should_not == "789 Any Circle" }
7 it { should_not be(address) } # object equality
8 it { should_not equal(address) } # object equality too
9 it { should eql(address) }
10 it { should == address }
11 end
```
2. Comparisons allow us to verify greater than and less than conditions:
```
1 describe "Comparisons" do
2 subject { 42 }
3 it { should be > 41 }
4 it { should be >= 42 }
5 it { should be <= 42 }
6 it { should be < 43 }
7 end
```
3. In RSpec, there is no restriction specifying that only numbers may be compared; many other types are candidates for comparison. You would find yourself comparing floating point numbers or checking whether a value is within an acceptable threshold:
```
1 describe "Floating Comparison" do
2 subject { 3.141_592_653_5 }
3 it { should be_within(0.000_2).of(3.141_590) }
4 end
```
4. Regular expression comparisons are a convenient and powerful way of validating portions of text, and are especially noteworthy for their use in validating Rails view specs:
```
1 describe "Regular Expression Comparison" do
2 subject { "this is a block of text" }
3 it { should match(/text$/) }
4 it { should =~ /\bblock\b/ }
5 end
```
5. Boolean tests determine the truthiness of a variable or statement:
```
1 describe "Boolean" do
2 subject { "non-nil is true" }
3 it { should be_true }
4 it { should_not be_false }
5 end
```
6. RSpec performs some magic by dynamically creating matchers for any methods on a class that either begin with the word has or end with a question mark. These dynamically created methods are named have_method_name or be_method_name respectively and are called predicates:
```
1 describe "Predicate" do
2 subject { { :a => 1, :b => 2 } }
3 it { should have_key(:a) } # has_key?(:a)
4 it { should_not be_empty } # empty?
5 end
```
7. Determining whether a given value is contained by a collection is done with the include matcher. Remember that a string is a collection of characters, so include may be used with substrings, shown as follows:
```
1 describe "Collections" do
2 subject { ["text one", "text two"] }
3 it { should include "text two" }
4 its (:first) { should include "ext" }
5 end
```
8. Testing for a particular class or superclass has limited applicability in Ruby, but it can be done as shown in the following code:
```
1 describe "Class" do
2 subject { 42 }
3 it { should be_instance_of Fixnum }
4 it { should be_kind_of Integer } # Fixnum > Integer
5 end
```
9. Because Ruby doesn't have interfaces or abstract classes, it can become important to verify that a given class adheres to a specific contract:
```
1 describe "Contract Validation" do
2 subject { Resource.new }
3 it { should respond_to :near? }
4 end
```
10. Unlike throw in other languages, Ruby's throw and catch are used as control structures and may have an associated symbol and optional payload.
```
1 describe "Throws" do
2 subject { Proc.new { throw :some_symbol, "x" } }
3 it "should throw some_symbol" do
4 expect { subject.call }.to throw_symbol
5 expect { subject.call }.to throw_symbol(:some_symbol)
6 expect { subject.call }.to throw_symbol(:some_symbol, "x")
7 end
8 end
```
11. Raising errors is similar to the throw statements, but are used for error situations and not for the control flow.
```
1 describe "Errors" do
2 subject { Proc.new { raise RuntimeError.new("x") } }
3 it "should raise an exception" do
4 expect { subject.call }.to raise_error
5 expect { subject.call }.to raise_error(RuntimeError)
6 expect { subject.call }.to raise_error(RuntimeError, 'x')
7 expect { subject.call }.to raise_error('x')
8 end
9 end
```
