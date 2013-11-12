# gif-encoder [![Build status](https://travis-ci.org/twolfson/gif-encoder.png?branch=master)](https://travis-ci.org/twolfson/gif-encoder)

Streaming [GIF][] encoder

[GIF]: http://en.wikipedia.org/wiki/Graphics_Interchange_Format

This is built as part of the [gifsockets][] project. It is forked from [gif.js][] to allow for a streaming API and performance optimization.

[gifsockets]: https://github.com/twolfson/gifsockets-server

## Getting Started
Install the module with: `npm install gif-encoder`

```js
// Create a 10 x 10 gif
var GifEncoder = require('gif-encoder');
var gif = new GifEncoder(10, 10);

// using an rgba array of pixels [r, g, b, a, ... continues on for every pixel]
// This can be collected from a <canvas> via context.getImageData(0, 0, width, height).data
var pixels = [0, 0, 0, 255/*, ...*/];

// Collect output
var file = require('fs').createWriteStream('img.gif');
gif.pipe(file);

// Write out the image into memory
gif.writeHeader();
gif.addFrame(pixels);
// gif.addFrame(pixels); // Write subsequent rgba arrays for more frames
gif.finish();
```

## Documentation
`gif-encoder` exports a constructor function which extends `readable-stream@1.1.9`. This means you can use any `streams1`/`streams2` functionality. I will re-iterate what this means below.

```js
// streams1
var gif = new GifEncoder(10, 10);
gif.on('data', console.log);
gif.on('end', process.exit);

// streams2
var gif = new GifEncoder(10, 10);
gif.on('readable', function () {
  console.log(gif.read());
});
```

### Constructor

### Events
`data`

`end`

`error`

`readable`

`writeHeader#start/stop`

`frame#start/stop`

`finish#start/stop`

### Methods
setDelay(ms); // Milliseconds to wait between frames

setFrameRate(fps); // Sugar method to set delay based on amount of frames per second

setDispose(disposalCode); // TODO: Research this more

setRepeat(repeat); -1: play once, 0: indefinitely, positive number: n times

setTransparent(color);

setQuality(quality);

1 is best colors, worst performance.

20 is suggested maximum but there is no limit.

10 is the default, provided an even trade-off.

addFrame(imageData);

finish();

#### Low-level
flushData();

writeHeader();

writeImageInfo();

analyzeImage();

outputImage();

// TODO: Make this method.
setImagePixels();

## Donating
Support this project and [others by twolfson][gittip] via [gittip][].

[![Support via Gittip][gittip-badge]][gittip]

[gittip-badge]: https://rawgithub.com/twolfson/gittip-badge/master/dist/gittip.png
[gittip]: https://www.gittip.com/twolfson/

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint via [grunt](https://github.com/gruntjs/grunt) and test via `npm test`.

## UNLICENSE
As of Nov 11 2013, Todd Wolfson has released all code differences since initial fork from [gif.js][] to the public domain.

These differences have been released under the [UNLICENSE][].

[UNLICENSE]: UNLICENSE

At the [gif.js][] time of forking, [gif.js][] was using the [MIT license][].

[gif.js]: https://github.com/jnordberg/gif.js/tree/faee238491302de05a1ed05e4fbe562738a76310

[MIT license]: https://github.com/jnordberg/gif.js/tree/faee238491302de05a1ed05e4fbe562738a76310#license
