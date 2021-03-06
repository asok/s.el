# s.el [![Build Status](https://secure.travis-ci.org/magnars/s.el.png)](http://travis-ci.org/magnars/s.el)

The long lost Emacs string manipulation library.

## Installation

It's available on [marmalade](http://marmalade-repo.org/) and [Melpa](http://melpa.milkbox.net/):

    M-x package-install s

Or you can just dump `s.el` in your load path somewhere.

## Functions


### Shorten string

* [s-truncate](#s-truncate-len-s) `(len s)`
* [s-left](#s-left-len-s) `(len s)`
* [s-right](#s-right-len-s) `(len s)`
* [s-chop-suffix](#s-chop-suffix-suffix-s) `(suffix s)`
* [s-chop-suffixes](#s-chop-suffixes-suffixes-s) `(suffixes s)`

### Tweak whitespace

* [s-chomp](#s-chomp-s) `(s)`
* [s-trim](#s-trim-s) `(s)`
* [s-trim-left](#s-trim-left-s) `(s)`
* [s-trim-right](#s-trim-right-s) `(s)`
* [s-collapse-whitespace](#s-collapse-whitespace-s) `(s)`
* [s-word-wrap](#s-word-wrap-len-s) `(len s)`

### To and from lists

* [s-lines](#s-lines-s) `(s)`
* [s-match](#s-match-regexp-s) `(regexp s)`
* [s-join](#s-join-separator-strings) `(separator strings)`
* [s-concat](#s-concat-rest-strings) `(&rest strings)`

### Predicates

* [s-equals?](#s-equals-s1-s2) `(s1 s2)`
* [s-matches?](#s-matches-regexp-s) `(regexp s)`
* [s-blank?](#s-blank-s) `(s)`
* [s-ends-with?](#s-ends-with-suffix-s-optional-ignore-case) `(suffix s &optional ignore-case)`
* [s-starts-with?](#s-starts-with-prefix-s-optional-ignore-case) `(prefix s &optional ignore-case)`
* [s-contains?](#s-contains-needle-s-optional-ignore-case) `(needle s &optional ignore-case)`
* [s-lowercase?](#s-lowercase-s) `(s)`
* [s-uppercase?](#s-uppercase-s) `(s)`
* [s-mixedcase?](#s-mixedcase-s) `(s)`

### The misc bucket

* [s-repeat](#s-repeat-num-s) `(num s)`
* [s-replace](#s-replace-old-new-s) `(old new s)`
* [s-downcase](#s-downcase-s) `(s)`
* [s-upcase](#s-upcase-s) `(s)`
* [s-capitalize](#s-capitalize-s) `(s)`
* [s-index-of](#s-index-of-needle-s-optional-ignore-case) `(needle s &optional ignore-case)`

### Pertaining to words

* [s-split-words](#s-split-words-s) `(s)`
* [s-lower-camel-case](#s-lower-camel-case-s) `(s)`
* [s-upper-camel-case](#s-upper-camel-case-s) `(s)`
* [s-snake-case](#s-snake-case-s) `(s)`
* [s-dashed-words](#s-dashed-words-s) `(s)`
* [s-capitalized-words](#s-capitalized-words-s) `(s)`

## Documentation and examples


### s-truncate `(len s)`

If `s` is longer than `len`, cut it down and add ... at the end.

```cl
(s-truncate 6 "This is too long") ;; => "Thi..."
(s-truncate 16 "This is also too long") ;; => "This is also ..."
(s-truncate 16 "But this is not!") ;; => "But this is not!"
```

### s-left `(len s)`

Returns up to the `len` first chars of `s`.

```cl
(s-left 3 "lib/file.js") ;; => "lib"
(s-left 3 "li") ;; => "li"
```

### s-right `(len s)`

Returns up to the `len` last chars of `s`.

```cl
(s-right 3 "lib/file.js") ;; => ".js"
(s-right 3 "li") ;; => "li"
```

### s-chop-suffix `(suffix s)`

Remove `suffix` if it is at end of `s`.

```cl
(s-chop-suffix "-test.js" "penguin-test.js") ;; => "penguin"
(s-chop-suffix "\n" "no newlines\n") ;; => "no newlines"
(s-chop-suffix "\n" "some newlines\n\n") ;; => "some newlines\n"
```

### s-chop-suffixes `(suffixes s)`

Remove `suffixes` one by one in order, if they are at the end of `s`.

```cl
(s-chop-suffixes '("_test.js" "-test.js" "Test.js") "penguin-test.js") ;; => "penguin"
(s-chop-suffixes '("\r" "\n") "penguin\r\n") ;; => "penguin\r"
(s-chop-suffixes '("\n" "\r") "penguin\r\n") ;; => "penguin"
```


### s-chomp `(s)`

Remove one trailing `\n`, `\r` or `\r\n` from `s`.

```cl
(s-chomp "no newlines\n") ;; => "no newlines"
(s-chomp "no newlines\r\n") ;; => "no newlines"
(s-chomp "some newlines\n\n") ;; => "some newlines\n"
```

### s-trim `(s)`

Remove whitespace at the beginning and end of `s`.

```cl
(s-trim "trim ") ;; => "trim"
(s-trim " this") ;; => "this"
(s-trim " only  trims beg and end  ") ;; => "only  trims beg and end"
```

### s-trim-left `(s)`

Remove whitespace at the beginning of `s`.

```cl
(s-trim-left "trim ") ;; => "trim "
(s-trim-left " this") ;; => "this"
```

### s-trim-right `(s)`

Remove whitespace at the end of `s`.

```cl
(s-trim-right "trim ") ;; => "trim"
(s-trim-right " this") ;; => " this"
```

### s-collapse-whitespace `(s)`

Convert all adjacent whitespace characters to a single space.

```cl
(s-collapse-whitespace "only   one space   please") ;; => "only one space please"
(s-collapse-whitespace "collapse \n all \t sorts of \r whitespace") ;; => "collapse all sorts of whitespace"
```

### s-word-wrap `(len s)`

If `s` is longer than `len`, wrap the words with newlines.

```cl
(s-word-wrap 10 "This is too long") ;; => "This is\ntoo long"
(s-word-wrap 10 "This is way way too long") ;; => "This is\nway way\ntoo long"
(s-word-wrap 10 "It-wraps-words-but-does-not-break-them") ;; => "It-wraps-words-but-does-not-break-them"
```


### s-lines `(s)`

Splits `s` into a list of strings on newline characters.

```cl
(s-lines "abc\ndef\nghi") ;; => '("abc" "def" "ghi")
```

### s-match `(regexp s)`

When the given expression matches the string, this function returns a list
of the whole matching string and a string for each matched subexpressions.
If it did not match the returned value is an empty list (nil).

```cl
(s-match "^def" "abcdefg") ;; => nil
(s-match "^abc" "abcdefg") ;; => '("abc")
(s-match "^/.*/\\([a-z]+\\)\\.\\([a-z]+\\)" "/some/weird/file.html") ;; => '("/some/weird/file.html" "file" "html")
```

### s-join `(separator strings)`

Join all the strings in `strings` with `separator` in between.

```cl
(s-join "+" '("abc" "def" "ghi")) ;; => "abc+def+ghi"
(s-join "\n" '("abc" "def" "ghi")) ;; => "abc\ndef\nghi"
```

### s-concat `(&rest strings)`

Join all the string arguments into one string.

```cl
(s-concat "abc" "def" "ghi") ;; => "abcdefghi"
```


### s-equals? `(s1 s2)`

Is `s1` equal to `s2`?

This is a simple wrapper around the built-in `string-equal`.

```cl
(s-equals? "abc" "ABC") ;; => nil
(s-equals? "abc" "abc") ;; => t
```

### s-matches? `(regexp s)`

Does `regexp` match `s`?

This is a simple wrapper around the built-in `string-match-p`.

```cl
(s-matches? "^[0-9]+$" "123") ;; => t
(s-matches? "^[0-9]+$" "a123") ;; => nil
```

### s-blank? `(s)`

Is `s` nil or the empty string?

```cl
(s-blank? "") ;; => t
(s-blank? nil) ;; => t
(s-blank? " ") ;; => nil
```

### s-ends-with? `(suffix s &optional ignore-case)`

Does `s` end with `suffix`?

If `ignore-case` is non-nil, the comparison is done without paying
attention to case differences.

Alias: `s-suffix?`

```cl
(s-ends-with? ".md" "readme.md") ;; => t
(s-ends-with? ".MD" "readme.md") ;; => nil
(s-ends-with? ".MD" "readme.md" t) ;; => t
```

### s-starts-with? `(prefix s &optional ignore-case)`

Does `s` start with `prefix`?

If `ignore-case` is non-nil, the comparison is done without paying
attention to case differences.

Alias: `s-prefix?`. This is a simple wrapper around the built-in
`string-prefix-p`.

```cl
(s-starts-with? "lib/" "lib/file.js") ;; => t
(s-starts-with? "LIB/" "lib/file.js") ;; => nil
(s-starts-with? "LIB/" "lib/file.js" t) ;; => t
```

### s-contains? `(needle s &optional ignore-case)`

Does `s` contain `needle`?

If `ignore-case` is non-nil, the comparison is done without paying
attention to case differences.

```cl
(s-contains? "file" "lib/file.js") ;; => t
(s-contains? "nope" "lib/file.js") ;; => nil
(s-contains? "^a" "it's not ^a regexp") ;; => t
```

### s-lowercase? `(s)`

Are all the letters in `s` in lower case?

```cl
(s-lowercase? "file") ;; => t
(s-lowercase? "File") ;; => nil
(s-lowercase? "123?") ;; => t
```

### s-uppercase? `(s)`

Are all the letters in `s` in upper case?

```cl
(s-uppercase? "HULK SMASH") ;; => t
(s-uppercase? "Bruce no smash") ;; => nil
(s-uppercase? "123?") ;; => t
```

### s-mixedcase? `(s)`

Are there both lower case and upper case letters in `s`?

```cl
(s-mixedcase? "HULK SMASH") ;; => nil
(s-mixedcase? "Bruce no smash") ;; => t
(s-mixedcase? "123?") ;; => nil
```


### s-repeat `(num s)`

Make a string of `s` repeated `num` times.

```cl
(s-repeat 10 " ") ;; => "          "
(s-concat (s-repeat 8 "Na") " Batman!") ;; => "NaNaNaNaNaNaNaNa Batman!"
```

### s-replace `(old new s)`

Replaces `old` with `new` in `s`.

```cl
(s-replace "file" "nope" "lib/file.js") ;; => "lib/nope.js"
(s-replace "^a" "\\1" "it's not ^a regexp") ;; => "it's not \\1 regexp"
```

### s-downcase `(s)`

Convert `s` to lower case.

This is a simple wrapper around the built-in `downcase`.

```cl
(s-downcase "ABC") ;; => "abc"
```

### s-upcase `(s)`

Convert `s` to upper case.

This is a simple wrapper around the built-in `upcase`.

```cl
(s-upcase "abc") ;; => "ABC"
```

### s-capitalize `(s)`

Convert each word's first character to upper case and the rest to lower case in `s`.

This is a simple wrapper around the built-in `capitalize`.

```cl
(s-capitalize "abc DEF") ;; => "Abc Def"
```

### s-index-of `(needle s &optional ignore-case)`

Returns first index of `needle` in `s`, or nil.

If `ignore-case` is non-nil, the comparison is done without paying
attention to case differences.

```cl
(s-index-of "abc" "abcdef") ;; => 0
(s-index-of "CDE" "abcdef" t) ;; => 2
(s-index-of "n.t" "not a regexp") ;; => nil
```


### s-split-words `(s)`

Split `s` into list of words.

```cl
(s-split-words "under_score") ;; => '("under" "score")
(s-split-words "some-dashed-words") ;; => '("some" "dashed" "words")
(s-split-words "evenCamelCase") ;; => '("even" "Camel" "Case")
```

### s-lower-camel-case `(s)`

Convert `s` to lowerCamelCase.

```cl
(s-lower-camel-case "some words") ;; => "someWords"
(s-lower-camel-case "dashed-words") ;; => "dashedWords"
(s-lower-camel-case "under_scored_words") ;; => "underScoredWords"
```

### s-upper-camel-case `(s)`

Convert `s` to UpperCamelCase.

```cl
(s-upper-camel-case "some words") ;; => "SomeWords"
(s-upper-camel-case "dashed-words") ;; => "DashedWords"
(s-upper-camel-case "under_scored_words") ;; => "UnderScoredWords"
```

### s-snake-case `(s)`

Convert `s` to snake_case.

```cl
(s-snake-case "some words") ;; => "some_words"
(s-snake-case "dashed-words") ;; => "dashed_words"
(s-snake-case "camelCasedWords") ;; => "camel_cased_words"
```

### s-dashed-words `(s)`

Convert `s` to dashed-words.

```cl
(s-dashed-words "some words") ;; => "some-words"
(s-dashed-words "under_scored_words") ;; => "under-scored-words"
(s-dashed-words "camelCasedWords") ;; => "camel-cased-words"
```

### s-capitalized-words `(s)`

Convert `s` to Capitalized Words.

```cl
(s-capitalized-words "some words") ;; => "Some Words"
(s-capitalized-words "under_scored_words") ;; => "Under Scored Words"
(s-capitalized-words "camelCasedWords") ;; => "Camel Cased Words"
```


## What's with the built-in wrappers?

Imagine looking through the function list and seeing `s-ends-with?`, but
`s-starts-with?` is nowhere to be found. Why? Well, because Emacs already has
`string-prefix-p`. Now you're starting out slightly confused, then have to go
somewhere else to dig for the command you were looking for.

The wrapping functions serve as both documentation for existing functions and
makes for a consistent API.

## Contributors

* [Arthur Andersen](https://github.com/leoc) contributed `s-match`

Thanks!

## Contribute

Yes, please do. There's a suite of tests, so remember to add tests for your
specific feature, or I might break it later.

You'll find the repo at:

    https://github.com/magnars/s.el

**Looking for work?** Here are some features we would like:

 - `(s-center 80 s)` pads s with spaces to center the string.
 - `(s-distance s1 s2)` calculates Levenshtein distance between s1 and s2
 - `(s-shared-start s1 s2)` returns the longest prefix s1 and s2 have in common
 - `(s-shared-end s1 s2)` returns the longest suffix s1 and s2 have in common

## Development

Run the tests with

    ./run-tests.sh

Create the docs with

    ./create-docs.sh

I highly recommend that you install these as a pre-commit hook, so that
the tests are always running and the docs are always in sync:

    cp pre-commit.sh .git/hooks/pre-commit

Oh, and don't edit `README.md` directly, it is auto-generated.
Change `readme-template.md` or `examples-to-docs.el` instead.

## License

Copyright (C) 2012 Magnar Sveen

Authors: Magnar Sveen <magnars@gmail.com>
Keywords: strings

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
