## 1. Ruby on Rails Backend Setup

This section covers setting up your Ruby on Rails API backend.

### 1.1 Project Initialization

Open your terminal and navigate to your desired projects directory.

Create a new Rails API application called `backend`:

```jsx
rails new backend --api
```

The `--api` flag configures Rails specifically for API development, saving time on manual controller setup.

Navigate into your new `backend` directory:

```jsx
cd backend
```

**Output after `rails new backend --api`:**

```jsx
PS C:ruby-todo> rails new backend --api
Based on the specified options, the following options will also be activated:

  --skip-javascript [due to --api]
  --skip-hotwire [due to --skip-javascript]
  --skip-asset-pipeline [due to --api]

      create
      create  README.md
      create  Rakefile
      create  .ruby-version
      create  config.ru
      create  .gitignore
      create  .gitattributes
      create  Gemfile
         run  git init from "."
Initialized empty Git repository in C:/Dev/Udemy/ruby-todo/backend/.git/
      create  app
      create  app/assets/stylesheets/application.css
      create  app/controllers/application_controller.rb
      create  app/helpers/application_helper.rb
      create  app/jobs/application_job.rb
      create  app/mailers/application_mailer.rb
      create  app/models/application_record.rb
      create  app/views/layouts/application.html.erb
      create  app/views/layouts/mailer.html.erb
      create  app/views/layouts/mailer.text.erb
      create  app/views/pwa/manifest.json.erb
      create  app/views/pwa/service-worker.js
      create  app/assets/images
      create  app/assets/images/.keep
      create  app/controllers/concerns/.keep
      create  app/models/concerns/.keep
      create  bin
      create  bin/brakeman
      create  bin/dev
      create  bin/rails
      create  bin/rake
      create  bin/rubocop
      create  bin/setup
      create  bin/thrust
      create  Dockerfile
      create  .dockerignore
      create  bin/docker-entrypoint
      create  .rubocop.yml
      create  .github/workflows
      create  .github/workflows/ci.yml
      create  .github/dependabot.yml
      create  config
      create  config/routes.rb
      create  config/application.rb
      create  config/environment.rb
      create  config/cable.yml
      create  config/puma.rb
      create  config/storage.yml
      create  config/environments
      create  config/environments/development.rb
      create  config/environments/production.rb
      create  config/environments/test.rb
      create  config/initializers
      create  config/initializers/assets.rb
      create  config/initializers/content_security_policy.rb
      create  config/initializers/cors.rb
      create  config/initializers/filter_parameter_logging.rb
      create  config/initializers/inflections.rb
      create  config/initializers/new_framework_defaults_8_0.rb
      create  config/locales
      create  config/locales/en.yml
      create  config/master.key
      append  .gitignore
      create  config/boot.rb
      create  config/database.yml
      create  db
      create  db/seeds.rb
      create  lib
      create  lib/tasks
      create  lib/tasks/.keep
      create  log
      create  log/.keep
      create  public
      create  public/400.html
      create  public/404.html
      create  public/406-unsupported-browser.html
      create  public/422.html
      create  public/500.html
      create  public/icon.png
      create  public/icon.svg
      create  public/robots.txt
      create  script
      create  script/.keep
      create  tmp
      create  tmp/.keep
      create  tmp/pids
      create  tmp/pids/.keep
      create  vendor
      create  vendor/.keep
      create  test/fixtures/files
      create  test/fixtures/files/.keep
      create  test/controllers
      create  test/controllers/.keep
      create  test/mailers
      create  test/mailers/.keep
      create  test/models
      create  test/models/.keep
      create  test/helpers
      create  test/helpers/.keep
      create  test/integration
      create  test/integration/.keep
      create  test/test_helper.rb
      create  storage
      create  storage/.keep
      create  tmp/storage
      create  tmp/storage/.keep
      remove  app/assets
      remove  app/helpers
      remove  test/helpers
      remove  app/views/layouts/application.html.erb
      remove  app/views/pwa
      remove  public/400.html
      remove  public/404.html
      remove  public/406-unsupported-browser.html
      remove  public/422.html
      remove  public/500.html
      remove  app/assets/stylesheets/application.css
      remove  config/initializers/content_security_policy.rb
      remove  config/initializers/new_framework_defaults_8_0.rb
         run  bundle install --quiet
WARN: Unresolved or ambiguous specs during Gem::Specification.reset:
      stringio (>= 0)
      Available/installed versions of this gem:
      stringio (>= 0)
      Available/installed versions of this gem:
      - 3.1.7
      - 3.1.1
WARN: Clearing out unresolved specs. Try 'gem cleanup <gem>'
Fetching gem metadata from https://rubygems.org/.........
Fetching gem metadata from https://rubygems.org/.........
Resolving dependencies...
         run  bundle binstubs bundler
         run  bundle binstubs kamal
         run  bundle exec kamal init
Created configuration file in config/deploy.yml
Created .kamal/secrets file
Created sample hooks in .kamal/hooks
       force  .kamal/secrets
       force  config/deploy.yml
       rails  solid_cache:install solid_queue:install solid_cable:install
      create  config/cache.yml
      create  db/cache_schema.rb
        gsub  config/environments/production.rb
      create  config/queue.yml
      create  config/recurring.yml
      create  db/queue_schema.rb
      create  bin/jobs
        gsub  config/environments/production.rb
      create  db/cable_schema.rb
       force  config/cable.yml
```

### 1.2 Configure API Namespace and Routes

You'll define an API namespace to organize your endpoints and create a custom route for updating a todo's completion status.

Open `config/routes.rb`.

Define the API namespace and custom routes:

- Enclose your `resources :todos` declaration within a `namespace :api` block. This automatically prefixes your routes with `/api` (e.g., `/api/todos`).
- Inside the `resources :todos` block, add a `member` block. This `member` block defines routes that operate on a specific member (instance) of the resource, identified by an ID.
- Within the `member` block, create a `patch :update_completed` route. This creates a `PATCH` endpoint like `/api/todos/:id/update_completed` that maps to the `update_completed` action in your `TodosController`.

Your `config/routes.rb` should look exactly like this:

```jsx
Rails.application.routes.draw do
  namespace :api do
    resources :todos do
      member do
        patch :update_completed
      end
    end
  end
end
```

### 1.3 Create the Todos Controller

Next, you need to create the API controller for your `Todo` resource.

**Ruby on Rails Backend: API Todos Controller**

Here's the documentation for your `Api::TodosController`, which handles all the CRUD (Create, Read, Update, Delete) operations for your to-do items. This controller is specifically designed for an API, providing JSON responses.

You will need to create the file `app/controllers/api/todos_controller.rb`.

Ruby

```jsx
# app/controllers/api/todos_controller.rb
class Api::TodosController < ApplicationController
  # This is where your CRUD actions (index, show, create, update, destroy, update_completed) will go.
end
```

**Key Takeaways and Recommendations:**

- **Namespace (`Api::`):** By adding `Api::` to your controller class name and placing it in the `app/controllers/api` directory, you've successfully namespaced your API. This helps keep your API-specific logic separate from any potential regular web controllers you might have.

### 1.4 Database Migration and Testing

To test out your setup:

First, run the database migration to create the `todos` table (assuming you have a migration file generated to create the `todos` table, e.g., by running `rails generate model Todo title:string completed:boolean` previously).

Bash

```jsx
rails db:migrate
```

**Output after `rails db:migrate`:**

```jsx
PS C:ruby-todo\backend> rails db:migrate
== 20250628221506 CreateTodos: migrating ======================================
-- create_table(:todos)
   -> 0.0051s
== 20250628221506 CreateTodos: migrated (0.0057s) =============================
```

Then, start the Rails server:

Bash

```jsx
rails s
```

If you go to `http://[::1]:3000/api/todos` in your browser, you will see `[]` (an empty JSON array), indicating that your API endpoint is correctly configured and returning a response.