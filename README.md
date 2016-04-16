(this is super early - please don't try and use)

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
      {"host":"$APP_DIR/storage/","app":"/app/storage/"}
    ]
  }
}
```

The `"dokku":{"plugins":[]}` array can contain a list of plugin names, which 
by default will be created with the app name and linked to your app.
This will only work the official plugins which have `create` and `link` methods.

If you need more control over the creation process or you'd like to use
a non-official plugins the below syntax can be used. Each command in the
array will be run in sequence.

```json
  "dokku":{
    "plugins":[
      {
        "name":"mariadb",
        "commands":["create mydb","link mydb $APP"]
      }
    ]
  }
```

`$APP` will be replaced with your app name, `$APP_DIR` will be replaced with 
your apps directory ie `/home/dokku/yourapp`
