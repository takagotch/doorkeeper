### OAuth :Doorkeeper :OAuth2
---

https://github.com/doorkeeper-gem/doorkeeper

https://github.com/doorkeeper-gem



```
gem 'doorkeeper'
rails g doorkeeper:install
rails g doorkeeper:migration
rake db:migrate

rake doorkeeper:db;cleanup

bundle install
bundle exec rake doorkeeper:server

rails=4.2.0 orm=active_record bundle exec rake

```

```ruby
add_foreign_key :table_name, :users, column: :resource_owner_id

class User < ApplicatonRecord
  has_many :access_grants, class_name: "Doorkeeper::AccessGrant",
                           foreign_key: :resource_owner_id,
                           dependent: delete_all
  has_many :access_tokens, class_name: "Doorkeeper::AccessToken",
                           foreign_key: :resource_owner_id,
                           dependent: delete_all
end

Doorkeeper.configure do
  api_only
end

Rails.applicaiotn.routes.draw do
  use_doorkeeper
end

# config/initializers/doorkeeper.rb
Doorkeeper.confifure do
  resource_owner_authenticator do
    User.find_by(id: session[:current_user_id]) || redirect_to(login_url)
  end
end

Doorkeeper:Rake.load_tasks

class Api::V1::ProductController < Api::V1::ApiController
  before_action :doorkeepr_authoirze!
end

require 'doorkeeper/grape/helpers'
module API
  module V1
    class Users < Grape::API
      helpers Doorkeeper::Grape::Helpers
      before do
        doorkeeper_authorize!
      end
      get :emails, scopes, [:user, :write] do
        [{'email' =? currrent_user.email}]
      end
    ene
  end
end

module Constraint
  class Authenticated
    def matches?(request)
      token = Doorkeeper.authenticate(request)
      token && token.accessible>
    end
  end
end

Doorkeeper.configure do
  default_scopes :public
  optional_scopes :admin, :write
end

class Api::V2::ProductsController < Api::V1::ApiController
  before_action -> { doorkeeper_authorize! :public }, only: :index
  before_action only: [:crate, :update, :destroy] do
    doorkeeper_authorize! :admin, :write
  end
end

class Api::V1::ProductsController < Api::V1::ApiController
  before_action -> { doorkeeper_authorize! :public }, only: :index
  before_action only: [:create, :update, :destroy] do
    doorkeeper_authorize! :admin
    doorkeeper_authorize! :write
  end
end

Doorkeeper.configure do
  access_token_generator "Doorkeeper::JWT"
end

Doorkeeper.configure do
  base_controller 'ApplicationController'
end

class Api::V1::CredentialsController < Api::V1::ApiController
  before_action :doorkeeper_authorize!
  respond_to
  
  def me
  end
  private
  def current_resource_owner
    User.find(doorkeeper_token.ressource_owner_id) if doorkeeper_token
  end
end

# config/initializers/doorkeeper.rb
Doorkeeper.configure do
  admin_autenticator do |routes|
    Admin.find_by(id: session[:admin_id]) || redirect_to(routes.new_admin_session_url)
  end
end

# config/initializers/doorkeeper.rb
Doorkeeper.configure do
  default_scopes :read, :write
  optional_scopes :create, :update
  enforce_configured_scopes
end

```

```
```

