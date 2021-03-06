[![Build Status](https://travis-ci.org/OADA/oada-lookup-js.svg)](https://travis-ci.org/OADA/oada-lookup-js)
[![Coverage Status](https://coveralls.io/repos/OADA/oada-lookup-js/badge.png?branch=master)](https://coveralls.io/r/OADA/oada-lookup-js?branch=master)
[![Dependency Status](https://david-dm.org/oada/oada-lookup-js.svg)](https://david-dm.org/oada/oada-lookup-js)
[![License](http://img.shields.io/:license-Apache%202.0-green.svg)](http://www.apache.org/licenses/LICENSE-2.0.html)

oada-lookup-js
==============
JavaScript utility library to lookup OADA documents such as [Well-Known (RFC
5785)][well-known] resource, e.g., oada-configuration, [openid-configuration][],
etc, and public OADA client registrations.

Getting Started
---------------

### Installation ###
The library can be installed with `npm` using
```sh
$ npm install oada-lookup
```

### Running the tests, coverage, and style checks ###
The libraries test can be ran with:
```sh
$ npm test
```

The coverage report is generated by:
```sh
$ npm run cover
```

Gulp runs `jshint` (lint) and `jscs` (style) with:
```sh
$ gulp lint
$ gulp style
```

### wellKnown(hostname, suffix, options, cb) ###
Fetch a [Well-Known (RFC 5785)][well-known] Resource. The hostname will
automatically be parsed from any URL.

#### Parameters ####
`hostname` {String} Hostname (or URL) hosting the Well-Known resource being
requested. Sub-domains and ports are be persevered; Protocol, path, query
parameters, and hash are dropped. It is assumed that the Well-Known resource is
hosted with TLS (https) *Pull Request appreciated*

[`suffix`][] {String} Well-Known resource suffix being requested.

`options` {Object} containing at least the following properties:

* `timeout` {Number} *Default: 1000* Timeout before HTTP request fails in ms.

`cb` {Function} Result callback. It takes the form `function(err, resource) {}`.

#### Usage Example ####
```javascript
var lookup = require('oada-lookup');

var options = {
  timeout: 500
};

lookup.wellKnown('provider.oada-dev.com', 'oada-configuration', options,
  function(err, resource) {
    console.log(err);
    console.log(resource);
  });
```

### clientRegistration(clientId, options, cb) ###
Fetch a client registration from an OADA client id.

#### Parameters ####
`clientId` {String} The OADA client id to lookup the client registration for. It
takes a form similar to email: `id@domain`.

`options` {Object} containing at least the following properties:

* `timeout` {Number} *Default: 1000* Timeout before HTTP request fails in ms.

`cb` {Function} Result callback. It takes the form `function(err, registration){}`.

#### Usage Example ####
```javascript
var lookup = require('oada-lookup');

var options = {
  timeout: 500
};

lookup.clientRegistration('xJx82s@provider.oada-dev.com', options,
  function(err, registration) {
    console.log(err);
    console.log(registration);
  });
```

### jwks(uri, options, cb) ###
Fetch a [Json Web Key Set (JWKS)][json-web-key-set] from an URI.

#### Parameters ####
`uri` {String} The URI containing the desired JWKS document. For example, the
value of the OpenID Connect openid-configuration `jwks_uri` property.

`options` {Object} containing at least the following properties:

* `timeout` {Number} *Default: 1000* Timeout before HTTP request fails in ms.

`cb` {Function} Result callback. It takes the form `function(err, jwks){}`.

#### Usage Example ####
```javascript
var lookup = require('oada-lookup');

var options = {
  timeout: 500
};

lookup.jwks('provider.oada-dev.com/oidc/jwks', options,
  function(err, jwks) {
    console.log(err);
    console.log(jwks);
  });
```

References
----------

1. [Defining Well-Known Uniform Resource Identifiers (URIs)][well-known]
2. [OpenID Discovery](http://openid.net/specs/openid-connect-discovery-1_0.html)

[well-known]: http://tools.ietf.org/html/rfc5785
[openid-configuration]: http://openid.net/specs/openid-connect-discovery-1_0.html#ProviderMetadata
[`suffix`]: http://tools.ietf.org/html/rfc5785#section-5.1.1 "RFC5785 Section 5.1.1"
[json-web-key-set]: https://tools.ietf.org/html/draft-ietf-jose-json-web-key-33#page-10
