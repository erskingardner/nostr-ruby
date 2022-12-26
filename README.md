# Nostr Ruby

A ruby library to interact with the [Nostr Protocol](https://github.com/nostr-protocol/nostr).

---

**Note**: this is a first proof of concept version, the API will probably change in the near future.

---

## Manage the keys
```ruby
require "./nostr-ruby.rb"

n = Nostr.new({private_key: "964b29795d621cdacf05fd94fb23206c88742db1fa50b34d7545f3a2221d8124"})
# <Nostr:0x00000001063ffa28 @private_key="964b29795d621cdacf05fd94fb23206c88742db1fa50b34d7545f3a2221d8124" @public_key="da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81">

n.bech32_keys
# => {:public_key=>"npub1mg2nzunrsk9df94zr3uudhzltnu6lzq2muax09xmhu5gxxrvnkqsvpjg3p", :private_key=>"nsec1je9jj72avgwd4nc9lk20kgeqdjy8gtd3lfgtxnt4ghe6ygsasyjq7kh6c4"}

n = Nostr.new({private_key: "nsec1je9jj72avgwd4nc9lk20kgeqdjy8gtd3lfgtxnt4ghe6ygsasyjq7kh6c4"})
# => #<Nostr:0x00000001060952c0 @private_key="964b29795d621cdacf05fd94fb23206c88742db1fa50b34d7545f3a2221d8124", @public_key="da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81">

n.keys
# => {:public_key=>"da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81", :private_key=>"964b29795d621cdacf05fd94fb23206c88742db1fa50b34d7545f3a2221d8124"}
```

## Set the user metadata
```ruby
metadata = n.build_metadata_event("Mr Robot", "I walk around the city", "https://upload.wikimedia.org/wikipedia/commons/3/35/Mr_robot_photo.jpg", "mrrobot@mrrobot.com")
#["EVENT",
# {:pubkey=>"9be59510fa12b77340bb57e555bac716455fedf46d1a354185d4e72bd0340b6f",
#  :created_at=>1671546067,
#  :kind=>0,
#  :tags=>[],
#  :content=>"{\"name\":\"Mr Robot\",\"about\":\"I walk around the city\",\"picture\":\"https://upload.wikimedia.org/wikipedia/commons/3/35/Mr_robot_photo.jpg\",\"nip05\":\"mrrobot@mrrobot.com\"}",
#  "id"=>"3bd77596ea999dde26689c24370dc4adfa66c33abf1b4c23bf863a516106cda2",
#  "sig"=>"2ff752e9f3ed824e7677c41c73728315f0532f3437857774d7a50a577563f391785afd1f84bef3e3574939b14cf096380d4790375953c793504ffcf2f0467d69"}]
```

## Create a post
```ruby
note = n.build_note_event("Hello Nostr!")
# =>
# ["EVENT",
#  {:pubkey=>"da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81",
#   :created_at=>1671406583,
#   :kind=>1,
#   :tags=>[],
#   :content=>"Hello Nostr!",
#   "id"=>"23411895658d374ec922adf774a70172290b2c738ae67815bd8945e5d8fff3bb",
#   "sig"=>"871177b77840bdf092dabacf98c47690647fd6ceb3cc79dd7af7e98c6aded0b808abd5566e2864bd438364cea2f17bd6f9d55091b3c5136839cf160beca42b63"}]
```

## Create a channel post
```ruby
channel_note = n.build_note_event("Welcome on my channel :)", "136b0b99eff742e0939799417d04d8b48049672beb6d8110ce6b0fc978cd67a1")
# =>
# ["EVENT",
#  {:pubkey=>"da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81",
#   :created_at=>1671406522,
#   :kind=>42,
#   :tags=>[["e", "136b0b99eff742e0939799417d04d8b48049672beb6d8110ce6b0fc978cd67a1"]],
#   :content=>"Welcome on my channel :)",
#   "id"=>"96ac317516e9cc3bae8238cf11a95a2f12d1bd2f6553c0867d47f3165ca3483b",
#   "sig"=>"ccb6cbfa5c3cfac7b7f48dd9cda25d6a2493cbf8df91fa8f9fee2559a20c92613326a319f5b76aff9fef85278e04ce0ee78e636afb4ef2bb000ee8a6fdf418d2"}]
```

## Recommend a relay
```ruby
recommendation = n.build_recommended_relay_event("wss://relay.damus.io")
# =>
# ["EVENT",
#  {:pubkey=>"d0fbe5e40469bba810ecb9e1b0b6c13370592df161655e81497c2eb69d0d5bef",
#   :created_at=>1672079256,
#   :kind=>2,
#   :tags=>[],
#   :content=>"wss://relay.damus.io",
#   "id"=>"1842c9feb3bf2ad7095c8a51238f598fa028116d4fd919af22ad2c63ba3b7d69",
#   "sig"=>"9c2f158f379b2d234fd0d363b46a7f90c25392f9296111b6cc04224df8aec69817fa62d7225c12b90fdc31eb89c7afaa427b18147cc8ad6cd411b47dda1331b6"}]
```

## Create a private message
```ruby
private_message = n.build_dm_event("Hello!", "da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81") # To myself
# =>
# ["EVENT",
#  {:pubkey=>"da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81",
#   :created_at=>1671406025,
#   :kind=>4,
#   :tags=>[["p", "da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81"]],
#   :content=>"AIZ7vomEJEFgB934gWzlNA==?iv=mwKLb6lZSG5X1y1BNYv6dg==",
#   "id"=>"6a3efcf47a31bb05aeca0b13bf1f9b9e91b126e0a67e783253fb3bae20f0dc63",
#   "sig"=>"0e390bb3c783157b3e32c3f6641fb40df9f62e326ac8a3448a70c94103b909e80292a2c4530562298f2c3935899111843a43548185abf09abf583bc3e6e3ddde"}]
```

## Decrypt a private message
```ruby
# Get the reply from the relay
reply
# => ["EVENT","0.27631406274260906",{"tags":[["p","da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81"]],"content":"vWATHf2l/KI7RSyVlbxpvufZc+Lui/0oDysTyfEG5vs=?iv=G4SG7ArMGkglX0UJbJBDUA==","sig":"fe816b86579f5d13ab23c88410364442a9b4393ac0d74f4642cd51a1887f04908ff57ef60409d529a6939c50d77b048320417005460b0353c8f990d1b35c3661","id":"5a0274f33cdb064136c1423ac4b096d7f1e3fb36f60404ef2449ee44331de03f","kind":4,"pubkey":"da15317263858ad496a21c79c6dc5f5cf9af880adf3a6794dbbf2883186c9d81","created_at":1671407520}]

n.decrypt_dm(reply)
# => "Nice to meet you!"

```