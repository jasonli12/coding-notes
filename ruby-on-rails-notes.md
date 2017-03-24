# Rails

### Creating a New Rails App

`rails _4.2.5_ new bloccit -T`

`_4.2.5_`: declares the version of Rails we would like to build the application in

`bloccit`: app name

`-T`: specifies that the app should not be created with standard test packages (include `T` if we are testing our app with RSpecs)

`rails new`: creates the Rails app structure

`rails s`: Start the Rails server

**README**
+ Describes what the app or program does
+ How to install the app/program
+ How to run tests
+ Anything else another developer would need to know such as: Ruby version, System dependencies, Configuration, Database creation, Database initialization, Services (job queues, cache servers, search engines, etc.), Deployment instructions

**Gemfile**:

If we changed our `Gemfile`, we will need to run `bundle install --without production` in the root directory of our app, which installs everything specified in the `Gemfile` and ensure all the gems work harmoniously. `--without production` ignores gems in `group :production`. The Production environment will automatically run `bundle install` during deployment and will account for gems in `group :production` at that point.

`rake db:create`: creates a new local database for our app to use. Must be run after creating a new app or after dropping an existing database.

**The Asset Pipeline**

+ Provides a framework to concatenate and minify, or compress, JavaScript and CSS assets.
+ Adds the ability to write these assets in other languages, such as CoffeeScript, Sass, and ERB.
+ Purpose is the make Rails apps fast by default while allowing developers to write "assets" (images, styles, and JavaScript) in a variety of languages.

*Rails 4 Deployment to Heroku*: need to add `gem 'rails_12factor'` to `group :production` in order for Rails 4 to serve your assets. Full explanation [here](https://devcenter.heroku.com/articles/rails-4-asset-pipeline).

```ruby
group :production do
   gem 'pg'
   gem 'rails_12factor'
 end
```
### MVC (Model View Controller) Architecture

**Model**: contains the data for the application and is often linked to a database. It contains the state of the application and all business logic. It has no knowledge of user interfaces, so it can be easily reused. It isn't aware of how it's information is being presented to the user.

**View**: a webpage. It generates the user interface, which presents data to the user. The view is what the user is actually seeing. It doesn't do any processing and its work is complete when it is done displaying to the user. There is no logic or modification done to the data by the view. Many views can access the same model for different reasons or ways.

**Controller**: determines what view should be shown. Controllers should only be concerned with passing things to other parties. It receives events from the outside world, usually through views and interacts with the model either retrieving information from the model or updating information stored in the model. It then passes the information to the user via the view.

+ When you visit a website, you initiate a chain of actions or requests. A request is handled by a controller, which receives information from the model layer, and then uses that information to display a view.

+ Analogy: A customer (user) places an with the waiter (controller). The waiter (controller) informs the kitchen (model) of the order. After the kitchen (model) makes the order, the waiter serves the dish (view) to a customer.

+ MVC is the basic architectural pattern that guides the creation of all Rails applications


### Generating a Controller and Views

`rails generate controller welcome index about`

`welcome`: represents controller name
`index` and `about`: views corresponding to `welcome` controller

``` ruby

app/controllers/welcome_controller.rb

class WelcomeController < ApplicationController
  def index
  end

  def about
  end
end

```

`WelcomeController` is a Ruby class and contains two empty methods corresponding to view names (i.e. `index` and `about`).

**Default Rendering**: When a controller's method's purpose is to invoke a view, it must be named with respect to the view. Ex: The `index` method in the `WelcomeController` will invoke the `index` view inside the `app/views/welcome` directory.

### Generating Models

```

$ rails generate model Post title:string body:text
      invoke  active_record
   identical    db/migrate/20150606010447_create_posts.rb
   identical    app/models/post.rb
      invoke    rspec
      create      spec/models/post_spec.rb
```

The model generator (`rails generate model Post title:string body:text`) created a model called `Post` with two attributes: `title` and `body`. `title` will be a string data type, while `body` will be a text data type.

`post.rb`: a Ruby class that represents the `Post` model. This class will handle the logic and define the behavior of posts.

`post_spec.rb`: the test spec for the `Post` class.

`20150606010447_create_posts.rb`: the database migration file. A migration file defines the action taken on a database for a given model. An app will have many migration files and together they serve as a set of instructions for building a database.

```
$ cat app/models/post.rb
class Post < ApplicationRecord
end
```
When the model generator created this template, it made the `Post` class inherit from `ApplicationRecord` which in turns inherits from `ActiveRecord::Base`. Up until Rails 4.2, all models inherited from `ActiveRecord::Base`, but starting with Rails 5, all models inherited from `ApplicationRecord`. The idea is that any customizations and extensions will be localized only to models inheriting from `ApplicationRecord`, which is effectively your app.

```Ruby
# app/models/application_record.rb

class ApplicationRecord < ActiveRecord::Base
  self.abstract_class = true
end
```

`ActiveRecord::Base` essentially handles interaction with the database and allows us to persist data through our class.

`schema.rb`: a file located in the `db` directroy that represents an application's complete database architecture; the tables it uses and how those tables relate to each other.

Rake is a Ruby build command that allows us to execute administrative tasks for our application. To see a complete list of rake tasks, type `rake --tasks` from the command line.

#### Relationships and Associations

```Ruby

rails generate model Comment body:text post:references

$ cat app/models/comment.rb
class Comment < ActiveRecord::Base
  belongs_to :post
end

```
In the example above, a comment belongs to a post or a post has many comments. The `post_id` attribute was generated when the `Comment` model was generated with the `post:references` argument.

Each model instance in a Rails app automatically gets assigned an `id` attribute to uniquely identify it. Each `post` and `comment` will each have a unique `id`. To make a comment belong to a post, we need to provide the post `id` to the comment. This is achieved using a foreign key.

A foreign key is the `id` of one model, used as an attribute in another model, in order to see the relationship. In the Post/Comment example, the `Comment` model will need to have an attribute named `post_id`, which exists so that a `comment` can belong to a `post`. To allow many comments belong to one `post`, you will have many comment records with the same `post_id` attribute.

```Ruby
class Post < ApplicationRecord
  has_many :comments
end
```
The `has_many` method allows a post instance to have many comments related to it and also methods to retrieve comments that belong to a post.


### Routing in Rails

The controller generator (`rails generate controller welcome index about`) created the basic code needed for the `WelcomeController` and its views, and it also created code in the `config/routes.rb` file:

``` ruby

Rails.application.routes.draw do
  get 'welcome/index'

  get 'welcome/about'
  ...
end

```
This code creates HTTP `GET` routes for the index and about views.

**HTTP (Hypertext Transfer Protocol)**: the protocol that the Internet uses to communicate with websites. The `get` action corresponds to the HTTP `GET` verb. `GET` requests are used to retrieve information identified by the URL.

If `routes.rb` doesn't specify a `GET` action, the view will not be served because the application won't know what to get when a user sends a request.

``` ruby

Rails.application.routes.draw do
  get 'welcome/index'

  get 'welcome/about'

  root 'welcome#index'
  ...
end

```

The `root` method allows us to specify the default page to load when we navigate to the home page URL

`rake routes`: shows all your app's available routes

```
       Prefix Verb URI Pattern              Controller#Action
welcome_index GET  /welcome/index(.:format) welcome#index
welcome_about GET  /welcome/about(.:format) welcome#about
         root GET  /                        welcome#index
```
`Prefix`: the route name: welcome_index
`Verb`: the HTTP action associated with the route: `GET`
`URI`: URL used to request the view: `/welcome/index`
`Controller#Action`: route destination, which translates to the controller and associated view: `welcome#index`

### Testing with RSpec

`rails generate rspec:install`: creates a spec directory where we write our tests. When we run `rails generate model...` or `rails generate controller...`, RPsec will now automatically add test files for our models and controllers.

`rails generate rspec:controller welcome`: generates a spec for `WelcomeController`


Tests should be run in isolation because they can alter data stored in a database. By default, Rails designates a separate database for testing. The test database is completely empty before you run your specs. Therefore, a spec must create the necessary data to test functionality. When the test is complete, the data is destroyed. RSpec empties the Test database before running each spec. **Each test must create the data it needs.**

#### Red, Green, Refactor

1. Write a failing test for production functionality that does not exist (Red).
  + Ensure the test actually fails. This way we ensure that the new spec does not pass and removes the possibility that the test always passes, which is a sign of a poorly-written test.
2. Create the production functionality (code) such that the test passes.
3. Refactor the production code to make it cleaner and more sustainable.

#### Basic Testing Principles
+ **Keep tests as low-level as possible**: Test models thoroughly, test controllers moderately and test complete application flow lightly.
+ **Respect object limits**: When testing an object, try not to test any other objects, even if they're related. Narrow the scope of the test to be as small as self-contained as possible.
+ **Don't test "how", test "what"**: We want to test what a method returns not how it returns it. The implementation piece (the "how") is subjective and can be improved through refactoring.
+ **Write DRY tests**: Whenever possible, avoid repetition in tests, just like production code.
+ **Test early and often**: At a minimum, run our specs before each commit. This way it allows us to reduce bugs proactively before adding them to our codebase.


### HTML and CSS
`.html.erb` file extension: allows us to use HTML and Ruby in the same file. ERB stands for "embedded Ruby." By integrating Ruby code with HTML, we can dynamically change the behavior of static HTML code based on user input.

`application.html.erb`: Similar to `index.html`. It contains the common code that will be included in every view. Other views will be called from and rendered inside this `application.html.erb` file.

#### Workflow

1. A user requests a view.
2. The controller corresponding to the requested view invokes `application.html.erb`.
3. `application.html.erb` inserts the appropriate view using `yield`
4. The complete web page is rendered and returned to the user.

`yield`: used to invoke a block, which renders a given view inside the `application.html.erb` container.

Code between `<%` and `%>` is interpreted as Ruby.

If the `<% %>` contains an `=`, such as `<%= %>`, the result of the Ruby code is printed to the screen (i.e. rendered as HTML). Without the `=`, the Ruby code is only *executed* but not printed.


``` html
<ul>
  <li><%= link_to "Home", welcome_index_path %></li>
  <li><%= link_to "About", welcome_about_path %></li>
</ul>

```
`link_to`: a Rails helper method available in views and returns a valid HTML hyperlink

In the example below, `link_to` takes two arguments, a string `"Home"`, which will be the display name of the hyperlink and a path `welcome_index_path`. `welcome_index_path` is a Rails method, generated by the `routes` file. Type `rake routes` in the command line and the `Prefix` column shows the route name. When you add `_path` to the route name, it is recognized as a helper method that returns `"/welcome/index"` in our example.

``` html
<li><%= link_to "Home", welcome_index_path %></li>
```

translates to:

```html
<a href="/welcome/index">Home</a>
```

#### CSS Selectors in Rails

When running `rails generate controller`, a `.scss` file was created. By Rails convention, each controller has a corresponding stylesheet and view. Similar to `.html.erb`, the `.scss` extension provides us with some additional syntax options (known as Sass) to enhance default CSS capabilities

``` css
app/assets/stylesheets/welcome.scss

h1 {
  color: red;
}
```
### Seeding Data in Rails

`seeds.rb` is a script we can run to seed the development database with test data.

```Ruby

# db/seeds.rb

require 'random_data'

# Create Posts
50.times do
# #1
  Post.create!(
# #2
    title:  RandomData.random_sentence,
    body:   RandomData.random_paragraph
  )
end
posts = Post.all

# Create Comments
# #3
100.times do
  Comment.create!(
# #4
    post: posts.sample,
    body: RandomData.random_paragraph
  )
end

puts "Seed finished"
puts "#{Post.count} posts created"
puts "#{Comment.count} comments created"

```
`create!` with a bang(`!`) instructs the method to raise an error if there's a problem with the data we're seeding.

`sample`: returns a random element from the array every time it's called.

If we add `config.autoload_paths << File.join(config.root, "lib")` to `application.rb` it will autoload any references to the `lib` directory used by our code.

`rake db:reset`: drops the database

```
Jasons-MBP:bloccit jasonli$ rake db:reset
Dropped database 'db/development.sqlite3'
Dropped database 'db/test.sqlite3'
Created database 'db/development.sqlite3'
Created database 'db/test.sqlite3'
-- create_table("comments", {:force=>:cascade})
   -> 0.0128s
-- create_table("posts", {:force=>:cascade})
   -> 0.0019s
-- initialize_schema_migrations_table()
   -> 0.0016s
-- create_table("comments", {:force=>:cascade})
   -> 0.0051s
-- create_table("posts", {:force=>:cascade})
   -> 0.0030s
-- initialize_schema_migrations_table()
   -> 0.0016s
Seed finished
50 posts created
100 comments created
```

`find`: an `ActiveRecord` class method that finds the instance using the unique id passed through it.

`p = Post.find 3`: We called `find` on `Post` and passed it a value that represents the unique `post_id`. `find` will return the instance (row) of post data that corresponds to an id of 3.

`p.comments.count`: `count` is another `ActiveRecord` method

`rake db:seed`: adds new data to your database

**Idempotence**: seeding unique data using `seeds.rb` without erasing or duplicating existing data. Idempotent code can be run many times or one time with identical results.
