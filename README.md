# `node:mime`

This repository implements the 
[JS MIME API](https://bmeck.github.io/node-proposal-mime-api/)for use bundled
within Node.js core.

## Installation

This is meant to be build into Node.js core due to various utilities it uses.
It is not meant to be installed standalone.

## Testing

Testing uses `--expose-internals` to get a hold of the utilities needed. Ignore
those warnings but other warnings are valid.

```console
$ npm t
```
