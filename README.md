# CF Redis Example App [![Build Status](https://travis-ci.org/pivotal-cf/cf-redis-example-app.svg)](https://travis-ci.org/pivotal-cf/cf-redis-example-app)

This fork is an example of how you can consume a Redis service deployed to CF Diego from the Docker container.

It allows you to set, get and delete Redis key/value pairs using RESTful endpoints.

### Getting Started

Install the app by pushing it to your Cloud Foundry and binding with the user-provided Redis service

Example:

     $ git clone git@github.com:ldmberman/cf-redis-example-app.git
     $ cd redis-example-app
     $ cf push redis-example-app --no-start
     $ cf docker-push redis ldmberman/redis-custom
     
Find the app's host-port:

     $ cf app redis --guid
     $ curl -X GET receptor.<cf_domain>/v1/actual_lrps  # find the host and port in the output using your guid
     $ cf create-user-provided-service redis -p '{"port":"<port>","host":"<host>"}'
     $ cf bind-service redis-example-app redis

Then it is neccessary to allow the inner traffic to Redis instance by adding [a security group](https://github.com/cloudfoundry/cf-mysql-release#security-groups).

### Endpoints

#### PUT /:key

Sets the value stored in Redis at the specified key to a value posted in the 'data' field. Example:

    $ export APP=redis-example-app.my-cloud-foundry.com
    $ curl -X PUT $APP/foo -d 'data=bar'
    success


#### GET /:key

Returns the value stored in Redis at by the key specified by the path. Example:

    $ curl -X GET $APP/foo
    bar

#### DELETE /:key

Deletes a Redis key spcified by the path. Example:

    $ curl -X DELETE $APP/foo
    success

