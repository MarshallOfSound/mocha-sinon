mocha-sinon
====================

[![Build Status](https://secure.travis-ci.org/elliotf/mocha-sinon.png)](http://travis-ci.org/elliotf/mocha-sinon)

Integration between mocha and sinon, allowing for automatic cleanup of spies

## Installation

via npm:

```
$ npm install --save-dev mocha-sinon
```

## Usage

### Server-side

Require it somewhere in your spec helper file (easier, better)

```javascript
require('mocha-sinon');
```

or, on your command line calling mocha:

```javascript
$ mocha -R mocha-sinon test.js
```

### Browser-side

Source the mocha-sinon file after you have sourced mocha and sinon, and it will "do the right thing"

Unfortunately, I'm not familiar with [requirejs](http://requirejs.org/), so mocha-sinon does not yet support requirejs.  Pull requests are welcome, though.

## Example

```javascript
var child_process = require('child_process')
  , events        = require('events')
  , chai          = require('chai')
  , expect        = chai.expect
;

chai.use(require('sinon-chai'));
require('mocha-sinon');

describe('a shell command', function(){
  beforeEach(function(){
    var fakeChild = this.fakeChild = {
      stdout: new events.EventEmitter()
    };

    this.sinon.stub(child_process, 'spawn', function(){
      return fakeChild;
    });
  });

  it('gets called', function(done){
    someFunction(function(err){
      expect(child_process.spawn).to.have.been.calledWith('/usr/bin/env', ['rm', '-rf', '/']);
    });
  });
});
```

## If you don't like this module

This module is stupidly simple.  You can do it yourself with:

```javascript
var sinon = require('sinon');

beforeEach(function() {
  this.sinon = sinon.sandbox.create();
});

afterEach(function(){
  this.sinon.restore();
});
```
