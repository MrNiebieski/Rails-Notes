#code that looks interesting#

    rails g scaffold Person name:index email:uniq


    match "/404", :to => "errors#not_found"

in `routes.rb` namespace, how to use it?

resources

what is `Kassi`?

`case`...`when`


    class Worklog < ActiveRecord::Base
      include Redmine::SafeAttributes
      
      unloadable
      belongs_to :author, :class_name => "User", :foreign_key => "user_id"
      has_many :worklog_reviews

      attr_accessor :plan,:plan_done,:week_feel

this is an example of `:foreign_key`

    a = [ "a", "b", "c", "d" ]
    a.collect {|x| x + "!" }   #=> ["a!", "b!", "c!", "d!"]
    a                          #=> ["a", "b", "c", "d"]




    class WorklogsController < ApplicationController
      model_object Worklog
      unloadable

`Topic.count` this works


`active record`:

    Client.find_by first_name: 'Jon'


`? replacement`

    Client.where("orders_count = ? AND locked = ?", params[:orders], false)

`condition`:

    Client.where(locked: true)
    Article.where.not(author: author)

`Range Condition`:

    Client.where(created_at: (Time.now.midnight - 1.day)..Time.now.midnight)


    Client.select("viewable_by, locked")



`has_many :votes, dependent: :destroy` this is important to ensure destroy correctly.

    has_many :votes, dependent: :destroy

