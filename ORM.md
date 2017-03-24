### Object Relational Mapping (ORM)

ORM is similar to a translation service. For example, Rails speaks Ruby while a database speaks SQL. ORM allows Rails developers to manipulate a database using Ruby instead of writing SQL. Rails uses an ORM library called `ActiveRecord` to perform this translation.

Structured Query Language (SQL): the standard language for accessing and manipulating databases.

### Rails Console

The Rails console loads our app in a shell and provides access to Rails methods, app-specific methods, persisted data, and Ruby.

`rails c`: launches the Rails console from the command line.

```
Loading development environment (Rails 5.0.2)
2.4.0 :001 > Post.create(title: "First Post", body: "This is the first post in our system")
   (0.0ms)  begin transaction
  SQL (1.3ms)  INSERT INTO "posts" ("title", "body", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "First Post"], ["body", "This is the first post in our system"], ["created_at", 2017-03-23 16:01:46 UTC], ["updated_at", 2017-03-23 16:01:46 UTC]]
   (1.4ms)  commit transaction
 => #<Post id: 1, title: "First Post", body: "This is the first post in our system", created_at: "2017-03-23 16:01:46", updated_at: "2017-03-23 16:01:46">

```
In our example above, we created a post using `Post.create`, which created a new row in the `posts`table. The `create` method is part of the `ActiveRecord` class that `Post` inherits from.

``` Ruby

#post.rb
class Post < ApplicationRecord
  has_many :comments
end

#application_record.rb
class ApplicationRecord < ActiveRecord::Base
  self.abstract_class = true
end

```

Then we passed the hash, which has two keys, `title` and `body` to the `create` method. `create` translates `Post.create(title: "First Post", body: "This is the first post in our system")` to SQL.


### SQL at Work

```
  SQL (1.3ms)  INSERT INTO "posts" ("title", "body", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "First Post"], ["body", "This is the first post in our system"], ["created_at", 2017-03-23 16:01:46 UTC], ["updated_at", 2017-03-23 16:01:46 UTC]]
```

`INSERT INTO "posts"` : adds a row to the posts table using the `INSERT INTO` SQL statement.

`("title", "body", "created_at", "updated_at")`: column names or attributes of the `posts` table.

`[["title", "First Post"], ["body", "This is the first post in our system"], ["created_at", 2017-03-23 16:01:46 UTC], ["updated_at", 2017-03-23 16:01:46 UTC]]`: values that correspond to the column names.

`created_at` and `updated_at` are default columns that Rails adds automatically, which is why they don't need to be specified in the `create` call.


```
(1.4ms)  commit transaction
=> #<Post id: 1, title: "First Post", body: "This is the first post in our system", created_at: "2017-03-23 16:01:46", updated_at: "2017-03-23 16:01:46">
```

Committing the transaction executes  `INSERT INTO`. Commit statements end a SQL transaction and make all changes permanent. A transaction is one or more SQL statements that a database treats as a single unit.

### Retrieving information

A row in a table corresponds to an instance of a class. Like a class instance, a row in a database table is unique. ORM allows us to retrieve information stored in a row and map it to a class instance that we create in our application.

```
2.4.0 :002 > post = Post.first
  Post Load (6.6ms)  SELECT  "posts".* FROM "posts" ORDER BY "posts"."id" ASC LIMIT ?  [["LIMIT", 1]]
 => #<Post id: 1, title: "First Post", body: "This is the first post in our system", created_at: "2017-03-23 16:01:46", updated_at: "2017-03-23 16:01:46">

```

`Post.first` executes a `SELECT` SQL statement and fetches the first row from the posts table. `SELECT` is used to fetch a set of records from one or more tables.

After fetching the first row, `ActiveRecord` converts the row's data into an instnace of `Post`, or a post object. This post object is then assigned to the `post` variable. `ActiveRecord` is what makes the conversion from a database record to a Ruby object possible.

```
2.4.0 :004 > post.comments.create(body: "First comment!")
   (0.1ms)  begin transaction
  SQL (1.3ms)  INSERT INTO "comments" ("body", "post_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["body", "First comment!"], ["post_id", 1], ["created_at", 2017-03-24 00:06:01 UTC], ["updated_at", 2017-03-24 00:06:01 UTC]]
   (0.8ms)  commit transaction
 => #<Comment id: 1, body: "First comment!", post_id: 1, created_at: "2017-03-24 00:06:01", updated_at: "2017-03-24 00:06:01">
 ```

 `post.comments.create`: `ActiveRecord` interpreted this as "create a new comment for the first post".


 ```
 2.4.0 :005 > post.comments
  Comment Load (0.2ms)  SELECT "comments".* FROM "comments" WHERE "comments"."post_id" = ?  [["post_id", 1]]
 => #<ActiveRecord::Associations::CollectionProxy [#<Comment id: 1, body: "First comment!", post_id: 1, created_at: "2017-03-24 00:06:01", updated_at: "2017-03-24 00:06:01">]>
 ```
`post.comments` returns an `ActiveRecord::Association` because a comment depends on a given post.

### `ActiveRecord` Associations

`has_many` and `belongs_to` represent associations between posts and comments.

```Ruby

class Comment < ApplicationRecord
  belongs_to :post
end

```

The `belongs_to :post` declaration generates a `post` method for each comment, giving us the ability to call `.post` on an instance of `Comment` and retrieve the associated post. The database stores this relationship, keeping a `post_id` (foreign key) for each comment.

```Ruby

class Post < ApplicationRecord
  has_many :comments
end

```

The `has_many :comments` declaration in `Post` is the counterpart of `belongs_to :post`. The posts table makes no reference to comments, so this relationship is stored in the comments table exclusively. A post retrieves its associated comments by fetching all the comments with a `post_id` that matches the id of the post. Storing the relationship in the comments table is a database strategy to allow data to be intersected or joined in an efficient manner.


```
2.4.0 :014 > post.comments.each { |comment| p comment.body }
"First comment!"
"Second Comment!"
 => [#<Comment id: 1, body: "First comment!", post_id: 1, created_at: "2017-03-24 00:06:01", updated_at: "2017-03-24 00:06:01">, #<Comment id: 2, body: "Second Comment!", post_id: 1, created_at: "2017-03-24 00:19:06", updated_at: "2017-03-24 00:19:06">]
 ```

 The `|comment|` bock represents an instance of `Comment` with each iteration. We call body of each comment instance to retrieve the comment's body attribute from the database.

 The `SELECT` statement fetches all the comments with the given `post_id`.
