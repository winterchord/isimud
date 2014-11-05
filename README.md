# Isimud: Messaging abstraction layer for AMQP and testing.

>Isimud is a minor god, the messenger of the god Enki in Sumerian mythology.
>He is readily identifiable by the fact that he possesses two faces looking in opposite directions.
>
>*Source: Wikipedia*

## Installation

Add this line to your application's Gemfile:

    gem 'isimud'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install isimud
    
For Rails applications, use the following generators to create config and initializer files, respectively:

    $ rails g isimud:config
    $ rails g isimud:initializer
    
Customize the AMQP broker settings in the config/isimud.yml

## Usage

### Connecting to an AMQP server

There are two supported conventions for specifying a RabbitMQ server (broker) in the configuration file:

#### Using a URL

    server: amqp:port//user_name:password@host/vhost

#### Using separate parameters:

[Complete list of Bunny options available here](http://rubybunny.info/articles/connecting.html)

    server:
        host:  hostname
        port:  15672
        user:  user_name
        pass:  password
        vhost: vhost



Isimud is designed to work with [RabbitMQ](http://www.rabbitmq.com).
Besides the standard AMQP 0.9.1 protocol, Isimud relies on Publishing Confirms (Acknowledgements), which
is a RabbitMQ specific extension to AMQP 0.9.1.

Note that Isimud does not automatically create exchanges. Make sure the exchange has been declared on the
message server, or you will get an exception. It is highly recommended to set the /durable/ parameter on the exchange
in order to prevent loss of messages due to failures.

Isimud uses [Bunny](http://rubybunny.info) to connect to RabbitMQ.

### Message publication

Isimud uses topic based exchanges publish messages. This allows for multiple listener
workers to operate in parallel to process messages.

### Message binding and consumption

Isimud uses non-exclusive, durable queues to listen for and consume messages. Named queues are automatically created
if they do not exist.

## Changes

### 0.3.0

* Added rake task for manual synchronization using ModelWatcher

### 0.2.17

* Added guard on null #updated_at instances
* Added ModelWatcher#isimud_sync for manual synchronization

### 0.2.15

* Changed Event#send to Event#publish, to avoid overloading Ruby.

### 0.2.13

* Add :omit_parameters option to Event#as_json

### 0.2.12

* Demodulize ActiveRecord model name when setting ModelWatcher event type

### 0.2.10

* Added Isimud.retry_failures
* Isimud::ModelWatcher now includes :created_at and :updated_at columns by default
* Added Isimud::Client.connected?
* Avoid connecting to database when Isimud::ModelWatcher.watch_attributes is called

### 0.2.4

* Add Isimud::ModelWatcher#isimud_synchronize? to allow conditional synchronization. Override to activate.

### 0.2.2

* Add enable_model_watcher configuration parameter (default is true)

### 0.2.0

* Added Isimud::Event
* Extracted Isimud::Client#log into Isimud::Logging module

### 0.1.4

* Don't reject messages when exception is raised in bind block

### 0.1.3

* Upgrade bunny gem requirement to 1.3.x
* Fixed message acknowledgements
* Added log_level configuration parameter (default is :debug)

### 0.1.2

* Reject message with requeue when an exception is raised during processing

### 0.1.1

* Enable channel confirmations for message publication

### 0.1.0

* ModelWatcher mix-in for ActiveRecord, sends events on instance changes
* Initializer generator for Rails

### 0.0.8 (first working version)

* Don't clear the queues when reconnecting TestClient


## Contributing

1. Fork it ( https://github.com/[my-github-username]/isimud/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
