# Hi! These are my Rspec Notes
Hi there! I am trying to learn how to write rspec (more specifically rspec for rails). I am trying to write some notes as I learn.

# Useful Links
[relish rspec docs](https://relishapp.com/rspec)

[rspec-rails github](https://github.com/rspec/rspec-rails)

[relish rspec-rails docs](https://relishapp.com/rspec/rspec-rails/v/3-7/docs/transactions)

[lynda rspec tutorial by Kevin Skoglund](https://www.lynda.com/Ruby-tutorials/Transactional-examples/183884/371481-4.html?srchtrk=index%3a1%0alinktypeid%3a2%0aq%3arspec%0apage%3a1%0as%3arelevance%0asa%3atrue%0aproducttypeid%3a2)

# RSpec Configuration
## Examples of Configuration Options

| Default Option   | Alternative Option           | Meaning  |
| ------------- |:-------------:| -----:|
|--no-color|  --color | Output single vs. multicolour|
|--format progress|  --format | Format progress - shows dots each time test is run, Format documentation - shows tests that have been run|
|--no-profile|  --profile | Profile keeps track of fastest & slowest test|
|--no-fail-fast|  --fail-fast | Run test & report all failurs at end vs. report failures as soon as it is encountered (& stops)|
| --order defined | --order random | Order tests are run|

## Configuration Options

Method 1: Type in options into command line when running rspec

`rspec --option` (e.g. `rspec --color`)

Method 2: Configuration files

- Global: ~/.rspec
- Project: ./.rspec 
    - typically checked into git 
- Local: ./.rspec-local (introduced in rspec v3)
- Comnand line

There is a hierarchy. A local configuration .rspec file would over-write a  project .rspec file. Command line > Local > Project > Global (`>` denotes trump)

Example of a .rspec file

```
--color
--format progress
--no-profile
--no-fail-fast
--order defined

```

# Set up rspec
(assuming you have installed rspec gem)

In your the directory ruby (non-rails) project. In the command line enter
`rspec init`

This creates
1. .rspec (project .rspec configuration file)
2. /spec/spec_helper.rb

Managing your specs
- Keep your other code in `/lib`
- Keep your test specs in `/spec`
- You would typically name your spec file after the file you test.
    - E.g. for `/lib/user.rb` you would create `/spec/user_spec.rb`


# Writing Specs - Basic Format


- Example Group `describe`
    - Nested Group `describe / context`
        - Example `it`
            - Expectations expect().to()


### Describe
- Takes an argument (either a string or class name)
- Allows us to know which test rspec is referring to
- Has a ruby block that comes after it from `do` to `end` 
- Everything inside the ruby block is an example group, prefixed by `it`

### It (example group)
- Accepts a string
    - Usually a simple sentence which can be made sense of
- Has a ruby block

### Nested Groups
- Describe
    - You can nest a describe blocks within a describe block
    - E.g. for each method within a class
        - such that `it` within the nested block refers to the method.
        - For a class method prefix with `.`
            - e.g. `describe .method`
        - For an instance method prefix with `#`
            - e.g. `describe #method`
- Context
    - Provide a context


### Expectations
- Provide arguments to 
    - 1) expect(): 
    - 2) to() or not_to
    
    `expect().to()`

    `expect().not_to()`

### Pending & Skipping Examples
- Can be used if you have not written the code yet

#### Skipping
- `xit` instead of `it`
- `xdescribe` instead of `describe`
- include `skip` within block can take a string as an argument why it was skipped.
- `skip("message)`

#### Pending
- Omit the block (do not include the `do` and `end`)
- include `pending` within block 
    - still runs code within the block though as opposed to skip!
    - Use if only if code is expected to fail


# Running rspec test
Make sure `spec_helper` has been required

1. Either in .rspec (new way)
```
--require rspec_helper
```

3. Old school way in (/name_spec.rb)

```
# /name_spec.rb require spec_helper
```

In command line:
`rspec spec/name_spec.rb`

You can run a test on a specific line
rspec spec/name_spec.rb:linenumber e.g. `rspec spec/name_spec.rb:7` (test would be on line 7 in the spec file)

Or
`rspec spec` runs all spec/*_spec.rb


# Expectations
- 1 expectation per example
    - Otherwise, with more you will only see the first failure.

e.g.
```
actual_variable = "expected_name"

expect(actual_variable).to(eq("expected_name"))
````

But Rspec is usually written without parentheses after to

`expect(actual_variable).to eq("expected_name") `


General syntax

`expect(actual).to some-form-of-match(expected)`

## Examples of matchers (some-form-of-match)
1. Equivalence matchers
    - `eq()` - loose equality (we will usually use this)
    - `eql()`  - value quality
    - `equal()` -  identity equality
    - `be()` -  identity equality
2. Truthiness matchers
    - `be(true)`
    - `be(false)`
    - `be_truthy` - (e.g. non-nil is truty)
    - `be_falsey` - (e.g. nil is falsey. 0 is NOT falsey in ruby)
    - `be_nil`
3. Numeric-Comparison Matchers
    - be + operator
        - `be == 100`
        - `be > 100`
        - `be < 100`
        - `be <= 100`
        - `be >= 100`
    - be_between + inclusive/exclusive 
        - `be_between(1, 5).inclusive ` - 1-5
        - `be_between(1, 5).exclusive`  - 2-4
    - be_within + of
        - `be_within(5).of(100)` - 95-105
    - cover
        - cover(100) - actual includes 100
4. Collection Matchers
    -`include(1,2)`
    -`start_with(1)`
    -`end_with(2)`
    -`match_array([1,2])` - array contains same contents array, but order does not matter
    - `contain_exactly(1, 2)` - simiar to match_array, but items passed in as arguments, instead of array
    - `include`, `start_with`, `end_with` can be used strings
    - `include` can be used on hashes
5. Useful matchers
    - Regular Expression Matcher
        - `match(/^Regexphere$/)`
        - Only works on strings!
    - Object type Matcher
        - `be_instance_of(ClassName)` or `be_an_instance_of(ClassName)`
        - `be_kind_of(AnyAncestorClass)` or `be_a_kind_of(AnyAncestorClass)` or `be_a(AnyAncestorClass)` or `be_an(AnyAncestorClass)`
    - Respond To Matcher
        - `respond_to(:method_name)`
    - Attribute Matcher
        - `have_attributes(:key => "value")`
    - Satisfy Matcher
        - Matcher of last resort!
        - Takes a block (with result true or false)
        `satisfy {|value| value < 10 && value.odd?}`
        ```
        satisfy do |value|
            value <10 && value.odd?
        end
        ```
6. Predicate Matchers (Use cautiously!)
    - `be_nil` value.nil?
    - `be_odd` value. odd?
    - `be_even` value.even?
    - `be_zero` value.zero?
    - `be_nonzero` value.nonzero?
    - `be_integer` value.integer?
    - `be_empty` value.empty?


    `expect(value).to be_empty`
    is similar to calling
    `expect(value.empty?).to be true`

    Rspec removes `be_` from `be_suffix` and calls the `suffix?` method on the value. A.K.A this is dynamically generated
    Which means you can write your own method! 

    Or if you have a method `has_something?`

    - `have_something` - value.has_something?
    
    `expect(hash).to have_key(:key)` is similar to calling `expect(hash.has_key?(:key)).to be true`
7. Observation Matchers
    - Pass expect a block `{}`
    - Must have value before block
    - Block must change value
    - Observe Object Attributes/Values (before & after block )
        - `expect {}.to change().from(original).to(final)`
        - Different syntax - expects a block
        - Calls empty block first, then calls block of code and checks for change
        - e.g. 
        ```
        x = 10
        expect {x +=1}.to change{x}.from(10).to(11)
        expect {x +=1}.to change{x}.by_at_least(1)
        expect {x +=1}.to change{x}.by_at_most(1)
        expect {x +=1}.to change{x}.by(1)
        ```
    - Observe Errors
        - `expect {}.to raise_error`
        - `expect {}.to raise_exception`
        - `expect {1/0}.to raise_error(ZeroDivisionError)`
        - `expect {1/0}.to raise_error.with_message(/divided/)`
    - Observe Output
        - expect {print 'hello}.to output.to_stdout`
        - expect {print 'hello}.to output(/ll/).to_stdout`

## Compound Expectations
1. and
- `expect().to ___.and ___`
- `expect().to ___ & ___`

2. or
- `expect().to ___.or ___`
- `expect().to ___ | ___`

Composing Matchers
```
array = [1, 2, 3]

expect(array).to all (be < 5)
```

- all(matcher)
- include(matcher, matcher)
- start_with(matcher)
- end_with(matcher)
- contain_exactly(matcher, matcher, matcher)
- match(matcher)
- from(matcher).to(matcher) 
- by(matcher)


e.g.
```
fruits = ['apple', 'banana', 'cherry']

expect(fruits).to_start_with('apple')

expect(fruits).to start_with( start_with('a'))

expect(fruits.to start_with( (a_string_starting_with('a))) 
```

Noun-Phrase Aliases for Matchers
| Default Option   | Alternative Option           | 
| ------------- |:-------------:|
| start_with | a_string_starting_with |
| end_with | a_string_ending_with |
| match | a_string_matching |
| be < 2 | a_value < 2 |
| be_within | a_value_with |
| contain_exactly | a_collection_containing_exactly |
| be_an_instance_of | an_instance_of |

### `==` vs. `eql?` vs. `equal?`
- `==` Loose equality
- `eql?` Value equality
- `equal?` Identity euality 

```
x = 1.0
y = 1
x == y #true
x.eql?(y) #false
x.equal?(y) #false
x.equal?(1.0) #false
```

# Rspec with rails
## Setting up

Use the rspec-rails gem

1. Add to `group :development, :test` the gem:  `gem 'rspec-rails'`
2. In command line: `bundle install
3. In command line:`bin/rails g rspec:install`

This creates:
```
.rspec
spec
spec/spec_helper.rb
spec/rails_helper.rb
```

Note: if you use Rspec, best not to use the default test unit (Minitest)! When making new project: `rails new project -T` so that the default `test` folder isn't there.

## To run Rspecs in rails:  
`bundle exec rspec`

## Rspec-rails with generators
If you rails generate a model, with rspec-rails a spec file is also created.

`rails generate model ModelName`

`spec/models/model_name_spec.rb` will be created. 

If you created your model manually, you can generate a rspec spec
`rails generate rspec:model ModelName`

## Tips:
- let vs let!
    - let = lazy load (does not execute until called)
    - let = eager load (hits the database right away)
- in rails-rspec , `match_array` works works with active record relations.


## Useful Gems:
- Shoulda-matchers (to check associations, validations)
    - `have_many`
    - `validate_presence_of`
- Rspec-activemodel-mocks (for test doubles)
    - `mock_model` double of activerecord object
    - `stub_model` stub certain behaviours of activerecord object
    - create something that acts like activerecord object, but does not hit database.
- Rails-controller-testing
    - `assigns`
    - `assert_template`
    - Apparently not recommended for new rails apps..


## Model Specs


## Helper Specs
- Assigning instance variables
    - instead of `@var = 5`
    - usually written as `assign(:var, 5)
- Use `helper` method

## Controller Specs - Requests
### Controller Spec HTTP Requests
Simulates the HTTP request to the controller
- get(action, options)
- post(action, options)
- patch(action, options)
- put(action, options)
- delete(action, options)

Spec knows which controller, so we only need to specify action.
- Usually `get(:action)`
    - e.g. `get(:index)`
- Alternatively can pass a string which includes the controller
    - e.g. `get("/controller/action")`
- Can pass in params
    - `get(:user, {:id => 1})`
    - `post(:create, :user => {:first_name => "John", :last_name => "Doe"})`

### Objects:
- controller
- request
- response
### Hash Objects
- assigns (the assigns hash contains instance variables assigned in the action)
    - THIS IS DEPRECATED IN RAILS 5!

- session
- flash
- cookies
#### Tips for using hash objects:
- Access hashes with square bracket & string notation 
    -  In RSpec, When accessing the hashes, access using square notations with strings. e.g. `flash['notice]` NOT flash[:notice]
    - There is however, another  `flash(:notice)` , which is a method wrapper that converts a symbol to a string for you.
- When using cookies, specifiy if it is request or response. 
    - e.g. `request.cookies['logged_in]` or `response.cookies['logged_in]`

## Controller Specs - Response

### Controller Spec Response Matchers
- render_template
    - `expect(response).to render_template ( template_name )`
    - Delegates to Rails' assert_template
        - `assert_template` in Rails 5 is in external gem (rails-controller-testing)
    - Controller Specs DOES NOT actually render template by default !!
        - i.e. by default: `expect(respose.body).to eq('')` is true
        - add `render_views` if you want to render the views.
            - BUT this slows down the specs for all examples even if it does not need it
            - this is best left to view specs
- redirect_to
    - `expect(response).to redirect_to ( path_name )`
- have_http_status
    - `expect(response).to redirect_to ( httpstatus )`

#### HTTP status codes (can pass in number or symbol)
| Number   | Symbol | 
| ------------- |:-------------:|
| 200 | : ok |
| 403 | :forbidden |
| 404 | :not_found |
| 301 | :moved_permanently |
| 302 | :found |
| 500 | :internal_server_error |
| 502 | :bad_gateway |

## View Specs
- Isolated from controller & request-response
- One spec file per template
- Rspec infers controller & action based on the describe `controller/action` string.
- Does not render layout unless specified
    -  `render(:template => 'users/index.html.erb', layout: 'layout.html.erb')`
- You can stub partials
    e.g. `stub_template('shared/_partial.html.erb' => "Replacement text" )

3 Typical Parts for View Specs
1. Make instance variable assignments (because we are not going through controller!)
2. Render template (using `render`)
3. Set expectations on what is returned

Rspec-rails Helpers for View Specs
- view
- assign
- render (renders the view)
    - can pass in template if you did not pass it into describe string, e.g.
    `render(:template => 'users/index.html.erb')`
- rendered (where results of render is kept)
