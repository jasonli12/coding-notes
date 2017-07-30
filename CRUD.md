### CRUD (Create, Read, Update, Delete)

Resource: an object that users need to be able to access. It is a collection of models, views, controllers and routes.

``` Ruby

# config/routes.rb

Rails.application.routes.draw do
  resources :post

  get `about` => 'welcome#about'

  root 'welcome#index'
end

```
`resources :post`: calls the `resources` method and passes it a `Symbol`, which creates 7 different routes in the application, all mapping to the controller.

`resources :post, only: :index`: will only generate specified routes (i.e. `:index` in example)

`resources :post, only: :show`: will only generate specified routes (i.e. `:show` in example). This takes a parameter (`:id`), which is the `post` we want to look at.

```
Prefix Verb URI Pattern          Controller#Action
  post GET  /posts/:id(.:format) posts#show

```

`resources :posts, only: [:new, :create]`: Note that the verb for `posts#create` has a `POST` verb instead of a `GET` verb

```
Prefix Verb URI Pattern          Controller#Action
 posts POST /posts(.:format)     posts#create
new_post GET  /posts/new(.:format) posts#new
```
`resources :posts, only: [:edit, :update]`:

```   
Prefix Verb  URI Pattern               Controller#Action
edit_post GET   /posts/:id/edit(.:format) posts#edit
     post PATCH /posts/:id(.:format)      posts#update
          PUT   /posts/:id(.:format)      posts#update
```


`get 'about' => 'welcome#about'`: Allows users to visit `/about` instead of `welcome/about`


`rake routes`:

```
Prefix Verb   URI Pattern               Controller#Action
 posts GET    /posts(.:format)          posts#index
       POST   /posts(.:format)          posts#create
new_post GET    /posts/new(.:format)      posts#new
edit_post GET    /posts/:id/edit(.:format) posts#edit
  post GET    /posts/:id(.:format)      posts#show
       PATCH  /posts/:id(.:format)      posts#update
       PUT    /posts/:id(.:format)      posts#update
       DELETE /posts/:id(.:format)      posts#destroy
 about GET    /about(.:format)          welcome/about#about
  root GET    /                         welcome#index
```

`Verb`: HTTP Request Methods. They specify the action to be done on the specified resource.
+ `GET`: asks for data
+ `POST`: creates data
+ `PATCH`/`PUT`: updates data
+ `DELETE`: deletes data

Rails created a route `/posts` which requires a `GET`. The route maps to the `index` method in `PostsController`

`PATCH` vs `PUT`: `PUT` updates data by sending the complete resource. `PATCH` sends just the changes.


| CRUD Action        | HTTP Verb           | Rails Action(s)    |
| ------------------ |---------------------| -------------------|
| Create             | POST                | create             |
| Read               | GET                 | new/show/index/edit|
| Update             | PUT/PATCH           | update             |
| Delete             | DELETE              | destroy            |
