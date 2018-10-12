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

Song::Create.call(whatever: "", in: "here")
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
reulst = Song::Create.()
result.success? # => true
result = Song::Create.()
result.success? # => false

class Song::Create < Trailblazer::Operation
  step Model( Song, :new )
  step Contract::Build()
  step Contract::Validate()
  step Contract::Persist()
end
result = Song::Create.( title: "", length: 13 )
result.success? # => true
result[] # => #<>

step Persist( method: :sync )

class Song::Create < trailblazer::Operation
end

result = Song::Create.({})
result[].errors.message # =>
result = Create.()
result[].success? # =>
result[].errors # =>
result[].errors.messages # =>

class Song < ActiveRecord::Base
end

class Song::Policy
end

class Song::Create < Trailblazer::Operation
end

class SongsController < ApplicationController
end

class Song::Create < Trailblazer::Operation
end

describe Song::Update do
  let(:song){ Song::Create.(  ) }
end



```

```
= simple_form_for @form do |f|
  = f.input :body

gem ""
gem ""
gem ""
```


