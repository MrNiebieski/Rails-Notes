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