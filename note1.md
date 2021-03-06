##getting started##

    rails new MySite
    bundle install
    rails server

default app location is <http://localhost:8000>

Rails router: `config/routes.rb`

    rails generate controller Pages

then go to `app/controllers/pages_controller.rb`

and change it to the following codes:

    class PagesController < ApplicationController
       def home
       end
    end

in `config/routes.rb` add

    get 'welcome' => 'pages#home'

then edit `app/views/pages/home.html.erb`

    <div class="main">
      <div class="container">
        <h1>Hello world!</h1>
      </div>
    </div>

(css file is stored in `app/assets/stylesheets/pages.css.scss`)

Model:

    rails generate model Message

this create two files `app/models/message.rb` and a migration file in `db/migrate/`

in `db/migrate/` file, example:

    class CreateMessages < ActiveRecord::Migration
      def change
        create_table :messages do |t|
          t.text :content
          t.timestamps
        end
      end
    end

then run:

    rake db:migrate
    rake db:seed

`rake db:seed` seeds the database with sample data from `db/seeds.rb`

    rails generate controller Messages

write the following code in `app/controllers/messages_controller.rb`:


    class MessagesController < ApplicationController
           def index
                @messages = Message.all
           end
    end


goto `config/routes.rb` again:

    Rails.application.routes.draw do
        get '/messages' => 'messages#index'
    end

in `app/views/messages/index.html.erb`

    <div class="header">
        <div class="container">
            <img src="http://s3.amazonaws.com/codecademy-content/courses/learn-rails/img/logo-1m.svg">
            <h1>Messenger</h1>
        </div>
    </div>
    <div class="messages">
        <div class="container">
            <% @messages.each do |message| %>
                <div class="message">
                    <p class="content"><%= message.content %></p>
                    <p class="time"><%= message.created_at %></p>
                </div>
            <% end %>
        </div>
    </div>

the result is at <http://localhost:8000/messages>

in `config/routes.rb`:

    get 'messages/new' => 'messages#new'
    post 'messages' => 'messages#create'


in `app/controllers/messages_controller.rb`:

        def new
            @message = Message.new
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

in view file `app/views/messages/new.html.erb` add:

    <%= form_for(@message) do |f| %>
          <div class="field">
            <%= f.label :message %><br>
            <%= f.text_area :content %>
          </div>
          <div class="actions">
            <%= f.submit "Create" %>
          </div>
    <% end %>

in view file `app/views/messages/index.html.erb` add:

    <%= link_to 'New Message', "messages/new" %>

By giving the edit route a name of "edit\_destination", Rails automatically creates a helper method named edit\_destination\_path. Use it to generate a URL to a specific destination's edit path.