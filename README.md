# matthiasmullie/php-api example


This repository is just an example of how to implement an API with
[matthiasmullie/php-api](https://github.com/matthiasmullie/php-api). With tests & everything!

Just explore the code! Or keep reading for a very quick overview.


## Code

First make sure to include matthiasmullie/php-api:

```
composer require matthiasmullie/php-api
```


### [html/index.php](https://github.com/matthiasmullie/php-api-example/blob/master/html/index.php)

This is the access point. This file needs to be placed in your document root.
It'll interpret the server request and pass it on to the correct controller,
based on the routing file (in this case **config/routes.yml**),

You can use any web server, as long as all requests end up at index.php.
I included **html/.htaccess** which will rewrite all requests for Apache.


### [config/routes.yml](https://github.com/matthiasmullie/php-api-example/blob/master/config/routes.yml)

This file countains the mapping of server requests to controllers.
Pretty straightforward!

This example has 2 routes:

* GET & POST requests to `/` will be executed by `ExampleController`
* GET requests to `/hello/*` will also be executed by `ExampleController`

In case you prefer JSON over YAML, there's also support for other file types!


### [src/ExampleController.php](https://github.com/matthiasmullie/php-api-example/blob/master/src/ExampleController.php)

And this is an example of where a request could end up!
This is a pointless example that doesn't do anything interesting, but this would
be the place where you put all your application logic. This is where a
particular request is being dealt with.

Just make sure this method returns an array (the result will be `json_encode`d)
or a `ResponseInterface` object. Or throw any `League\Route\Http\Exception`
(e.g. `BadRequestException`) to generate an error response.


## Tests


And it's easy to test your APIs! It comes with 2 helper TestCases:


### MatthiasMullie\Api\TestHelpers\HttpTestCase


This one makes it easy to test your entire thing, including bootstrapping.
It'll let you do requests to `server` (provided by docker-compose), which should
serve your **index.php**.

A docker-compose.yml/Dockerfile is included to quickly get this up and running
(see further down.)

This will let you perform end-to-end tests, where you just feed input into your
API (a specific request) and test the response from the API.

This will perform a real, actual server request. It'll go through your
controllers, bootstrapping, everything. And come back with the result.


### MatthiasMullie\Api\TestHelpers\RequestHandlerTestCase

This will let you perform the exact same types of end-to-end tests that
HttpTestCase does, without performing the actual server request.

It'll pretend to feed a server request into the router. While this bypasses
those 4 little lines of bootstrapping, it does make tests a lot faster, since no
real request has to go out!

Use whichever you prefer!


## Docker & Travis CI

In order to quickly get your API running on your local machine (or anything
supporting Docker images), just build the docker-compose suite by issuing this
makefile command:

```
make test
```

With the included **.travis.yml** config, you should have those tests on
Travis CI in no time!
