# API Scapegoat

[![GitHub Release][releases-shield]][releases]
![Project Stage][project-stage-shield]
[![License][license-shield]](LICENSE.md)

![Supports armhf Architecture][armhf-shield]
![Supports armv7 Architecture][armv7-shield]
![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]
![Supports i386 Architecture][i386-shield]

![Project Maintenance][maintenance-shield]
[![GitHub Activity][commits-shield]][commits]

A simple to use API gateway with failover capabilities.

## About

A simple API gateway with failover capabilities for Home Assistant REST
sensors and the likes.

![banner][banner]

## Run

To run this application simply execute the following commands:

### via npm

```shell
git clone https://github.com/shawly/api-scapegoat
cd api-scapegoat
npm install
npm start
```

### via docker


```shell
docker run -p 3000:3000 -v ./config:/config shawly/api-scapegoat
```

**Note**: _Standalone docker image will follow soon._

## Configuration

API Scapegoat can be configured either via .json or .yaml files. Even though
API Scapegoat uses [http-proxy-middleware][http-proxy-middleware], not every
functionality is implemented or will be implemented. Meaning every feature
which is not documented here, is not supported.

Example configurations are located in the `config` folder, put your own
configurations there as well and remove the examples.

### Example

```yml
---
route: /api
target: 'http://192.168.100.233:8080'
pathRewrite:
  ^/api/hello: /other/api/hello
  ^/api/todos: /todos
changeOrigin: 'false'
router:
  'myclient': 'https://jsonplaceholder.typicode.com'
  '192.168.100.200:8080': 'http://google.de'
failover:
  response:
    status_code: 202
    headers:
      content-type: application/json
      server: API Scapegoat
    body: '{ "status": "api offline" }'

```

### Option: `route` (required)

This option defines the path under which your API target will be served.
Wildcards are allowed, but not necessary if you want to expose the whole
API of your target. To expose your whole API use `/api` without any wildcards.

### Option: `target` (required)

This option defines the target which will receive the requests, this can be
anything from `http://192.168.100.233:8080` for a local host, to any remote
host like `https://google.com` or websockets `ws://echo.websocket.org`.

### Option: `failover` (required)

This option contains settings for the case where your target is unavailable.

#### Option: `failover.response` (required)

The definition of your fallback response which is returned when your target API
is unavailable.

##### Option: `failover.response.status_code` (optional)

The HTTP status code which is returned in your response. If not set, it will
fall back to `200` (OK).

##### Option: `failover.response.headers` (required)

The HTTP response headers of your response. You should at minimum define the
`content-type` of your response, everything else is optional. `content-length`
will always be set according to the length of the response body.

##### Option: `failover.response.body` (required)

The body of your response, this can be anything you want as long as you define
it as string. From an empty JSON object `{}` to a full HTML body, everything
goes.

### Option: `changeOrigin` (optional)

Rewrites the origin of the 'host' header to the target URL.
Can be `'true'` or `'false'`.

### Option: `pathRewrite` (optional)

Here you can define multiple multiple paths that should be rewritten, this is
useful for when you defined a `route` which is different from the base path of
your target.

#### Example

```yml
route: /mytvapi
pathRewrite:
  ^/mytvapi: /api
```

Which will now forward requests from `/mytvapi/*` to `/api/*`.

### Option: `router` (optional)

Here you can define different targets depending on the `Host` header of the
client which triggered the request.

#### Example

```yml
router:
  'myclient': 'https://jsonplaceholder.typicode.com'
  '192.168.100.200:8080': 'http://google.de'
```

If your client, lets say your REST sensor, provides the `Host` header with the
value `myclient`, all requests to your `route` will go to
`https://jsonplaceholder.typicode.com` instead. This might be useful for cases
where you want multiple targets for the same API.

## Changelog & Releases

This repository keeps a change log using [GitHub's releases][releases]
functionality. The format of the log is based on
[Keep a Changelog][keepchangelog].

Releases are based on [Semantic Versioning][semver], and use the format
of ``MAJOR.MINOR.PATCH``. In a nutshell, the version will be incremented
based on the following:

- ``MAJOR``: Incompatible or major changes.
- ``MINOR``: Backwards-compatible new features and enhancements.
- ``PATCH``: Backwards-compatible bugfixes and package updates.

## Support

Got questions?

You can [open an issue here][issue] on GitHub.

## Authors & contributors

The original setup of this repository is by [shawly][shawly].

For a full list of all authors and contributors,
check [the contributor's page][contributors].

## License

MIT License

Copyright (c) 2020 shawly

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[armv7-shield]: https://img.shields.io/badge/armv7-yes-green.svg
[banner]: https://raw.githubusercontent.com/shawly/api-scapegoat/master/logo.png
[commits-shield]: https://img.shields.io/github/commit-activity/y/shawly/api-scapegoat
[commits]: https://github.com/shawly/api-scapegoat/commits/master
[contributors]: https://github.com/shawly/api-scapegoat/graphs/contributors
[dockerhub]: https://hub.docker.com/u/shawly/api-scapegoat
[docs]: https://github.com/shawly/api-scapegoat
[gitlabci-shield]: https://gitlab.com/shawly/api-scapegoat/badges/master/pipeline.svg
[gitlabci]: https://gitlab.com/shawly/api-scapegoat/pipelines
[hass]: https://www.home-assistant.io/
[home-assistant]: https://home-assistant.io
[http-proxy-middleware]: https://github.com/chimurai/http-proxy-middleware
[api-scapegoat]: https://github.com/shawly/api-scapegoat
[i386-shield]: https://img.shields.io/badge/i386-yes-green.svg
[issue]: https://github.com/shawly/api-scapegoat/issues
[keepchangelog]: http://keepachangelog.com/en/1.0.0/
[license-shield]: https://img.shields.io/github/license/shawly/api-scapegoat.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2020.svg
[project-stage-shield]: https://img.shields.io/badge/project%20stage-working-green.svg
[releases-shield]: https://img.shields.io/github/release/shawly/api-scapegoat.svg
[releases]: https://github.com/shawly/api-scapegoat/releases
[repository]: https://github.com/shawly/api-scapegoat
[semver]: http://semver.org/spec/v2.0.0.htm
[shawly]: https://github.com/shawly