##part 2: TravelApp##

    rails generate model Tag
    rails generate model Destination


in `app/models/tags.rb`

    class Tag < ActiveRecord::Base
        has_many :destinations 
    end

in `app/models/destination.rb`

    class Destination < ActiveRecord::Base
        belongs_to :tag
    end

in `db/migrate/` file, example:

    class CreateDestinations < ActiveRecord::Migration
          def change
            create_table :destinations do |t|
                    t.string :name
                    t.string :image
                    t.string :description
                    t.references :tag
              t.timestamps
            end
          end
    end


allowed basic data types:

* string
* text (longer than text: e.g. description)
* integer
* float
* timestamp & datetime
* date & time
* binary
* boolean


route is written as something like:

      get '/tags'=>'tags#index'
      get '/tags/:id' => 'tags#show', as: :tag

By giving the route in step 1 the name "tag", Rails automatically creates a helper method named tag\_path. We use tag\_path(t)


in `view` write something like:


     <% @tags.each do |tag| %>
        <div class="card col-xs-4">
          <img src="<%= tag.image %>"> 
          <h2><%= tag.title %></h2>
        </div>
    <% end %>


    <%= image_tag t.image %>
    <%= link_to "Learn more", tag_path(t) %>


controller:

    class TagsController < ApplicationController
      def show
        @tag = Tag.find(params[:id])
        @destinations = @tag.destinations
      end
    end

`bundle exec` excute a script in the context of the current bundle.

##Associations##

no foreign key: `has_many`

with foreign key: `belongs_to`

connect models

`has_many: <comments>, dependent: :destroy`

`rails console`


`rake routes`


    validates_presence_of : <name>


rollback to the very begining

    rake db:migrate VERSION=0