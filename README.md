### trailblazer
---

https://github.com/trailblazer/trailblazer


```
app
|--concepts
|  |-- song
|  |  |--operation
|  |  |  |--create.rb
|  |  |  |--update.rb
|  |  |--contract
|  |  |  |-- create.rb
|  |  |  |-- update.rb
|  |  |--cell
|  |  |  |-- show.rb
|  |  |  |-- index.rb
|  |  |--view
|  |  |  |-- show.haml
|  |  |  |-- index.rb
|  |  |  |-- song.css.sass

```

```ruby
Rails.application.routes.draw do
  resources :songs
end

class SongsController < ApplicationController
  def create
    run Song::Create
  end
end

class SongsController < ApplicationController
  def create
    run Song::Create do |op|
      return redirect_to(song_path op.model)
    end
    render :new
  end
end

class Song::Create < Traiblazer::Operation
  step :process!
  def process!(options)
  end
end

Song::Create.call(whatever: "goes", in: "here")
Song::Create.(whatever: "goes", in: "here")

# app/concepts/song/contract/create.rb
module Song::Contract
  class Create < Reform::Form
    property :title
    property :length
    validates :title, length: 2..33
    validates :length, numericality: true
  end
end

# app/concepts/song/operation/create.rb
class Song::Create < Trailblazer::Operation
  step Model( Song, :new )
  step Contract::Build( constant: Song::Contract::Create )
  step Contract::Validate()
  step Contract::Persist()
end

result = Song::Create.(title: "A" )
result.success? # => false
result["contract.default"].errors.messages
# => {:title=>["is too short (minimum is 2 characters)"], :length=>["is not a number"]}

class Song::New < Trailblazer::Operation
  step Model( Song, :new )
  step COntract::Build( constant: Song:Contract::Create )
end

resule = Song::New.()
result["model"]
result["contract.default"]

class Song::ValidateOnly < Trailblazer::Operation
  step Model( Song, :new )
  step Contract::Build( constant: Song::Contract::Create)
  step Contract::Validate()
end

result = Song::ValidateOnly.({})
result.success? # => false

result = Song:ValidateOnly.({ title: "Rising Force", length: 13 })
result.success? # => true
result["model"] # => #<struct Song title=nil, length=nil>
result["contract.default"].title 

class Song::Create < Traiblazer::Operation
  step Model( Song, :new )
  step Contract::Build( constant: Song::Contract::Create )
  step Contract::Validate( key: "song" )
  step Contract::Persist( )
end
reulst = Song::Create.({ "song" => { title: "Rising Force", length: 13 } })
result.success? # => true
result = Song::Create.({ title: "Rising Force", length: 13 })
result.success? # => false

class Song::Create < Trailblazer::Operation
  step Model( Song, :new )
  step Contract::Build( constant: Song::Contract::Create )
  step Contract::Validate()
  step Contract::Persist()
end
result = Song::Create.( title: "Rising Force", length: 13 )
result.success? # => true
result["model"] # => #<>

step Persist( method: :sync )

class Song::Create < trailblazer::Operation
  step Model( Song, :new )
  step Contract::Build( name: "form", constant: Song::Contract::Create )
  step Contract::Validate( name: "form" )
  step Contract::Persist( name: "form" )
end

result = Song::Create.({ title: "A" })
result[].errors.message # => {:title=>["is too short (minimum is 2 ch...)"]}
result = Create.({ length: "A" })
result["result.contract.default"].success? # => false
result["result.contract.default"].errors # => Errors object
result["result.contract.default"].errors.messages # => {:length=>["is not a number"]}

class Song < ActiveRecord::Base
  belongs_to :thing
  scope :latest, lambda { all.limit(9).order("id DESC") }
end

class Song::Policy
  def initialize(user, song)
    @user, @song = user, song
  end
  def create?
    @user.admin?
  end
end

class Song::Create < Trailblazer::Operation
  step Policy( Song::Policy, :create? )
end

class SongsController < ApplicationController
  def new
    form Song::Create
  end
end

class Song::Create < Trailblazer::Operation
  representer do
    include Roar::JSON::HAL
    link(:self) { song_path(represeented.id) }
  end
end

describe Song::Update do
  let(:song){ Song::Create.(song: {body: "[That](http://traiblazer.to)!"}) }
end



```

```
= simple_form_for @form do |f|
  = f.input :body

gem "traiblazer"
gem "traiblazer-rails"
gem "trailblazer-cells"
```


