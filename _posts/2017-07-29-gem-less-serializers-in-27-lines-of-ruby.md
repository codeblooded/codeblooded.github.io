---
layout: post
title: "Gem-less Serializers in 27 Lines of Ruby"
date:   2017-07-29 00:00:00 -0600
---

So, I love the active_model_serializers gem, but I encountered a really weird bug lately. I’m hoping it’s resolved in a future release. However, I’m having to migrate my project to another option in the meantime. I explored jbuilder, but I was disappointed by its lack of support for efficient layout rendering. The contributors can’t fix it without breaking backwards compatibility. Then, I looked at RABL, but the DSL seemed a little excessive for my needs. So, I looked back and decided that I really like serializers.

Could a developer write a magic-less serializer from scratch? Well, it turns out you can. As a matter of fact, it’s only 27 lines of ruby and requires no external gems. It also works with any hash-able object in ruby. Doesn't even require it to be an ActiveModel.

```ruby
require 'json'

class Serializer
  def initialize(subject)
    @subject = subject
    @attrs = {}
    @sanitized_hash = {}
  end

  def as_hash
    @attrs.map do |k, v|
      @sanitized_hash[k] = @subject[k] if v
    end

    @sanitized_hash
  end

  def as_json
    as_hash.to_json
  end

  protected

  def expose(*atts, iff: true)
      atts.each do |att|
        @attrs[att] = iff
      end
  end
end
```

Then, to create a specific serializer, simply declare a subclass and mark the properties to expose in the constructor.

```ruby
class UserSerializer < Serializer
  def initialize(subject)
    super(subject)
    expose :id
    expose :name
    expose :auth_token, :auth_token_expiration
  end
end
```

It supports conditional serialization as well:
```ruby
def show_auth_token_expiration?
  false
end

class UserSerializer < Serializer
  def initialize(subject)
    super(subject)
    expose :id, :name, :auth_token
    expose :auth_token_expiration, iff: show_auth_token_expiration?
  end
end
```

And here is an example of it being used:

```ruby
render json: UserSerializer.new({
  id: 1,
  name: 'Hello',
  auth_token: '2323',
  auth_token_expiration: 'Dec 14, 1995',
  password: 'fooboo'
}).as_json
```

And it successfully does not serialize the attributes that aren't exposed or who's conditions fail:

```
{"id":1, "name": "Hello", "auth_token": "2323"}
```

It's not very feature complete, but it works. It meets the needs for my project. Would love some feedback as I may publish the Serializer class in a gem.
