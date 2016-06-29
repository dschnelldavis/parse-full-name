# parse-full-name

[![Build](https://travis-ci.org/dschnelldavis/parse-full-name.svg?branch=master)](https://travis-ci.org/dschnelldavis/parse-full-name)
[![Dependencies](https://david-dm.org/dschnelldavis/parse-full-name.svg)](https://david-dm.org/dschnelldavis/parse-full-name)
[![Vulnerabilities](https://snyk.io/test/npm/parse-full-name/badge.svg?style=flat-square)](https://snyk.io/test/npm/parse-full-name)

## Description

parseFullName() is designed to parse large batches of full names in multiple
inconsistent formats, as from a database, and continue processing without error,
even if given some unparsable garbage entries.

parseFullName():

1. accepts a string containing a person's full name, in any format,
2. analyzes and attempts to detect the format of that name,
3. (if possible) parses the name into its component parts, and
4. (by default) returns an object containing all individual parts of the name:
    - title (string): title(s) (e.g. "Ms." or "Dr.")
    - first (string): first name or initial
    - middle (string): middle name(s) or initial(s)
    - last (string): last name or initial
    - nick (string): nickname(s)
    - suffix (string): suffix(es) (e.g. "Jr.", "II", or "Esq.")
    - error (array of strings): any parsing error messages

Optionally, parseFullName() can also:

* return only the specified part of a name as a string (or errors as an array)
* always fix or ignore the letter case of the returned parts (the default is
    to fix the case only when the original input is all upper or all lowercase)
* stop on errors (the default is to return warning messages in the output,
    but never throw a JavaScript error, no matter how mangled the input)
* detect more variations of name prefixes, suffixes, and titles (the default
    detects 29 prefixes, 19 suffixes, 16 titles, and 8 conjunctions, but it
    can be set to detect 97 prefixes, 23 suffixes, and 204 titles instead)

If this is not what you're looking for, is overkill for your application, or
is in the wrong language, check the "Credits" section at the end of this file
for links to several other excellent parsers which may suit your needs better.

## Use

### Basic Use

```javascript
var parseFullName = require('parse-full-name').parseFullName;

name = parseFullName('Mr. David Davis');

assert.equal(name.title, 'Mr.');
assert.equal(name.first, 'David');
assert.equal(name.last, 'Davis');
```

### Options

parseFullName(nameToParse, partToReturn, fixCase, stopOnError, useLongLists)

nameToParse (string, required): the name to be parsed

partToReturn (string, optional): the name of a single part to return

  - 'all' (default) = return an object containing all name parts
  - 'title' = return only the title(s) as a string (or an empty string)
  - 'first' = return only the first name as a string (or an empty string)
  - 'middle' = return only the middle name(s) as a string (or an empty string)
  - 'last' = return only the last name as a string (or an empty string)
  - 'nick' = return only the nickname(s) as a string (or an empty string)
  - 'suffix' = return only the suffix(es) as a string (or an empty string)
  - 'error' = return only the array of parsing error messages (or an empty array)

fixCase (integer, optional): fix case of output name

  - -1 (default) = fix case only if input name is all upper or lowercase
  - 0 or false = never fix the case (retain and output same case as input name)
  - 1 or true = always fix case of output, even if input is mixed case

stopOnError (integer, optional): makes parsing errors throw JavaScript errors

  - 0 or false (default) = return warnings about parsing errors, but continue
  - 1 or true = if a parsing error is found, throw a JavaScript error

useLongLists (integer, optional): use long prefix, suffix, and title lists

  - 0 or false (default) = use default lists (29 prefixes, 19 suffixes, 16 titles)
  - 1 or true = use experimental long lists (97 prefixes, 23 suffixes, 204 titles)
  Note: The alternate long lists are experimental and have not been tested.
  Be especially careful using the long prefix list, which may incorrectly
  detect "Ben" as a prefix, which is common in middle-eastern names,
  rather than as a first name, which is common in English names

### Advanced Use

```javascript
var parseFullName = require('parse-full-name').parseFullName;

name = parseFullName('DE LORENZO Y GUTIEREZ, Mr. JÜAN MARTINEZ (MARTIN) Jr.','all',1,0,0);

assert.equal(name.title, 'Mr.');
assert.equal(name.first, 'Jüan');
assert.equal(name.middle, 'Martinez');
assert.equal(name.last, 'de Lorenzo y Gutierez');
assert.equal(name.nick, 'Martin');
assert.equal(name.suffix, 'Jr.');
assert.equal(name.error, []);
```

## Reporting Bugs

If you find a name this function does not parse correctly, or any other bug,
please report it here: https://github.com/dschnelldavis/parse-full-name/issues

## Credits and precursors

Before creating this function I studied many other name-parsing functions.
None quite suited my needs, but many are excellent at what they do, and
this function uses ideas from several of them.

My thanks to all the following developers for sharing their work.

"If I have seen further, it is by standing on the shoulders of giants."
— Isaac Newton

Josh Fraser's PHP-Name-Parser:
https://github.com/joshfraser/PHP-Name-Parser

Josh Fraser's JavaScript-Name-Parser:
https://github.com/joshfraser/JavaScript-Name-Parser

Garve Hays' Java NameParser:
https://github.com/gkhays/NameParser

Jason Priem's PHP HumanNameParser:
https://web.archive.org/web/20150408022642/http://jasonpriem.org/human-name-parse/ and
https://github.com/jasonpriem/HumanNameParser.php

Keith Beckman's PHP nameparse:
http://alphahelical.com/code/misc/nameparse/

Jed Hartman's PHP normalize_name:
http://www.kith.org/journals/jed/2007/02/11/3813.html and
http://www.kith.org/logos/things/code/name-parser-php.html

ashaffer88's JavaScript parse-name:
https://github.com/weo-edu/parse-name and
https://www.npmjs.com/package/parse-name

Derek Gulbranson's Python nameparser:
https://github.com/derek73/python-nameparser/

Discussion about how to change all upper or lowercase names to correct case:
http://stackoverflow.com/questions/11529213/given-upper-case-names-transform-to-proper-case-handling-ohara-mcdonald

Title lists modified from:
http://www.codeproject.com/Questions/262876/Titles-or-Salutation-list

Suffix lists modified from:
http://en.wikipedia.org/wiki/Suffix_(name) and
https://github.com/derek73/python-nameparser/blob/master/nameparser/config/suffixes.py

Prefix lists modified from:
http://en.wikipedia.org/wiki/List_of_family_name_affixes

Conjunction list copied entirely from:
https://github.com/derek73/python-nameparser/blob/master/nameparser/config/conjunctions.py
