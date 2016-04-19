(this isn't polished, but should work as described - tested on dokku 4.*)

# Dokku Require

A plugin which allow you to specify plugins and volumes
required by your app.

## Requirements

* dokku 0.4.0+

## Installation

```
# on 0.4.x
dokku plugin:install https://github.com/crisward/dokku-require.git require
```

## Usage

In the root of your app, you need to add an app.json file [as specificed in this heroku spec](https://devcenter.heroku.com/articles/app-json-schema#schema-reference)

With the addition of a dokku section (see below). 

```json
{
  "name": "my website",
  "description": "My website",
  "keywords": [
    "productivity",
    "HTML5"
  ],
  "website": "https://mywebsite.com/",
  "dokku":{
    "plugins":[
      "mariadb","redis"
    ],
    "volumes":[
      {"host":"/var/lib/dokku/data/storage","app":"/storage"}
    ]
  }
}
```

The `"dokku":{"plugins":[]}` array can contain a list of plugin names, which 
by default will be created with the app name and linked to your app.
This will only work with the official plugins which have `create` and `link` methods.
ie.

* [CouchDB (beta)](https://github.com/dokku/dokku-couchdb)                     
* [Elasticsearch (beta)](https://github.com/dokku/dokku-elasticsearch-plugin)  
* [MariaDB (beta)](https://github.com/dokku/dokku-mariadb-plugin)              
* [Memcached (beta)](https://github.com/dokku/dokku-memcached-plugin)          
* [Mongo (beta)](https://github.com/dokku/dokku-mongo-plugin)                  
* [MySQL (beta)](https://github.com/dokku/dokku-mysql-plugin)                  
* [Nats (beta)](https://github.com/dokku/dokku-nats)                           
* [Postgres (beta)](https://github.com/dokku/dokku-postgres-plugin)            
* [RabbitMQ (beta)](https://github.com/dokku/dokku-rabbitmq-plugin)            
* [Redis (beta)](https://github.com/dokku/dokku-redis-plugin)                  
* [RethinkDB (beta)](https://github.com/dokku/dokku-rethinkdb-plugin)   

If you need more control over the creation process or you'd like to use
a non-official plugins the below syntax can be used. Each command in the
array will be run in sequence.

```json
  "dokku":{
    "plugins":[
      {
        "name":"mariadb",
        "commands":["mariadb:create mydb","mariadb:link mydb $APP"]
      }
    ]
  }
```

## Alternative Settings for Development and Testing

It's often neccessary to have different volumes or database configurations
for production, development and testing. To enable this you can set a different 'mode'
for 'dokku require' which overrides the key used within the app.json file.
This allows you to setup mutiple configurations for each of your chosen environments

ie. Development mode could be set with
```
dokku require:mode dokku_dev
```
Then in your `app.json` you would set your development settings under that key.

```json
  "dokku":{
    "plugins":["mariadb","redis"],
    "volumes":[{"host":"/var/lib/dokku/data/storage","app":"/storage"}]
  }
  "dokku_dev":{
    "plugins":["mariadb","redis"],
    "volumes":[
      {"host":"/var/lib/dokku/data/storage","app":"/storage"},
      {"host":"/home/vagrant/myapp/src","app":"/app/src"}
    ]
  }
```

**Note:** this setting is global for all apps on your server, by default it is set to dokku.


## Apps Using this Plugin

As this is very new, I've only created a single app using this. 

* https://github.com/crisward/dokku-ghost - create a ghost blog in dokku


## License

(The MIT License)

Copyright (c) 2015 Cris Ward

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.