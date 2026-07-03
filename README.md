![Seneca](http://senecajs.org/files/assets/seneca-logo.png)
> A [Seneca.js](http://senecajs.org) plugin

# seneca

[![npm version](https://img.shields.io/npm/v/seneca.svg)](https://npmjs.com/package/seneca)
[![build](https://github.com/senecajs/seneca/actions/workflows/build.yml/badge.svg)](https://github.com/senecajs/seneca/actions/workflows/build.yml)
[![Coverage Status](https://coveralls.io/repos/senecajs/seneca/badge.svg?branch=master&service=github)](https://coveralls.io/github/senecajs/seneca?branch=master)
[![Known Vulnerabilities](https://snyk.io/test/github/senecajs/seneca/badge.svg)](https://snyk.io/test/github/senecajs/seneca)
[![DeepScan grade](https://deepscan.io/api/teams/5016/projects/6816/branches/59148/badge/grade.svg)](https://deepscan.io/dashboard#view=project&tid=5016&pid=6816&bid=59148)
[![Maintainability](https://api.codeclimate.com/v1/badges/3a95be9ab6432c620bea/maintainability)](https://codeclimate.com/github/senecajs/seneca/maintainability)

| ![Voxgig](https://www.voxgig.com/res/img/vgt01r.png) | This open source module is sponsored and supported by [Voxgig](https://www.voxgig.com). |
|---|---|

## Install

```sh
npm install seneca
```

## Quick Example

```js
'use strict'

var Seneca = require('seneca')

// Functionality in seneca is composed into simple
// plugins that can be loaded into seneca instances.

function rejector () {
  this.add('cmd:run', (msg, done) => {
    return done(null, {tag: 'rejector'})
  })
}

function approver () {
  this.add('cmd:run', (msg, done) => {
    return done(null, {tag: 'approver'})
  })
}

function local () {
  this.add('cmd:run', function (msg, done) {
    this.prior(msg, (err, reply) => {
      return done(null, {tag: reply ? reply.tag : 'local'})
    })
  })
}

// Services can listen for messages using a variety of
// transports. In process and http are included by default.

Seneca()
  .use(approver)
  .listen({type: 'http', port: '8260', pin: 'cmd:*'})

Seneca()
  .use(rejector)
  .listen(8270)

// Load order is important, messages can be routed
// to other services or handled locally. Pins are
// basically filters over messages

function handler (err, reply) {
  console.log(err, reply)
}

Seneca()
  .use(local)
  .act('cmd:run', handler)

Seneca()
  .client({port: 8270, pin: 'cmd:run'})
  .client({port: 8260, pin: 'cmd:run'})
  .use(local)
  .act('cmd:run', handler)

Seneca()
  .client({port: 8260, pin: 'cmd:run'})
  .client({port: 8270, pin: 'cmd:run'})
  .use(local)
  .act('cmd:run', handler)

// Output
// null { tag: 'local' }
// null { tag: 'approver' }
// null { tag: 'rejector' }
```

## More Examples

See [test/](test/) for usage examples.

## Motivation

Seneca is a microservices toolkit for Node.js. It helps you write clean, organized code that you can scale and deploy at any time.

## Support

If you're using this module and need help, you can:

- Post a [github issue](https://github.com/senecajs/seneca/issues)
- Tweet to [@senecajs](http://twitter.com/senecajs)
- Ask on the [Gitter](https://gitter.im/senecajs/seneca)

## API

See [senecajs.org](http://senecajs.org) for full API documentation.

## Contributing

The [Senecajs org](https://github.com/senecajs/) encourages open participation. If you feel you can help in any way, be it with documentation, examples, extra testing, or new features please get in touch.

The [Senecajs org](https://github.com/senecajs/) encourages open participation. If you feel you can help in any way, be it with documentation, examples, extra testing, or new features please get in touch.

### Running tests

```sh
npm run test
```

## Background

Seneca is sponsored and supported by [Voxgig](https://www.voxgig.com).

[abbreviated form](https://github.com/rjrodger/jsonic) of JSON. In fact, you
[Programmer Anarchy](http://vimeo.com/43690647).
[Coveralls]: https://coveralls.io/github/senecajs/seneca?branch=master
[DeepScan]: https://deepscan.io/dashboard#view=project&tid=5016&pid=6816&bid=59148
[CodeClimate]: https://codeclimate.com/github/senecajs/seneca/maintainability
[Npm]: https://www.npmjs.com/package/seneca
