[![Maintainability](https://api.codeclimate.com/v1/badges/ece4873a022c341da7de/maintainability)](https://codeclimate.com/github/devrelcollective/xela/maintainability) [![BCH compliance](https://bettercodehub.com/edge/badge/devrelcollective/xela?branch=master)](https://bettercodehub.com/) [![Go Report Card](https://goreportcard.com/badge/github.com/devrelcollective/xela)](https://goreportcard.com/report/github.com/devrelcollective/xela) [![GoDoc](https://godoc.org/github.com/devrelcollective/xela?status.svg)](http://godoc.org/github.com/devrelcollective/xela) [![ZenHub](https://raw.githubusercontent.com/ZenHubIO/support/master/zenhub-badge.png)](https://app.zenhub.com/workspace/o/devrelcollective/xela/) [![MIT License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

# xela

A webapp for tracking sponsorship, speaking, and cfps for events.

Built with :heart: by [@mattstratton](https://github.com/mattstratton) in Go.


![xela](https://raw.githubusercontent.com/devrelcollective/xela/master/assets/images/xela-logo.png)


This project adheres to the Contributor Covenant [code of conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. We appreciate your contribution. Please refer to the [contributing guidelines](CONTRIBUTING.md) for details on how to help.

[Powered by Buffalo](http://gobuffalo.io)

## Setup

Right now, this is pretty sparse. A few things:

### Local Dev

Make sure you have [Buffalo](http://gobuffalo.io) installed.

To install the plugins we need, inside the project directory, run

`go get -u -v github.com/gobuffalo/buffalo-plugins`

and then

`buffalo plugins install`

### Running the app locally

Inside the project directory, run `buffalo dev`. This will start the application on http://127.0.0.1:3000

### Local database

You'll need access to a Postgres database set up in the same definition as listed in `database.yml`. I recommend just using Docker. Run this command to get the database going (note; you'll lose all the data when Docker goes down).

```
docker run --rm -it --publish 0.0.0.0:5432:5432 --name pg -e POSTGRES_PASSWORD=postgres postgres:alpine\n\n
```
#### Local database creation and migration

Run the following commands:

`buffalo db create`

`buffalo-pop pop migrate -e development`


### Environment variables

Create a file called `.env` at the root of this project (don't worry, it won't get into git). It should look like this:

```
GOOGLE_KEY=xxxxxx-xxxxxx.apps.googleusercontent.com
GOOGLE_SECRET=xxxx
AUTHORIZED_LOGIN_DOMAINS=pagerduty.com,gmail.com
S3_REGION=us-east-1
S3_BUCKET=xela-dev
AWS_ACCESS_KEY_ID=xxxxxx
AWS_SECRET_ACCESS_KEY=xxxxx
```
(Note - `AUTHORIZED_LOGIN_DOMAINS` is a comma separated list of domains that can log in. If you have only one domain, just enter it without the comma)

Replace the values as appropriate. If you don't know the Google settings, check with @mattstratton (if you're a PagerDuty employee; if you're not, you're on your own right now!)

#### Google Auth

Follow the instructions [here](https://github.com/devrelcollective/xela/blob/master/xela-google-auth.pdf).

### Heroku setup

The following environment variables must be set in Heroku; should look something like this:

```
=== pure-taiga-48603 Config Vars
AUTHORIZED_LOGIN_DOMAINS: pagerduty.com,gmail.com
DATABASE_URL:            postgres://xxxxxx:xxxxxx@ec2-54-83-49-109.compute-1.amazonaws.com:5432/xxxxx
GOOGLE_KEY:              xxxxxxxx-xxxxxxxxx.apps.googleusercontent.com
GOOGLE_SECRET:           xxxxxxxxxxx
GO_ENV:                  production
HOST:                    https://xxxx.herokuapp.com
SESSION_SECRET:          xxxxxxxxxxxxxxxxxx
S3_REGION:               us-east-1
S3_BUCKET:               xela-prod
AWS_ACCESS_KEY_ID:       xxxxxx
AWS_SECRET_ACCESS_KEY:   xxxxxxx
```

Deploying to Heroku via Docker uses these commands (TODO)

#### Initial Setup

(you'll need the [Heroku plugin](https://github.com/gobuffalo/buffalo-heroku) for Buffalo)
```
buffalo heroku new
heroku container:push web
heroku container:release web
heroku config:set HOST=xxxx.herokuapp.com
heroku config:set AUTHORIZED_LOGIN_DOMAINS=pagerduty.com
heroku config:set GOOGLE_KEY=xxx-xxx.apps.googleusercontent.com
heroku config:set GOOGLE_SECRET=xxx
heroku config:set S3_REGION=xxx
heroku config:set S3_BUCKET=xxx
heroku config:set AWS_ACCESS_KEY_ID=xxx
heroku config:set AWS_SECRET_ACCESS_KEY=xxx
heroku run /bin/app migrate
heroku open
```

#### Deployments

After initial setup, deployments can be run via:

```
heroku container:push web
heroku container:release web
heroku run /bin/app migrate
```

Or you can run the `heroku-deploy.sh` shell script.

## Idiosyncracies

This uses a commercial version of [Font Awesome](https://fontawesome.com/). It will work on localhost; it will NOT work on other domains. TODO is to add an environment variable for your font-awesome hash.

There is also a dependency on AWS S3 for uploading images. You'll need to set that up on your own bucket and make sure all the values are in the appropriate environment variables as listed above. If you don't set this up, you just won't be able to add logos to events or photos to Dutonians.

*This is heavily PagerDuty branded right now; a TODO is to make it more white-labeled.*


## Authors

- **Matt Stratton** - *Initial work* - [mattstratton](https://github.com/mattstratton)

## License

xela - A webapp for tracking sponsorship, speaking, and cfps for events

|                      |                                          |
|:---------------------|:-----------------------------------------|
| **Author:**          | Matt Stratton (<matt.stratton@gmail.com>)
| **Copyright:**       | Copyright 2018, Matt Stratton
| **License:**         | The MIT License

```markdown
The MIT License (MIT)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

```