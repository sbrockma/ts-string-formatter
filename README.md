# TypeScript/JavaScript String Formatter

A TypeScript/JavaScript library for string formatting, inspired by
[C++20 std::format](https://en.cppreference.com/w/cpp/utility/format/spec) and
[Python format](https://docs.python.org/3/library/string.html#formatspec).

## Disclaimer!

This is a hobby project—I'm building it for fun and to learn, but I'm putting
real effort into making it solid! That said, there might still be some bugs or
unexpected behavior. Please keep that in mind when using it!

## Note!

Legacy JavaScript has only a single *number* type, not separate *int* and *float*.

    // By default format number as float.
    format("{}", 5);   // "5.0"

    // To format number as integer, use type "d".
    format("{:d}", 5); // "5"

    // Now you can also use int() and float() wrappers. See more below.
    format("{}", int(5));   // "5"
    format("{}", float(5)); // "5.0"

## Install

    npm i @sbrockma/std-format

## Compatibility

This library is written in TypeScript and includes type declarations.
It is bundled with Webpack into ESM, CJS, and UMD formats.

* Only ES5-compatible JavaScript functions are used.
* No polyfills are included.
* CJS and UMD bundles are transpiled with Babel for ES5/IE11 compatibility.
* ESM bundle targets modern environments (ES6+).

While designed for compatibility in mind, the library has not been explicitly
tested against specific Node.js or browser versions.

## Usage

### ESM
    // Import default export
    import StrFmt from "@sbrockma/std-format";

    StrFmt.format("...");

    // Or import named exports
    import { format, int, float, setLocale, FormatError } from "@sbrockma/std-format";

    format("...");

### CJS
    const StrFmt = require("@sbrockma/std-format");
    
    StrFmt.format("...");

### UMD (browser)
This version is bundled with dependencies so it can be used standalone in browser.

Now available via the unpkg CDN. Use with version @1, @2 or exact version (e.g. @1.7.4 for last 1.x).

    <script src="https://unpkg.com/@sbrockma/std-format@2"></script>
    
    <script>
        var format = StdFormat.format;
        format("...");
    </script>

## Declarations

### Function format(str, ...args)

This is the main formatting function.

    format("{} {}!", "Hello", "World");

### Functions int() and float()

int() and float() are wrapper functions that can be used to force *number* to int or float.

    format("{}", int(5));   // "5"
    format("{}", float(5)); // "5.0"

Note: Formatting rules are strict.

    format("{:.2e}", int(5)); // Throws, cannot format int as float.
    format("{:d}", float(5)); // Throws, cannot format float as int.

float() simply wraps a *number*, while int() wraps a JSBI.BigInt, enabling support for large integers.

    format("{:d}", int("111111111111111111111111111111"));

You can also pass BigInt to format(), it will be safely wrapped to int().

    format("{:d}", BigInt("111111111111111111111111111111"));


### Function setLocale(locale)

Default locale is automatically detected.
Locale affects decimal and grouping separators when using the "n" or "L" specifiers.

    setLocale("en-UK");
    setLocale(); // Reset

### Class FormatError

    try {
        format("{:s}", 42);
    } 
    catch(e) {
        if(e instanceof FormatError) {
            console.error(e);
        }
    }

## String Formatting

Replacement field is enclosed in braces '{}' and consists of parts separated by ':'.

    {field_num:arr_1:arr_2:arr_N:elem}

- First part (field_num) is field number.
- Last part (elem) is element presentation.
- Parts between (arr_1...arr_N) are array presentations.
- Any part can be empty string.

Format specification for element:

    [[fill]<^>=][+- ][z][#][0][width][,_][.precision][L][scdnbBoxXeEfF%gGaA]

Format specification for array (and set, map, object):

    [[fill]<^>][width][dbnms]

## Examples

    // Using auto field numbering
    format("{}{}", "A", "B"); // "AB"
    
    // Using manual field numbering
    format("{1}{0}", "A", "B"); // "BA"

    // Using named fields
    format("{name} {age:d}", { name: "Tim", age: 95 }); // "Tim 95"

    // Fill, align and width
    format("{:0<8d}", 777);  // "77700000"
    format("{:0^8d}", 777);  // "00777000"
    format("{:0>8d}", -777); // "0000-777"
    format("{:0=8d}", -777); // "-0000777"

    // Precision
    format("{:.2f}", 1); // "1.00"

    // String width
    format("{:10.4s}", "Alligator"); // "Alli      "

    // With nested arguments
    format("{:{}.{}s}", "Alligator", 10, 4); // "Alli      "

    // Array
    format("{:d}", [1, 2, 3]); // "[1, 2, 3]"

    // Set
    format("{:d}", new Set([1, 2, 3, 2])); // "[1, 2, 3]"

    // Map
    format("{:m:}", new Map([["x", 1], ["y", -1]])); // "[x: 1.0, y: -1.0]"

    // Object
    format("{{{:n:}}}", { x: 1, y: -1}); // "{x: 1.0, y: -1.0}"

    // Floating point types
    format("{0:.3e} {0:.3f} {0:.3%} {0:.3g} {0:.3a}", Math.PI); // "3.142e+00 3.142 314.159% 3.14 1.922p+1"

    // Integer types
    format("{0:#b} {0:#o} {0:#d} {0:#x} {0:c}", 65); // "0b1000001 0o101 65 0x41 A"

## Found a bug or want to request a feature?

Please [open an issue](https://github.com/sbrockma/ts-string-formatter/issues) with a simple
input/output example — it really helps make the project better!

## v2.0.0 Notice

Removed deprecated:
- `stdFormat()`, `stdSpecificationHint()`, `stdLocaleHint()`, `StdFormatError`

Use instead:
- `format()`, `setLocale()`, `FormatError`

Use version `1.7.4` if you need legacy support.

## License

This project is licensed under the [zlib License](./LICENSE).

It also bundles the [JSBI](https://github.com/GoogleChromeLabs/jsbi) library,
which is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).

Please see the [LICENSE](./LICENSE) file for full details.
