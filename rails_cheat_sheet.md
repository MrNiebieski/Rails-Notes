

    sudo gem install rails --include-dependecies

    rake doc:app



    class Subjects < ActiveRecord::Migration
       def self.up
           create_table :subjects do |t|
              t.string :name
           end
           Subject.create :name => "Physics"
           Subject.create :name => "Mathematics"
           Subject.create :name => "Chemistry"
           Subject.create :name => "Psychology"
           Subject.create :name => "Geography"
      end
      def self.down
          drop_table :subjects
      end
    end

    ruby script/generate controller Book

vs.

    rails generate controller Pages



    <% if @books.blank? %>
    <p>There are not any books currently in the system.</p>
    <% else %>
    <p>These are the current books in our system</p>
    <ul id="books">
    <% @books.each do |c| %>
    <li><%= link_to c.title, {:action => 'show', :id => c.id} -%></li>
    <% end %>
    </ul>
    <% end %>
    <p><%= link_to "Add new Book", {:action => 'new' }%></p>


form

    <h1>Add new book</h1>
    <% form_tag :action => 'create'  do %>
    <p><label for="book_title">Title</label>:
    <%= text_field 'book', 'title' %></p>
    <p><label for="book_price">Price</label>:
    <%= text_field 'book', 'price' %></p>
    <p><label for="book_subject">Subject</label>:
    <%= collection_select(:book,:subject_id,@subjects,:id,:name) %></p>
    <p><label for="book_description">Description</label><br/>
    <%= text_area 'book', 'description' %></p>
    <%= submit_tag "Create" %>
    <% end  %>
    <%= link_to 'Back', {:action => 'list'} %>