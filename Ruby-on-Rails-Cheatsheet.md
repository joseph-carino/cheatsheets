[back to overview](/../..)
Looking for [Ruby](../master/Ruby-Cheatsheet.md)?

# Ruby on Rails Cheatsheet<!-- omit in toc -->

- [Basics](#basics)
  - [request/response cycle](#requestresponse-cycle)
  - [1. Generate a new Rails app](#1-generate-a-new-rails-app)
  - [2. Generate a controller and add an action](#2-generate-a-controller-and-add-an-action)
  - [3. Create a route that maps a URL to the controller action](#3-create-a-route-that-maps-a-url-to-the-controller-action)
  - [4. Create a view with HTML and CSS](#4-create-a-view-with-html-and-css)
- [Connect a Database](#connect-a-database)
  - [Example: messaging system](#example-messaging-system)
  - [1. Generate a new Model](#1-generate-a-new-model)
    - [Associations](#associations)
  - [2. Generate a controller and add actions](#2-generate-a-controller-and-add-actions)
  - [3. Create a route that maps a URL to the controller action](#3-create-a-route-that-maps-a-url-to-the-controller-action-1)
  - [4. Create a view with HTML and CSS](#4-create-a-view-with-html-and-css-1)
- [Extras](#extras)
- [Troubleshoots](#troubleshoots)

# Basics

## request/response cycle

controller > route > view
learn request/response cycle: https://www.codecademy.com/articles/request-response-cycle-forms

## 1. Generate a new Rails app

```bash
$ rails new MySite
# or using Postgress $ rails new myapp --database=postgresql
$ bundle install
$ rails server

> http://localhost:8000 # or similar Number up and running!
```

## 2. Generate a controller and add an action

```
$ rails generate controller Pages
```

generates a new controller named Pages

Open: app/controllers/pages_controller.rb

```Ruby
class PagesController < ApplicationController

  def home # add the home action/method to the pages controller
  end

end
```

## 3. Create a route that maps a URL to the controller action

Open: config/routes.rb

```Ruby
Rails.application.routes.draw do

  get 'welcome' => 'pages#home'

end
```

## 4. Create a view with HTML and CSS

Open: app/views/pages/home.html.erb

```html
<!-- write some html -->
<div class="main">
  <div class="container">
    <h1>Hello my name is King</h1>
    <p>I make Rails apps.</p>
  </div>
</div>
```

# Connect a Database

## Example: messaging system

## 1. Generate a new Model

[Let rails create the model for you as below](https://guides.rubyonrails.org/active_record_migrations.html#model-generators), or [manually create the class and migration yourself](https://guides.rubyonrails.org/active_record_migrations.html#creating-a-standalone-migration)
```
$ rails generate model Message
```

This creates model named Message, with two files:
- Model file app/models/message.rb
- Migration file db/migrate/YYYYMMDDHHMMSS_create_message.rb

```Ruby
class CreateMessages < ActiveRecord::Migration
  def change
    create_table :messages do |t|
			t.text :content # add this
      t.timestamps
    end
  end
end
```

1. `change` method tells Rails what change to make to the database.
2. `t.text :content` - create text column called content in the messages tables.
3. `t.timestamps` - a Rails command that creates columns created_at and updated_at. These columns are automatically set when a message is created and updated.

```
$ rails db:create # create db
$ rails db:drop # drop db
$ rails db:schema:load # run schema.rb
$ rails db:schema:dump # dump db schema to schema.rb
$ rails db:version # print schema version
$ rails db:migrate # run migrations
$ rails db:rollback # [rollback previous migration](https://edgeguides.rubyonrails.org/active_record_migrations.html#rolling-back)
$ rails db:seed # seed by running seeds.rb
$ rails db:setup # db:create, db:schema:load, db:seed
$ rails db:reset # db:drop, db:setup
$ rails db:migrate:reset # db:drop, db:create, db:migrate
```

Seed data in seeds.rb:
```Ruby
m1 = Message.create(content: "Text1") # new object and save to db
m3 = Message.new(content: "Test") # create new message object, do not save yet
m3.save # save object to db => true
m3.persisted? # => true
```

### [Associations](https://guides.rubyonrails.org/association_basics.html)

## 2. Generate a controller and add actions

```
$ rails generate controller Messages
```

Open: app/controllers/messages_controller.rb

Note class name in PascalCase must match file name in snake_case
```Ruby
class MessagesController < ApplicationController

  # Rails defines standard controller actions can be used to do common things such as display and modify data.
  # index, show, new, create, edit, update, and destroy
  # code below is optional

  def index
    @messages = Message.all # retrieves all messages from database and stores them in variable @messages.
  end

  def new
    @message = Message.new #
  end

  def create
    @message = Message.new(message_params)
    if @message.save
      redirect_to '/messages'
    else
      render 'new'
    end
  end

  private

  def message_params
    params.require(:message).permit(:content)
  end

end
```

## 3. Create a route that maps a URL to the controller action

Open: config/routes.rb

```Ruby
Rails.application.routes.draw do

  get 'messages' => 'messages#index' # 'messages' route targets class 'MessagesController' in file /app/controllers/messages_controller.rb action 'index'
  get 'messages/new' => 'messages#new'
  post 'messages' => 'messages#create'

  resources :messages # maps conventional routes
end
```

- `resources :MODEL` - sets up conventional routes, also helpers based on prefixes. [see here](https://guides.rubyonrails.org/getting_started.html#resourceful-routing)
- `resources :MODEL do resources :NESTED end` - nests NESTED in MODEL
- `$ rails routes` - shows all configured routes


## 4. Create a view with HTML and CSS

Open: app/views/messages/index.html.erb

```html
<div class="messages">
  <div class="container">
    <% @messages.each do |message| %>
    <!-- iterates through each message in @messages created in the Messages controller's index -->
    <div class="message">
      <p class="content"><%= message.content %></p>
      <p class="time"><%= message.created_at %></p>
    </div>
    <% end %> <%= link_to 'New Message', "messages/new" %>
    <!-- Sets a link to the creation page -->
  </div>
</div>
```

Open: app/views/messages/new.html.erb

```html
<div class="create">
  <div class="container">
    <!-- Create a form with a textarea field -->
    <%= form_for(@message) do |f| %>
    <div class="field">
      <%= f.label :message %><br />
      <%= f.text_area :content %>
    </div>
    <div class="actions"><%= f.submit "Create" %></div>
    <% end %>
  </div>
</div>
```

# Extras

- [Credentials](https://edgeguides.rubyonrails.org/security.html#custom-credentials)

# Troubleshoots

1. When seeing only a blank page after running rails server:
   http://stackoverflow.com/questions/25951969/rails-4-2-and-vagrant-get-a-blank-page-and-nothing-in-the-logs
2. Follow instructions to use postgres:
   https://www.digitalocean.com/community/tutorials/how-to-setup-ruby-on-rails-with-postgres
3. [Rollback](https://edgeguides.rubyonrails.org/active_record_migrations.html#rolling-back)
