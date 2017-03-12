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
