##associations##

    class User < ActiveRecord::Base
      has_many :posts
    end

        class Post < ActiveRecord::Base
      belongs_to :user
    end

then use `User.first.posts` and `Post.first.user`


two types of `Users`

    class Post < ActiveRecord::Base
      belongs_to :author, :class_name => "User"
      belongs_to :editor, :class_name => "User"
    end


to use `join`

    class Author < ActiveRecord::Base
      has_many :authorships
      has_many :books, :through => :authorships
    end

    class Authorship < ActiveRecord::Base
      belongs_to :author
      belongs_to :book
    end

    class Book < ActiveRecord::Base
      has_one :authorship
    end

    @author = Author.find :first
    # selects all books that the author's authorships belong to.
    @author.authorships.collect { |a| a.book }
    selects all books by using the Authorship join model
    @author.books 