# Docker Client

Docker client library to access the Docker remote API.

## Installation

Add this line to your application's Gemfile:

    gem 'docker-client'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install docker-client

## Usage

**Ready to be used with Docker version [0.4.0](http://get.docker.io/builds/Linux/x86_64/docker-latest.tgz).**

So far only the [containers](http://docs.docker.io/en/latest/api/docker_remote_api.html#containers) resource is supported. The images resource and endpoints in category misc according to the Docker [Remote API documentation](http://docs.docker.io/en/latest/api/docker_remote_api.html) are not yet implemented (wip: see [pull request](https://github.com/geku/docker-client/pull/1))


````ruby
require 'docker'
require 'awesome_print'

docker = Docker::API.new(base_url: 'http://localhost:4243')
containers = docker.containers

# Create a new container
result = containers.create(['/bin/sh', '-c', 'while true; do echo hello world; sleep 1; done'], 'base')
container_id = result["Id"]
ap result

# Start created container
containers.start(container_id)

# Get container details (inspect)
details = containers.show(container_id)
ap details

# Get file system changes of container
changes = containers.changes(container_id)
ap changes

# Attach to container for 3 seconds
options = {stdout: true, stderr: false}
containers.attach(container_id, options, 3) do |data|
  puts ">> #{data}"
end

# Get all output since container is started
output = containers.logs(container_id)
ap output

# List all running containers
running_containers = containers.list
ap running_containers

# Stop container
containers.stop(container_id)

# Remove container
containers.remove(container_id)

````

## Development

### Run tests

All tests are stubbed with VCR. You can edit the setting `config.default_cassette_options` in `spec_helper.rb` to run the tests against the docker API. Set it to `{:record => :all}`. This will  alos re-record all VCR request/response. To run the tests stubbed again uncomment the before mentioned setting.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request


## License

MIT License. Copyright 2013 Georg Kunz.

