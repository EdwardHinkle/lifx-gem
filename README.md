# LIFX

This gem allows you to control your [LIFX](http://lifx.co) lights.

It handles discovery, gateway connections, tags, and provides a object-based API
for talking to Lights.

Due to the nature of the current protocol, some methods are asynchronous.

This gem is in an early beta state. Expect breaking API changes.

## Hello private beta testers!

Welcome to the private beta. I'd love some feedback on all aspects of using this gem. API, documentation, examples, bugs, etc etc. Please file an issue or hit me (@chendo) up on Twitter/IRC.

## Requirements

* Ruby 2.1.1
* Bundler

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'lifx', git: "git@github.com:LIFX/lifx-gem.git"
```

And then execute:

```shell
$ bundle
```

## Usage

```ruby
client = LIFX::Client.lan                  # Talk to bulbs on the LAN
client.discover! do |c|                    # Discover lights. Blocks until a light with the label 'Office' is found
  c.lights.with_label('Office')
end
                                           # Blocks for a default of 10 seconds or until a light is found
client.lights.turn_on                      # Tell all lights to turn on
light = client.lights.with_label('Office') # Get light with label 'Office'

# Set the Office light to pale green over 5 seconds
green = LIFX::Color.green(saturation: 0.5)
light.set_color(green, duration: 5)        # Light#set_color is asynchronous

sleep 5                                    # Wait for light to finish changing
light.set_label('My Office')

light.add_tag('Offices')                   # Add tag to light

client.lights.with_tag('Offices').turn_off

client.flush                               # Wait until all the packets have been sent
```

## Documentation

Documentation is available at http://rubydoc.info/gems/lifx. Please note that undocumented classes/methods and methods marked private are not intended for public use.

## Examples

Examples are located in the `examples/` folder.

* [travis-build-light](examples/travis-build-light/build-light.rb): Changes the colour of a light based on the build status of a project on Travis
* [auto-off](examples/auto-off/auto-off.rb): Turns a light off after X seconds of it being detected turned on.
* [identify](examples/identify/identify.rb): Use divide-and-conquer search algorithm to identify a light visually.

## Testing

Run with `bundle exec rspec`.

The integration specs rely on a least one device tagged with `Test` to function. At this point, they're semi-unreliable due to the async nature of the protocol, and there's not much coverage at the moment as the architecture is still in flux.

A more comprehensive test suite is in the works.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
