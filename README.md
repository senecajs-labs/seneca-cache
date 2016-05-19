![Seneca](http://senecajs.org/files/assets/seneca-logo.png)

> A [Seneca.js][] in-memory caching plugin.

# seneca-cache
[![npm version][npm-badge]][npm-url]
[![Build Status][travis-badge]][travis-url]
[![Dependency Status][david-badge]][david-url]
[![Coveralls][BadgeCoveralls]][Coveralls]
[![Gitter][gitter-badge]][gitter-url]

## Description

This module is a plugin for the [Seneca framework](http://senecajs.org). It provides a set of common caching actions (`get`, `set` etc.), backed by [lru-cache](https://github.com/isaacs/node-lru-cache).
It also exposes some lru-cache specific actions (`peek`, `has`, `keys`, `values`, `reset`).

By moving cache operations into Seneca, you can change your cache implementation or business rules at a later point.
For example, you might decide to send certain kinds of keys to a different cache mechanism, such as redis.

### Seneca compatibility
Supports Seneca versions **1.x** and **2.x**

## Install

```sh
npm install seneca
npm install seneca-cache
```

### Quick example

This code snippet sets a value and then retrieves it.

```js
var seneca = require('seneca')();
seneca.use('cache');

seneca.ready(function(err) {
  seneca.act({role: 'cache', cmd: 'set', key: 'k1', val: 'v1'}, function(err) {
    seneca.act({role: 'cache', cmd: 'get', key: 'k1'}, function(err, out) {
      console.log('value = ' + out)
    });
  });
});
```

The full action argument pattern can be a bit tedious, so use a Seneca _pin_ to make things more convenient:

```js
var cache = seneca.pin({role:'cache', cmd:'*'});

cache.set({key: 'k1', val: 'v1'}, function(err) {
  cache.get({key:'k1'}, function(err, out) {
    console.log('value = ' + out);
  });
});
```

## Common Cache API

Seneca has a common caching API with the following actions:

   * `role:cache, cmd:set` store a value - _key_ and _val_ arguments required
   * `role:cache, cmd:get` retreive a value - _key_ argument is required
   * `role:cache, cmd:add` store a value, only if the key does not exist - _key_ and _val_ arguments required
   * `role:cache, cmd:delete` delete a value - _key_ argument is required, no error if key does not exist
   * `role:cache, cmd:incr` increment a value - _key_ and _val_ (integer) arguments required
   * `role:cache, cmd:decr` decrement a value - _key_ and _val_ (integer) arguments required

All caching plugins, including this one, implement this action API.

### Options

You can use any of the options from the [lru-cache](https://github.com/isaacs/node-lru-cache) module directly as options to this plugin:

```js
seneca.use('cached', {
  lrucache: {
    max: 1000,
    maxAge: 1000 * 60 * 60,
    length: function(n) {return n.length}
  }
});
```

## Contributing

The [Senecajs org][] encourages open participation. If you feel you
can help in any way, be it with documentation, examples, extra
testing, or new features please get in touch.

## Test

```bash
mocha test/cache.test.js
```

## License

Copyright (c) 2014-2016, Seamus D'Arcy and other contributors.
Licensed under [MIT][].

[MIT]: ./LICENSE
[Seneca.js]: https://www.npmjs.com/package/seneca
[travis-badge]: https://travis-ci.org/senecajs/seneca-cache.svg
[travis-url]: https://travis-ci.org/senecajs/seneca-cache
[npm-badge]: https://img.shields.io/npm/v/seneca-cache.svg
[npm-url]: https://npmjs.com/package/seneca-cache
[david-badge]: https://david-dm.org/senecajs/seneca-cache.svg
[david-url]: https://david-dm.org/senecajs/seneca-cache
[gitter-badge]: https://badges.gitter.im/Join%20Chat.svg
[gitter-url]: https://gitter.im/rjrodger/seneca
[Senecajs org]: https://github.com/senecajs/
[Coveralls]: https://coveralls.io/github/senecajs/seneca-cache?branch=master
[BadgeCoveralls]: https://coveralls.io/repos/github/senecajs/seneca-cache/badge.svg?branch=master
