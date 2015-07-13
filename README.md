The [UNIX command](http://en.wikipedia.org/wiki/Rm_(Unix)) `rm -rf` for node.  

Install with `npm install rimraf`, or just drop rimraf.js somewhere.

## API

`rimraf(f, callback)`

The callback will be called with an error if there is one.  Certain
errors are handled for you:

* Windows: `EBUSY` and `ENOTEMPTY` - rimraf will back off a maximum of
  `opts.maxBusyTries` times before giving up, adding 100ms of wait
  between each attempt.  The default `maxBusyTries` is 3.
* `ENOENT` - If the file doesn't exist, rimraf will return
  successfully, since your desired outcome is already the case.
* `EMFILE` - Since `readdir` requires opening a file descriptor, it's
  possible to hit `EMFILE` if too many file descriptors are in use.
  In the sync case, there's nothing to be done for this.  But in the
  async case, rimraf will gradually back off with timeouts up to
  `opts.emfileWait` ms, which defaults to 1000.

## rimraf.sync

It can remove stuff synchronously, too.  But that's not so good.  Use
the async API.  It's better.

## CLI

If installed with `npm install rimraf -g` it can be used as a global
command `rimraf <path>` which is useful for cross platform support.

## mkdirp

If you need to create a directory recursively, check out
[mkdirp](https://github.com/substack/node-mkdirp).

## Electron Usage
Electron monkey-patches the `'fs'` module to support `.asar` archives - because of this any `'fs'` api calls to an `.asar` file (or even a file named `*.asar`) fail. In this case here, if you try to run `rimraf` from Electron on a folder containing an `.asar` file inside, the process will fail. In order to support this, we detect for `process.versions.electron` and `require('original-fs')`, an unmodified version of the `'fs'` library.

More info:
- https://github.com/88dots/extract-zip
- https://github.com/atom/electron/issues/2035   **<= possible workaround**
- https://github.com/atom/electron/issues/1658
- https://github.com/atom/electron/issues/782
