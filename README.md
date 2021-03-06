[![Build Status][travis-image]][travis-url]
[![Docker Image Build Status][circle-image]][circle-url]
[![Code Coverage][coverage-image]][coverage-url]
[![Code Climate][climate-image]][climate-url]
[![Dependency Status][gemnasium-image]][gemnasium-url]
[![Current Version][npm-image]][npm-url]

[![Docker Stars][docker-stars-image]][docker-url]
[![Docker Pulls][docker-pulls-image]][docker-url]

[![Stories in Ready][waffle-ready-image]][waffle-url]
[![Stories In Progress][waffle-progress-image]][waffle-url]

[![JS Standard Style][js-standard-image]][js-standard-url]

# f1foo

An integration between Wufoo's WebHooks and Fellowship One's REST API.

It allows Wufoo to be used for event registration (and other) forms, while being
able to track registrations within Fellowship One.

## Installation

This application is best used on a PaaS cloud provider like Heroku, Cloud Foundry,
IBM BlueMix, and others. It's also available as a Docker image. It's (mostly)
pre-configured for Heroku and IBM BlueMix, and it can of course also be used on
a self-hosted environment.

Start by writing your [configuration](#Configuration) into a JSON file and
saving it where you're going to install `f1foo`.

Then, follow the instructions relevant to your environment...

### Docker

See [the docker website](https://docker.com) to get started with Docker.

```
$ docker run \
  --detach \
  --publish 3000:3000 \
  --env F1_CONFIG='{ "apiURL": "...", ... }' \
  hairyhenderson/f1foo
```

### Heroku

Assuming you already have a [Heroku account](https://signup.heroku.com/dc) and
the [Heroku Toolbelt](https://toolbelt.heroku.com/) installed, all you should
need to do is:

```
$ npm install f1foo
$ cd node_modules/f1foo
$ git init
$ heroku apps:create APP_NAME
$ heroku git:remote -a APP_NAME
$ heroku config:set F1_CONFIG="`cat ../../f1config.json`"
$ git add .
$ git commit -am "Initial commit"
$ git push heroku master
```

At this point, the URL to your new app will be printed out - something like
`https://APP_NAME.herokuapp.com`

### Self-hosted

```
$ npm install f1foo
$ cd node_modules/f1foo
$ npm start
```

The app is now running at http://localhost:3000/.

## Configuration

All configuration is done through environment variables.

### `F1_CONFIG`

_Required._

An environment variable `F1_CONFIG` must be set with your Fellowship One
credentials. It's a JSON string like this:

```json
{
  "apiURL": "https://churchcode.staging.fellowshiponeapi.com/v1",
  "username": "Me",
  "password": "reallysecurepassword",
  "oauth_credentials": {
    "consumer_key": "111",
    "consumer_secret": "12345678-9abc-defe-dcba-987654321012"
  }
}
```

___Note:___ For testing you probably should use the staging API URL, but make sure
to use your production API URL for production.

### `WUFOO_HANDSHAKE_KEY`

_Optional._

When `WUFOO_HANDSHAKE_KEY` is set, f1foo will reject incoming posts that don't
have a matching handshake key.

### `DEBUG`

_Optional._

Controls the [debug](https://npmjs.org/package/debug) logs to aid with troubleshooting.
Try `DEBUG=f1foo` to see f1foo's debug logs, or `DEBUG=*` to see all debug logs.

### `NEW_STATUS`

_Optional._

Defaults to `New from Website`.

When creating a Person record as a result of a form submission, the Person must be
assigned a [Status](http://developer.fellowshipone.com/docs/v1/People/Statuses.help).
This variable is used to look up the Status by `name`. To create new People with
different Statuses, set this variable, e.g. `NEW_STATUS='New from Wufoo'`.

## Usage

Once f1foo is [configured](#Configuration) and [running](#Installation), add
WebHook notifications to your forms.
See [these instructions](http://help.wufoo.com/articles/en_US/SurveyMonkeyArticleType/Webhooks).
The path on the URL should be `/hooks`, so if the app is running on Heroku,
the URL should be something like `https://f1foo.herokuapp.com/hooks`, and the
handshakeKey should be set (or not) to your `WUFOO_HANDSHAKE_KEY` value.

_Important:_ Wufoo must send the form metadata along with each POST. In the Wufoo
UI, this is
- [x] Include Field and Form Structures with Entry Data

## Contributions

Pull Requests are more than welcome!

## Tests

To run the unit tests:

```
$ npm install --dev
$ make test
```

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2014-2015 Dave Henderson

[travis-image]: https://img.shields.io/travis/hairyhenderson/f1foo.svg?style=flat
[travis-url]: https://travis-ci.org/hairyhenderson/f1foo

[circle-image]: https://img.shields.io/circleci/project/hairyhenderson/f1foo.svg?label=docker+build
[circle-url]: https://circleci.com/gh/hairyhenderson/f1foo

[coverage-image]: https://img.shields.io/codeclimate/coverage/github/hairyhenderson/f1foo.svg?style=flat
[coverage-url]: https://codeclimate.com/github/hairyhenderson/f1foo

[climate-image]: https://img.shields.io/codeclimate/github/hairyhenderson/f1foo.svg?style=flat
[climate-url]: https://codeclimate.com/github/hairyhenderson/f1foo

[gemnasium-image]: https://img.shields.io/gemnasium/hairyhenderson/f1foo.svg?style=flat
[gemnasium-url]: https://gemnasium.com/hairyhenderson/f1foo

[npm-image]: https://img.shields.io/npm/v/f1foo.svg?style=flat
[npm-url]: https://npmjs.org/package/f1foo

[docker-stars-image]: https://img.shields.io/docker/stars/hairyhenderson/f1foo.svg
[docker-pulls-image]: https://img.shields.io/docker/pulls/hairyhenderson/f1foo.svg
[docker-url]: https://hub.docker.com/r/hairyhenderson/f1foo

[waffle-ready-image]: https://badge.waffle.io/hairyhenderson/f1foo.svg?label=ready&title=Ready
[waffle-progress-image]: https://badge.waffle.io/hairyhenderson/f1foo.svg?label=in+progress&title=In+Progress
[waffle-url]: https://waffle.io/hairyhenderson/f1foo

[js-standard-image]: https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat
[js-standard-url]: http://standardjs.com/

