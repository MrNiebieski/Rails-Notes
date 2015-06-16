##CRUD##


read receipe

    b[:status]

    t = Tweet.find(3)

this will read in the table tweets

Create:

    t = Tweet.new
    t.status = "update"
    t.save

alternatively:

    t = Tweet.new(
        status: "update",
        others: "nothing")
    t.save


    t = Tweet.create(
        status: "update",
        others: "nothing")

Update:

    t = Tweet.find(3)
    t.status = "New"
    t.save


Delete:

    t = Tweet.find(3)
    t.destroy


advanced:

    Tweet.find(2,3,5)
    Tweet.first
    Tweet.last
    Tweet.all

    Tweet.count
    Tweet.Tweet.where(status: "update")
    Tweet.limit(10)
    Tweet.where(status: "update")


    t = Tweet.find(2)
    t = Tweet.create(
        status: "update",
        others: "nothing")

    Tweet.find(2).destroy
    Tweet.destroy_all

Chaining

    Tweet.where(status: "update").order(:id).limit(10)


    class Tweet < ActiveRecord::Base
        validates_presence_of :status
    end

    validates_numericality_of
    validates_uniqueness_of
    validates_confirmation_of
    validates_acceptance_of (checkbox read agreement)
    validates_length_of :password, minimum: 3
    validates_format_of :email, with /regex/i
    validates_inclusion_of :age, in: 21..99
    validates_exclusion_of :age, in 0...21,
                           message: "sorry must be over 21"

alternatively:

    validates :status, presence: true
    validates :status, length: { minimum: 3 }


    t.errors[:status][0]


Relationshiops:

    has_many :tweets


on the other side:

    belongs_to :<singular>


find and create an instance, the mapping will be taken care of by the rails



<% yield %>
<% link_to , %>


will get _path

don't need to 


layout contains templates

<% tweets = Tweet.all %>
<% if tweets.size == 0 %>
<%end %>

method: :delete

`twwets_controller.rb`

convention over configuration.


    class TweetsController < ApplicationController
        def show
        end
    end


`/app/views/tweets/show.html.erb`


`@tweet` instant variable (scope)


    render action: 'status'

->`/app/views/tweets/status.html.erb`

`params = {id: "1"}`

`Tweet.find(params[:id])`

`/tweets?tweet[status]=hello`

    @tweet = Tweet.create(status: params[:status])
    @tweet = Tweet.create(status: params[:tweet][:status])
    @tweet = Tweet.create(params[:tweet])

_Strong Parameters_ (Rails 4)

required only 

    require(:tweet)
    permit(:status)

    @tweet = Tweet.create(params.require(:tweet).permit(:status))

send in an array

    params.require(:tweet).permit([:status, :location])


    class TweetsController < ApplicationController
        def show
            @tweet = Tweet.find(params[:id])
            respond_to do |format|
                format.html  #show.html.erb
                format.json { render json: @tweet}
        end
    end


    if session[:user_id] !=  @tweet.user_id
        flash[:notice] = "sorry, you can't edit this"
        redirect_to(tweets_path)
    end


    class TweetsController < ApplicationController
      def create
        @tweet = Tweet.create(tweet_params)
        redirect_to tweet_path(@tweet)
      end
      private
      def tweet_params
        params.require(:tweet).permit(:name, :status)
      end

`flash` is a helper

alternatively combine `flash` and `redirect_to`

    redirect_to(tweets_pah,
        notice: "sorry, you can't edit this")

to show the `flash`, in `/app/views/layouts/application.html.erb`

    <% if flash[:notice] %>
        <div id="notice"><%= flash[:notice] %></div>
    <% end %>
    <% end %>
    <%= yield %>


all `edit`, `update` and `destroy` need authorization

    class TweetsController < ApplicationController
        before_action :get_tweet, only:[:edit, :update, :destroy]
        before_action :check_auth, only:[:edit, :update, :destroy]
        def get_tweet
            @tweet = Tweet.find(params[:id])
        end
        def check_auth
            if session[:user_id] !=  @tweet.user_id
                flash[:notice] = "sorry, you can't edit this"
                redirect_to(tweets_path)
            end
        end
        ...


###routes##

in `/config/routes.rb`

    ZombieTwitter::Application.routes.draw do
        resources :tweets
        get '/new_tweet' =>'tweets#new'


in `'tweets#new'`, `tweets` is `Controller` and `new` is `Action`


    get '/new_tweet' =>'tweets#new', as: 'all_tweets'

    <% link_to "All Tweets", all_tweets_path %>


_Redirect_

    get '/all' => redirect('/tweets')


_Root Route_

    root to: "tweets#index"
    <%= link_to "All Tweets", root_path %>


    def index
        if params[:zipcode]
            @tweets = Tweet.where(zipcoe: params[:zipcode])
        else
            @tweets = Tweet.all
        end
        respond_to do |format|
            format.html #index.html.erb
            format.xml {render xml: @tweets}
        end
    end

_Route Parameters_

    get '/local_tweets/:zipcode' => 'tweets#index'

    get '/local_tweets/:zipcode' => 'tweets#index', as: 'local_tweets'

then

    <%= link_to "Tweets in 95112", local_tweets_path(95112) %>

if only html, then don't need write `respond_to`

`Action Pack`

`Active Record` (design pattern) is the `M` in `MVC`

    Post.find_by_title("My Frist Post")

RoR can understand this.

    $rails console

(`irb` with `rails` preloaded)


_authentication_

5 parts

1. model
2. controller
3. views
4. routes
5. logic



    has\_secure\_password

    gem 'bcrypt', '~> 3.1.7'


    class CreateUsers < ActiveRecord::Migration
      def change
        create_table :users do |t|
          t.string :first_name
          t.string :last_name
          t.string :email
          t.string :password_digest
          t.timestamps
        end
      end
    end


    get 'signup' => 'users#new'
    resources :users


placeholder style


    <%= form_for(@user) do |f| %>
      <%= f.text\_field :first_name, :placeholder => "First name" %>
      <%= f.text\_field :last_name, :placeholder => "Last name" %>
      <%= f.email_field :email, :placeholder => "Email" %>
      <%= f.password_field :password, :placeholder => "Password" %>
      <%= f.submit "Create an account", class: "btn-submit" %>
    <% end %>


    private
        def user_params
        params.require(:user).permit(:first_name, :last_name, :email, :password)
    end


    def create 
        @user = User.new(user_params) 
        if @user.save 
            session[:user_id] = @user.id 
            redirect_to '/' 
        else 
            redirect_to '/signup' 
        end 
    end



    <%= form_for(:session, url: login_path) do |f| %> 
      <%= f.email_field :email, :placeholder => "Email" %> 
      <%= f.password_field :password, :placeholder => "Password" %> 
      <%= f.submit "Log in", class: "btn-submit" %>
    <% end %>



    def create
        @user = User.find_by_email(params[:session][:email])
        if @user && @user.authenticate(params[:session][:password])
            session[:user_id] = @user.id
            redirect_to '/'
        else
            redirect_to 'login'
        end 
    end

    def destroy 
        session[:user_id] = nil 
        redirect_to '/' 
    end


this is in `app/controllers/application_controller.rb`


    helper_method :current_user 

    def current_user 
      @current_user ||= User.find(session[:user_id]) if session[:user_id] 
    end

    def require_user 
        redirect_to '/login' unless current_user 
    end

then in other `controller`

    before_action :require_user, only: [:index, :show]

in `app/views/layouts/application.html.erb`

