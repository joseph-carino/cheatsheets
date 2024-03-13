[back to overview](/../..)
Looking for [Ruby](../master/Ruby-Cheatsheet.md)?

# Ruby on Rails Cheatsheet<!-- omit in toc -->

- [Basics](#basics)
  - [Terms](#terms)
  - [Commands](#commands)
  - [Request/Response Cycle](#requestresponse-cycle)
  - [Database Commands](#database-commands)
- [Panama](#panama)
  - [Usage Example](#usage-example)
- [Tutorial - To be moved](#tutorial---to-be-moved)
  - [1. Generate a new Rails app](#1-generate-a-new-rails-app)
  - [2. Generate a controller and add an action](#2-generate-a-controller-and-add-an-action)
  - [3. Create a route that maps a URL to the controller action](#3-create-a-route-that-maps-a-url-to-the-controller-action)
  - [4. Create a view with HTML and CSS](#4-create-a-view-with-html-and-css)
  - [5. Generate a new Model](#5-generate-a-new-model)
    - [Associations](#associations)
  - [6. Generate a controller and add actions](#6-generate-a-controller-and-add-actions)
  - [7. Create a route that maps a URL to the controller action](#7-create-a-route-that-maps-a-url-to-the-controller-action)
  - [8. Create a view with HTML and CSS](#8-create-a-view-with-html-and-css)
- [Extras](#extras)
- [Troubleshooting](#troubleshooting)

# Basics

## Terms
- `engine`: mini applications - `Rails::Application` inherits from `Rails:Engine`. Generated with `rails plugin new <name> --[full|mountable]`. Engine is a full plugin. [See more](https://guides.rubyonrails.org/engines.html)

## Commands
- `rails console`: jump into rails interactive console

## Request/Response Cycle
controller > route > view
learn request/response cycle: https://www.codecademy.com/articles/request-response-cycle-forms

## Database Commands
- `rails db:create` - create db
- `rails db:drop` - drop db
- `rails db:schema:load` - run schema.rb
- `rails db:schema:dump` - dump db schema to schema.rb
- `rails db:version` - print schema version
- `rails db:migrate` - run migrations
- `rails db:migrate RAILS_ENV=<env>` - run migrations in `<env>` (development, test)
- `rails db:rollback` - [rollback previous migration](https://edgeguides.rubyonrails.org/active_record_migrations.html#rolling-back)
- `rails db:seed` - seed by running seeds.rb
- `rails db:setup` - db:create, db:schema:load, db:seed
- `rails db:reset` - db:drop, db:setup
- `rails db:migrate:reset` - db:drop, db:create, db:migrate
- `rails generate migration <PascalName> ` - new migration `db/migrate/<snake_name>.rb`
```

# Logging, Metrics, Alerting, Observability
## Logging
- General Logging: Use [Splunk](https://logs.shopify.io/en-US/app/launcher/home)
  - `Rails.logger.error("Error message")` - log to splunk
- Logging for bugs: Use [Bugsnag](https://app.bugsnag.com/shopify/fbs/overview)
  - `Bugsnag.notify('TurboGraft:script-error', scriptSrc);` - JS
  - `rescue StandardError => e Bugsnag.notify(e)` - RB
## Metrics
- Use [DataDog](https://shopify.datadoghq.com)
  - `StatsD.event("event-name")` - event metric
  - `StatsD.gauge("metric-name", value)` - gauge, new values replace old values
  - `StatsD.distribution("metric-name", value)` - modified gauge, distribution of values over a sample period
  - `StatsD.increment("metric-name", value)` - counting, new values added to existing. generally value is 1
  - `StatsD.histogram("metric-name", value)` - histogram
  - `StatsD.measure("metric-name", value)` - timing measurement
## Jobs
- Use [Sidekiq](https://fbs.shopifycloud.com/sidekiq)
## DB
- Use [VividCortex](https://go.vividcortex.com/75c845789f)

# [Sagas](https://github.com/Shopify/fbs/blob/main/components/platform/SAGAS.md)
- Series of steps/transactions executed in sequence
- Replace "Workflows"
- see `components/<component>/app/public/<component>/orchestrators/`
- Orchestrator (inherits Platform::Sagas::Orchestrator) contains `event`s:
    - starting a saga involves calling `dispatch!` on the Orchestrator, passing the matching event
    - `event` contains a list of `step` actions (unit of business logic)
        - see `components/<component>/app/services/<component>/<related-to-orchestrator>/saga_steps/`
```Ruby
MyComponent::MyOrchestrator.dispatch!(:my_event, inbound_shipment_id: 7, other_required_parameter: "some value") # run saga async
result = orchestrator.run!(:my_event, inbound_shipment_id: 7, other_required_parameter: "some value", trace_id:"someId") # run saga sync for testing
...
module MyComponent
  class MyOrchestrator < Platform::Sagas::Orchestrator
    event
      :my_event, # unique for this orchestrator
      for_resource: InboundTransfer, # This event manipulates InboundTransfer objects
      resource_id_key: :inbound_shipment_id, # Use inbound_shipment_id instead of inbound_transfer_id to locate resource
      input_schema: { inbound_shipment_id: Integer, other_required_parameter: String } # All parameters listed here are required
    do
      step MyComponent::CommandProcessors::MyLegacyCommandClass # legacy workflow step
      step MyComponent::MyStepCommand # saga step
      step( # inline step
        "inline_step_unique_name", # Step identifier, unique for the saga
        transactional: true, # Execute the step as a transaction
        input_schema: { some_input_param: Integer } # Inputs will be validated against this schema at runtime
      ) do
        perform_command do |input|
          # Logic to be execute for this step
          step_result(some: data) # use step_result to return step results, injected into input for this saga
        end
        on_failure_command do |input|
          # Logic to be execute in case of step execution failure
        end
      end
      branch :conditional_param do # :conditional_param should be in the input to this event
        condition :success do
          # This will run if input[:conditional_param] == :success
          step "success_step_1" do ... end
          ...
        end
        condition :failure do
          ...
        end
        condition :other do
          ...
        end
        condition ->(input){ input[:some_param].present? } do
          # This will run if the above lambda evaluates to true
          step "lambda_step_1" do ... end
          ...
        end
      end
      # Iterate over input[:ids] (do nothing if not found or not an Array)
      for_each :ids do
        # These steps will be executed for each element in input[:ids]
        step "iter-step-1" do ... end
        step "iter-step-2" do ... end
        ...
      end
    end
...
module MyComponent
  class MyStepCommand < Platform::Sagas::StepCommandRunner
    transaction true # should this step be executed as a transaction?
    input_schema { some_resource_id: Integer } # define here the step input schema for validation

    def perform
      # Override perform to define the step behavior
      new_value = @input[:old_value] + 1

      # cancel() will stop execution of the whole saga early
      step_result(new_value: new_value) # use step_result to return step results
    end

    def on_failure
      # Optionally override on_failure to provide logic in case of errors
      new_value = @input[:old_value] - 1
      step_result(new_value: new_value)
    end
  end
end
```

# Panama
Used for query of BigData storage pre-computed reports
See [docs](https://panama.docs.shopify.io/) and [dashboard](https://panama.shopifycloud.com/) and [setup](https://vault.shopify.io/pages/5859-Panama-Setup)
## [Usage Example](https://github.com/Shopify/shopify/blob/main/components/apps/app/services/shopify_fulfillment_network/eligibility_checker.rb)
```Ruby
# add gem "shopify_panama", "~> 1.2.21" to GEMFILE
module PanamaInput
  DATASET               = "sfn-qualifications"
  QUALIFICATION_NAME    = "app_install"
  QUALIFICATION_VERSION = "2.1"
end
def check_eligibility_of_merchant(shop_id)
  panama_client = Shopify::Config.panama_client
  scope = { "shopify_shop_id" => shop_id.to_s,
            "qualification_name" => PanamaInput::QUALIFICATION_NAME,
            "qualification_version" => PanamaInput::QUALIFICATION_VERSION, }
  raw_response = panama_client.find_record(scope, PanamaInput::DATASET)&.payload
end
```

# Tutorial - To be moved

## 1. Generate a new Rails app
```bash
$ rails new MySite
# or using Postgress $ rails new myapp --database=postgresql
$ bundle install
$ rails server

> http://localhost:8000 # up and running!
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

## 5. Generate a new Model
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

Seed data in seeds.rb:
```Ruby
m1 = Message.create(content: "Text1") # new object and save to db
m3 = Message.new(content: "Test") # create new message object, do not save yet
m3.save # save object to db => true
m3.persisted? # => true
```

### [Associations](https://guides.rubyonrails.org/association_basics.html)

## 6. Generate a controller and add actions
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

## 7. Create a route that maps a URL to the controller action
Open: config/routes.rb

```Ruby
Rails.application.routes.draw do

  get 'messages' => 'messages#index' # 'messages' route targets class 'MessagesController' in file /app/controllers/messages_controller.rb action 'index'
  get 'messages/new' => 'messages#new'
  post 'messages' => 'messages#create'

  resources :messages # maps conventional routes

  mount Billing::Engine, at '/', as: 'billing' # 'billing' route redirected to Billing engine at '/' route
end
```

- `resources :MODEL` - sets up conventional routes, also helpers based on prefixes. [see here](https://guides.rubyonrails.org/getting_started.html#resourceful-routing)
- `resources :MODEL do resources :NESTED end` - nests NESTED in MODEL
- `$ rails routes` - shows all configured routes


## 8. Create a view with HTML and CSS
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
- [Rails Initialization](https://guides.rubyonrails.org/initialization.html) - `bin/rails server`
- Components may define a `table_name_prefix` to prefix their ActiveRecord's database table names
- [Packwerk](https://github.com/Shopify/packwerk) - Ruby gem used to enforce boundaries and modularize Rails applications
- Logs on SPIN for core: `journalctl -o cat -fu proc-shopify--hopify@server.service`

# Troubleshooting
1. When seeing only a blank page after running rails server:
   http://stackoverflow.com/questions/25951969/rails-4-2-and-vagrant-get-a-blank-page-and-nothing-in-the-logs
2. Follow instructions to use postgres:
   https://www.digitalocean.com/community/tutorials/how-to-setup-ruby-on-rails-with-postgres
3. [Rollback](https://edgeguides.rubyonrails.org/active_record_migrations.html#rolling-back)
