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


### Generating a Controller and views

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
