<!--

@license Apache-2.0

Copyright (c) 2024 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# Rayleigh Random Numbers

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Fill a strided array with pseudorandom numbers drawn from a [Rayleigh][@stdlib/random/base/rayleigh] distribution.

<section class="installation">

## Installation

```bash
npm install @stdlib/random-strided-rayleigh
```

Alternatively,

-   To load the package in a website via a `script` tag without installation and bundlers, use the [ES Module][es-module] available on the [`esm`][esm-url] branch (see [README][esm-readme]).
-   If you are using Deno, visit the [`deno`][deno-url] branch (see [README][deno-readme] for usage intructions).
-   For use in Observable, or in browser/node environments, use the [Universal Module Definition (UMD)][umd] build available on the [`umd`][umd-url] branch (see [README][umd-readme]).

The [branches.md][branches-url] file summarizes the available branches and displays a diagram illustrating their relationships.

To view installation and usage instructions specific to each branch build, be sure to explicitly navigate to the respective README files on each branch, as linked to above.

</section>

<section class="usage">

## Usage

```javascript
var rayleigh = require( '@stdlib/random-strided-rayleigh' );
```

#### rayleigh( N, sigma, ss, out, so )

Fills a strided array with pseudorandom numbers drawn from a [Rayleigh][@stdlib/random/base/rayleigh] distribution.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

// Create an array:
var out = new Float64Array( 10 );

// Fill the array with pseudorandom numbers:
rayleigh( out.length, [ 2.0 ], 0, out, 1 );
```

The function has the following parameters:

-   **N**: number of indexed elements.
-   **sigma**: rate parameter.
-   **ss**: index increment for `sigma`.
-   **out**: output array.
-   **so**: index increment for `out`.

The `N` and stride parameters determine which strided array elements are accessed at runtime. For example, to access every other value in `out`,

```javascript
var out = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

rayleigh( 3, [ 2.0 ], 0, out, 2 );
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

<!-- eslint-disable stdlib/capitalized-comments -->

```javascript
var Float64Array = require( '@stdlib/array-float64' );

// Initial array:
var sigma0 = new Float64Array( [ 2.0, 2.0, 2.0, 2.0, 2.0, 2.0 ] );

// Create offset view:
var sigma1 = new Float64Array( sigma0.buffer, sigma0.BYTES_PER_ELEMENT*3 ); // start at 4th element

// Create an output array:
var out = new Float64Array( 3 );

// Fill the output array:
rayleigh( out.length, sigma1, -1, out, 1 );
```

#### rayleigh.ndarray( N, sigma, ss, os, out, so, oo )

Fills a strided array with pseudorandom numbers drawn from a [Rayleigh][@stdlib/random/base/rayleigh] distribution using alternative indexing semantics.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

// Create an array:
var out = new Float64Array( 10 );

// Fill the array with pseudorandom numbers:
rayleigh.ndarray( out.length, [ 2.0 ], 0, 0, out, 1, 0 );
```

The function has the following additional parameters:

-   **os**: starting index for `sigma`.
-   **oo**: starting index for `out`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying `buffer`, the offset parameters support indexing semantics based on starting indices. For example, to access every other value in `out` starting from the second value,

```javascript
var out = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

rayleigh.ndarray( 3, [ 2.0 ], 0, 0, out, 2, 1 );
```

#### rayleigh.factory( \[options] )

Returns a function for filling strided arrays with pseudorandom numbers drawn from a [Rayleigh][@stdlib/random/base/rayleigh] distribution.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var random = rayleigh.factory();
// returns <Function>

// Create an array:
var out = new Float64Array( 10 );

// Fill the array with pseudorandom numbers:
random( out.length, [ 2.0 ], 0, out, 1 );
```

The function accepts the following `options`:

-   **prng**: pseudorandom number generator for generating uniformly distributed pseudorandom numbers on the interval `[0,1)`. If provided, the function **ignores** both the `state` and `seed` options. In order to seed the underlying pseudorandom number generator, one must seed the provided `prng` (assuming the provided `prng` is seedable).
-   **seed**: pseudorandom number generator seed.
-   **state**: a [`Uint32Array`][@stdlib/array/uint32] containing pseudorandom number generator state. If provided, the function ignores the `seed` option.
-   **copy**: `boolean` indicating whether to copy a provided pseudorandom number generator state. Setting this option to `false` allows sharing state between two or more pseudorandom number generators. Setting this option to `true` ensures that an underlying generator has exclusive control over its internal state. Default: `true`.

To use a custom PRNG as the underlying source of uniformly distributed pseudorandom numbers, set the `prng` option.

```javascript
var Float64Array = require( '@stdlib/array-float64' );
var minstd = require( '@stdlib/random-base-minstd' );

var opts = {
    'prng': minstd.normalized
};
var random = rayleigh.factory( opts );

var out = new Float64Array( 10 );
random( out.length, [ 2.0 ], 0, out, 1 );
```

To seed the underlying pseudorandom number generator, set the `seed` option.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var opts = {
    'seed': 12345
};
var random = rayleigh.factory( opts );

var out = new Float64Array( 10 );
random( out.length, [ 2.0 ], 0, out, 1 );
```

* * *

#### random.PRNG

The underlying pseudorandom number generator.

```javascript
var prng = rayleigh.PRNG;
// returns <Function>
```

#### rayleigh.seed

The value used to seed the underlying pseudorandom number generator.

```javascript
var seed = rayleigh.seed;
// returns <Uint32Array>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = rayleigh.factory({
    'prng': minstd
});
// returns <Function>

var seed = random.seed;
// returns null
```

#### rayleigh.seedLength

Length of underlying pseudorandom number generator seed.

```javascript
var len = rayleigh.seedLength;
// returns <number>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = rayleigh.factory({
    'prng': minstd
});
// returns <Function>

var len = random.seedLength;
// returns null
```

#### rayleigh.state

Writable property for getting and setting the underlying pseudorandom number generator state.

```javascript
var state = rayleigh.state;
// returns <Uint32Array>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = rayleigh.factory({
    'prng': minstd
});
// returns <Function>

var state = random.state;
// returns null
```

#### rayleigh.stateLength

Length of underlying pseudorandom number generator state.

```javascript
var len = rayleigh.stateLength;
// returns <number>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = rayleigh.factory({
    'prng': minstd
});
// returns <Function>

var len = random.stateLength;
// returns null
```

#### rayleigh.byteLength

Size (in bytes) of underlying pseudorandom number generator state.

```javascript
var sz = rayleigh.byteLength;
// returns <number>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = rayleigh.factory({
    'prng': minstd
});
// returns <Function>

var sz = random.byteLength;
// returns null
```

</section>

<!-- /.usage -->

<section class="notes">

* * *

## Notes

-   If `N <= 0`, both `rayleigh` and `rayleigh.ndarray` leave the output array unchanged.
-   Both `rayleigh` and `rayleigh.ndarray` support array-like objects having getter and setter accessors for array element access.

</section>

<!-- /.notes -->

<section class="examples">

* * *

## Examples

<!-- eslint no-undef: "error" -->

```javascript
var zeros = require( '@stdlib/array-zeros' );
var zeroTo = require( '@stdlib/array-zero-to' );
var logEach = require( '@stdlib/console-log-each' );
var rayleigh = require( '@stdlib/random-strided-rayleigh' );

// Specify a PRNG seed:
var opts = {
    'seed': 1234
};

// Create a seeded PRNG:
var rand1 = rayleigh.factory( opts );

// Create an array:
var x1 = zeros( 10, 'float64' );

// Fill the array with pseudorandom numbers:
rand1( x1.length, [ 2.0 ], 0, x1, 1 );

// Create another function for filling strided arrays:
var rand2 = rayleigh.factory( opts );
// returns <Function>

// Create a second array:
var x2 = zeros( 10, 'generic' );

// Fill the array with the same pseudorandom numbers:
rand2( x2.length, [ 2.0 ], 0, x2, 1 );

// Create a list of indices:
var idx = zeroTo( x1.length, 'generic' );

// Print the array contents:
logEach( 'x1[%d] = %.2f; x2[%d] = %.2f', idx, x1, idx, x2 );
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

* * *

## See Also

-   <span class="package-name">[`@stdlib/random-base/rayleigh`][@stdlib/random/base/rayleigh]</span><span class="delimiter">: </span><span class="description">Rayleigh distributed pseudorandom numbers.</span>
-   <span class="package-name">[`@stdlib/random-array/rayleigh`][@stdlib/random/array/rayleigh]</span><span class="delimiter">: </span><span class="description">create an array containing pseudorandom numbers drawn from a Rayleigh distribution.</span>

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2026. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/random-strided-rayleigh.svg
[npm-url]: https://npmjs.org/package/@stdlib/random-strided-rayleigh

[test-image]: https://github.com/stdlib-js/random-strided-rayleigh/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/random-strided-rayleigh/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/random-strided-rayleigh/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/random-strided-rayleigh?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/random-strided-rayleigh.svg
[dependencies-url]: https://david-dm.org/stdlib-js/random-strided-rayleigh/main

-->

[chat-image]: https://img.shields.io/badge/zulip-join_chat-brightgreen.svg
[chat-url]: https://stdlib.zulipchat.com

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/random-strided-rayleigh/tree/deno
[deno-readme]: https://github.com/stdlib-js/random-strided-rayleigh/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/random-strided-rayleigh/tree/umd
[umd-readme]: https://github.com/stdlib-js/random-strided-rayleigh/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/random-strided-rayleigh/tree/esm
[esm-readme]: https://github.com/stdlib-js/random-strided-rayleigh/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/random-strided-rayleigh/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/random-strided-rayleigh/main/LICENSE

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@stdlib/random/base/rayleigh]: https://github.com/stdlib-js/random-base-rayleigh

[@stdlib/array/uint32]: https://github.com/stdlib-js/array-uint32

<!-- <related-links> -->

[@stdlib/random/array/rayleigh]: https://github.com/stdlib-js/random-array-rayleigh

<!-- </related-links> -->

</section>

<!-- /.links -->
