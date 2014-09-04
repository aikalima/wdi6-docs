Nodejs proxy server
--

purpose: build a server froms scratch that does one particular thing.

Task: Buils a proxy server. A server that acts like a stand in for a service.

Wikipedia: proxy server

Draw diagram …



How to start:
- think about a few modules that you'll need:
**Google nodejs modules**
http://nodejs.org/api/modules.html#modules_modules

- basic notion of web / routing **Google: node js routing**
- configuration **Google: nodejs config**
- URL resolution and parsing **Google: node js parsing**
- Parsing html pages **Google: nodejs nokogiri** 

**require all thise in proxy - now how to load … **

1) **Google: what's the gemfile in nodejs** fourth from top:
http://stackoverflow.com/questions/4871932/using-npm-to-install-or-update-required-packages-just-like-bundler-for-rubygems

```/package.json```

1) Let's get basic hello world up

```
# Module dependencies.

#http://expressjs.com/
express = require 'express'

#https://github.com/flatiron/nconf
nconf = require 'nconf'

#http://nodejs.org/api/url.html
url = require 'url'

#http://docs.nodejitsu.com/articles/HTTP/clients/how-to-create-a-HTTP-request
https = require "https"

#utilities for dealing with query strings.
querystring = require 'querystring';

#http://stackoverflow.com/questions/7977945/html-parser-on-nodejs
cheerio = require 'cheerio'

# Standard JS
_ = require 'underscore'
require 'sugar' # extends prototypes globally

#http://nodejs.org/api/modules.html
app = module.exports = express()

app.listen(3000);
console.log('Listening on port 3000');

app.get '/hello.txt', (req, res) ->
  res.send 'Hello World'
  
```
END of _1
--

START _2
--

2) Let's start a basic conf

```/config.json```

```
{
    "proxy": {
        "port": 4000
    }
    , "airbnb": {
        "host": "www.airbnb.com"
        , "port": 443
    }
}
```

**add config()**

```
  # read in environment or command-line arguments first
  #  - priority is given to the first entry found, i.e. args > env > environment.config > config
  # environment options can be set like this:
  #   set/export proxy:port=12345
  #
  # argument options can be set like this:
  #  node app.js --proxy:port=12345

  nconf.argv().env()
  nconf.add 'default-file', {type: 'file', file: "config.json"}

  # Test config
  console.log nconf.get "airbnb:host"
  console.log nconf.get "airbnb:port"
  console.log nconf.get "proxy:port"
```

Let's make use of conf - configure port …

```
app.configure ->
  # read in environment or command-line arguments first
  #  - priority is given to the first entry found, i.e. args > env > environment.config > config
  # environment options can be set like this:
  #   set/export proxy:port=12345
  #
  # argument options can be set like this:
  #  node app.js --proxy:port=12345

  nconf.argv().env()
  nconf.add 'default-file', {type: 'file', file: "config.json"}

  # Test config
  console.log nconf.get "airbnb:host"
  console.log nconf.get "airbnb:port"
  console.log nconf.get "proxy:port"

proxy_port = nconf.get "proxy:port"
app.listen proxy_port, ->
  # console.log('Listening on port ' + nconf.get "proxy:port");
  console.log "Airbnb proxy server version 0.8 listening at http://localhost:#{proxy_port}"
  console.log "Airbnb: " +  nconf.get("airbnb:host") + " port:" + nconf.get("airbnb:port")

app.get '/hello.txt', (req, res) ->
  res.send 'Hello World'
```

END of _2
--

START _3
--

**Time to start thinking about our apis**

Let's go to airbnb ..
I want to model their search …

Let's see, we've got:

- location
- check in check out
- number of guests
- perhaps number of search results

What kind of results are we looking for?

- we get Listings
https://www.airbnb.com/s/los-angeles
- we have users
https://www.airbnb.com/users/show/2585715
https://www.airbnb.com/users/show/1579337


```
app.get '/user/:id', (req, res) ->
app.get '/listing/:id', (req, res) ->
app.get '/search/:location', (req, res) ->
app.get '/search/:location/:checkin', (req, res) ->
app.get '/search/:location/:checkin/:checkout', (req, res) ->
```

So let's define a few routes, and test.
How do we do this in express?

http://expressjs.com/api.html - search for routes?
**The app.VERB() methods provide the routing functionality in Express, where VERB is one of the HTTP verbs, such** also look up how to use params …


Here are the stubs …

```
app.get '/hello.txt', (req, res) ->
  res.send 'Hello World'

app.get '/user/:id', (req, res) ->
  res.send 'Hello User:' + req.params.id

app.get '/listing/:id', (req, res) ->
  res.send 'Hello Listing:' + req.params.id

app.get '/search/:location', (req, res) ->
  res.send 'Hello Search:' + req.params.location

app.get '/search/:location/:checkin', (req, res) ->
  res.send 'Hello Search: Loc ' + req.params.checkin + "-" +req.params.location

app.get '/search/:location/:checkin/:checkout', (req, res) ->
  res.send 'Hello Search: Loc/In/Out ' + req.params.location
  + "-" +req.params.checkin
  + "-" +req.params.checkout

```

Let's try to figure out how to implement one call and extrapolate from there …

- Look at airbnb user page
- Find relevant info

Stub out user call:

Get page from airbnb
---

So get page, remember research from earlier:
How to make http request ...
http://docs.nodejitsu.com/articles/HTTP/clients/how-to-create-a-HTTP-request

Let's run with google example:

```
  http.get "http://www.google.com/index.html", (res) ->
    console.log "Got response: " + res.statusCode
    body = ''
    res.on 'data', (chunk) ->
      body += chunk
    res.on 'end', () ->
      console.log 'DONE:' + body
```
      
airbnb version:

```
  https.get "https://"+ nconf.get("airbnb:host") + airbnb_path, (res) ->
    console.log "Got response: " + res.statusCode
    body = ''
    res.on 'data', (chunk) ->
      body += chunk
    res.on 'end', () ->
      console.log 'DONE:' + body
```

Let's move this out in a separate function:

Problem: Need to let caller know that fetch is complete, pass callback to call upon completion:

Call:

```
  app.local.fetch airbnb_path, (result) ->
    console.log result
```


Function:

```

app.local = app.local || {}

app.local.fetch = (airbnb_path, callback) ->
  req = https.get "https://"+ nconf.get("airbnb:host") + airbnb_path, (res) ->
    console.log "Got response: " + res.statusCode
    body = ''
    res.on 'data', (chunk) ->
      body += chunk
    res.on 'end', () ->
      callback body
```

Process page -> grab relevant info
--

Look at cheerio example on cheerio github

```
var cheerio = require('cheerio'),
    $ = cheerio.load('<h2 class="title">Hello world</h2>');

$('h2.title').text('Hello there!');
$('h2').addClass('welcome');

$.html();
//=> <h2 class="title welcome">Hello there!</h2>
```

Here's the real code:


```
Now process, i.e do nokogiri type stuff -> cheerio ..

    $ = cheerio.load(body)
    name= $("meta[property='og:title']").attr("content")
    description= $("meta[property='og:description']").attr("content")
    image= $("meta[property='og:image']").attr("content")
    url= $("meta[property='og:url']").attr("content")

    console.log description
    console.log name
    console.log url
    console.log image  
    
    res.send(JSON.stringify "OK", 200)
      
```

Turn into Json and return to caller
--

Introduce user object ..

```
    user =
      name: $("meta[property='og:title']").attr("content")
      description: $("meta[property='og:description']").attr("content")
      image: $("meta[property='og:image']").attr("content")
      url: $("meta[property='og:url']").attr("content")   
      
      res.send(JSON.stringify user, 200)

```
End of _3
--

    
That's a useful abstraction. Not turn user into an object

Here's User.coffee

```
http://stackoverflow.com/questions/7977945/html-parser-on-nodejs
cheerio = require 'cheerio'

init = ( body) ->
  $ = cheerio.load(body)

  user =
    name: $("meta[property='og:title']").attr("content")
    description: $("meta[property='og:description']").attr("content")
    image: $("meta[property='og:image']").attr("content")
    url: $("meta[property='og:url']").attr("content")

module.exports.init = init
```

Call in route
```
app.local.fetch User, airbnb_path, (user) ->
	res.send(JSON.stringify user, 200)
```

fetch
```
app.local.fetch = (entity, airbnb_path, callback) ->
  req = https.get "https://"+ nconf.get("airbnb:host") + airbnb_path, (res) ->
    body = ''
    res.on 'data', (chunk) ->
      body += chunk
    res.on 'end', () ->
      callback entity.init(body)
```

End of _4
--

Now let's apply to listing

**Easy**

```
#http://stackoverflow.com/questions/7977945/html-parser-on-nodejs
cheerio = require 'cheerio'

init = (body) ->
  $ = cheerio.load(body)

  hostingIdTag = body.match(/hostingId.*/g)
  if hostingIdTag == null
    return

  hostingId = new String(hostingIdTag).match(/\d+/g)[0]

  listing =
    hostingId: hostingId
    user: $(".name").children()[0]?.attribs.href.replace(/\/users\/show\//g, "")
    displayAddress: $("#display_address").text()
    nightlyPrice: new String(body.match(/nightlyPrice.*/g)).match(/\d+/g)[0]
    weeklyPrice: new String(body.match(/weeklyPrice.*/g)).match(/\d+/g)[0]
    monthlyPrice: new String(body.match(/monthlyPrice.*/g)).match(/\d+/g)[0]
    title: $("meta[property='og:title']").attr("content")
    description: $("meta[property='og:description']").attr("content")
    image: $("meta[property='og:image']").attr("content")
    zip: $("meta[property='airbedandbreakfast:postal-code']").attr("content")
    locality: $("meta[property='airbedandbreakfast:locality']").attr("content")
    region: $("meta[property='airbedandbreakfast:region']").attr("content")
    country: $("meta[property='airbedandbreakfast:country-name']").attr("content")
    city: $("meta[property='airbedandbreakfast:city']").attr("content")
    rating: $("meta[property='airbedandbreakfast:rating']").attr("content")
    latitude: $("meta[property='airbedandbreakfast:location:latitude']").attr("content")
    longitude: $("meta[property='airbedandbreakfast:location:longitude']").attr("content")
    fbId: $("meta[property='fb:app_id']").attr("content")
    roomType: $("#description_details li .value")[0]?.children[0].data
    bedType: $("#description_details li .value")[1]?.children[0].data
    sleeps: $("#description_details li .value")[2]?.children[0].data
    bedrooms: $("#description_details li .value")[3]?.children[0].data
    bathrooms: $("#description_details li .value")[4]?.children[0].data
    extraPeople: $("#description_details li .value #extra_people_price_string")[0]?.children[0]?.data
    #cleaningFee: new String($("#description_details li .value #cleaning_fee_price_string")[0]?.children[0]?.data).match(/\d+/g)?[0]
    searchQuery: $("#description_details li .value")[10]?.children[0].attribs?.href
    petOwner: $("#description_details li .value")[12]?.children[0].data

module.exports.init = init

```
End of _4
--


Now on to search
--

Talk about query defaults, to use in absence of not supplied values

Start with most complex search ..

```
app.get '/search/:location/:checkin/:checkout', (req, res) ->
  app.local.run_search req,res
  
```

```

query_defaults =
  checkin: Date.create('tomorrow')
  guests: 2
  length_of_stay = 3
  resultCount:25

AirbnbDate =
  create: (mmddyyyy) ->
    year = mmddyyyy.substring 4,8
    mm = mmddyyyy.substring 0,2
    dd = mmddyyyy.substring 2,4
    new Date('' + year + '-' + mm + '-' + dd)
    
..
test with: localhost:4000/search/london/02152013/02192013
app.get '/search/:location/:checkin/:checkout', (req, res) ->
  app.local.run_search req,res


..

app.local.run_search = (req, res) ->
  location = req.params.location.replace ' ','-' #airbnb search uses '-' for white space
  checkin = req.params.checkin
  checkout = req.params.checkout
  qString = querystring.parse (url.parse req.url).query
  guests = qString.guests
  guests = query_defaults.guests unless guests?
  count = qString.resultCount
  count = query_defaults.resultCount unless count?

  airbnb_path = '/s/'+location+'?checkin='+checkin+'&checkout='+checkout+'&guests='+guests+'&per_page='+count
  app.local.fetch SearchResult, airbnb_path, (result) ->
    res.send(JSON.stringify result, 200)
```

End of _5
--  

The rest is bringing other searches online … see finale version

...






Lab/HW - build an Angular app talking to your api. Map results


airbnb api
save results in mongo db

Angular events:


Differnces:

JS / Ruby
Node / Rails
