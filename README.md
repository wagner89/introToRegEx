# Introduction to Regular Expressions (RegEx)

```
Some people, when confronted with a problem, think 
“I know, I’ll use regular expressions.” Now they have two problems.
``` 
_source - Stack Exchange ([right here](https://softwareengineering.stackexchange.com/questions/223634/what-is-meant-by-now-you-have-two-problems))_

__Note: all examples of syntax are JavaScript based - check your language of choice for appropriate syntax__

## What is a Regular Expression

**Short answer:** a RegEx is a string, which defines a search pattern. We can then search for the described pattern in other strings, bodies of text, what have you.

**Long story:** RegExes are interpreted as being written in a language of their own, which allows for quite complex patterns to be defined. They are usually a combination of a pattern string and a set of flags, which define how the pattern should be interpreted. The patterns can range from simple wildcards to find strings of a given format within a text, or validate a format, to complex expression containing extractable groups, ranges, etc.

## What is a RegEx good for

A wide variety of things, here's a list of most common applications:
 - validate string formats: email addresses, phone numbers, passwords, credic card numbers, what have you
 - finding and replacing values in text to clean, reformat, or change content
 - extracting parts from a text: value from an HTML/XML/JSON file, extensions/names of files, paragraphs
 - parsing text files into values for import into a database, code input, etc

## Fundamentals

A RegEx is actually a pattern and some flags (optional)

In JS this looks like:

`regexp = new RegExp("pattern", "flags");` - init using constructor

`regexp = /pattern/; // no flags` 

`regexp = /pattern/gm; // with flags g,m` - init using `/` (like `'` and `"` for strings)

MDN regarding when to use constructor vs. literal:

```
Regular expression literals provide compilation of the regular expression when the script is loaded. When the regular expression will remain constant, use this for better performance.

Using the constructor function provides runtime compilation of the regular expression. Use the constructor function when you know the regular expression pattern will be changing, or you don't know the pattern and are getting it from another source, such as user input.
```

RegEx operations in JS:

`exec`	- search for a match in a string - returns array or `null`

`test`	- tests for a match in a string, true if the string matches, false otherwise

`match`	- searches for a match in a string - returns array or `null`

`matchAll`	- returns an iterator containing all of the matches, including capturing groups

`search`	- tests for a match in a string, returning the index of the match, or -1 if the search fails

`replace`	- search for a match in a string, and replaces the matched substring with a replacement substring

`split` -	break a string into an array of substrings

Single line vs. Multi-line (/m)

### Syntax

First let's check out the possible flags:

`g`	- Global search - all matches

`i`	- Case-insensitive search

`m`	- Multi-line search (changes `^` and `$` from start/end of string, to start/end of line)

`s`	- Allows . to match newline characters

`u`	- "unicode"; treat a pattern as a sequence of unicode code points

`y`	- Perform a "sticky" search that matches starting at the current position in the target string.


#### Characters

They can be literal, meta, shorthand, non-printable

- **literal characters** are like `f`, `Q`, `9`, `#` - they represent themselves
- **metacharacters** have special meanings, and are (in most flavors) the following:

`\`, `^`, `$`, `.`, `|`, `?`, `*`, `+`, `(`, `)`, `[`, `{`

`\` - escape character - treat following metacharacter as literal (`.` is `\.`, and `\` is `\\`)

`^` - start of a line

`$` - end of a line

`.` - any character except new line

`|` - alternation - basically a boolean OR

`?` - optional operator (quantifier matching 0 or 1)

`*` - quantifier matching 0 or more times

`+` - quantifier matching 1 or more times

`()` - character group - all characters from the set must match, in order

`[]` - character set - one character from the set must match, no ordering

`{n}` - the previous character or character group `n times`,

`{n,m}` - the previous character or character group `at least n, at most m times`

https://imgs.xkcd.com/comics/backslashes_2x.png

- **shorthand character classes**

`\d`, `\w`, `\s` - digit, 'word character' (`a-Z0-9_`), space

`\D`, `\W`, `\S` - NOT digit, word character, space

`\b`, `\B` - anchors for word boundaries

- **non printable characters** (the most common ones)

`\n` - LF (line feed) character

`\r` - CR (carrige return) character

`\t` - tab

`\R` - any line break - LF, CR, LF and CR, vertical tab, unicode newline

`\0` - null character

`\xFF` - hexadecimal, where F is [0-9A-F]


## Groups

`(` `)` - can be used to create (capturing) groups

`\1`, `\2` ... - can be used to reference groups

`(ha)\1` - this matches `haha` from `hahaha` 
`([a-z])_\1` - this matches `a_a`, but not `b_b`

A reference to a capturing group doens't repeat the rule, but the exact match it captured.

To get access to it's contents in JS, we can use

String.match(RegExp)
RegExp.exec(String)

Both return arrays: \[ entireMatch | groups\]

Example:
`/([M|m]rs*) (\w+) (\w+)/` applied to `mr. James Hetfield` returns ["mr. James Hetfield", "mr", "James", "hetfield"]
`'Mr James Hetfield'.match(/[M|m]rs* (\w+) \w+/)[1]` will result in "James", extracting the only capturing group 
`(?: )` - this is a non-capturing group, backslash reference numbers are skipped

A group matched multiple times keeps the last match in the result.

Example:
`'acbd'.match(/(\w)+/)` will return ["abcd", "d"]

Named capturing groups produce an object along with the array, like so:
`/[M|m]rs* (<firstname>\w+) \w+/` applied to `mr. James Hetfield` returns { 0: "mr. James Hetfield", 1: "James", groups: { firstname: "James" }, ... }

Lookaround (lookahead and lookbehind)

Also non-capturing groups, backslash reference numbers are skipped for all lookarounds.

Lookahead

`(?= )` - positive lookahead (something followed by something else)
The group itself is not included in the match, it's there to define what should follow the previous rule

Example: `^Mircea(?= [P|c])`

`(?! )` - negative lookahead (something not followed by something else)
The group itself is not included in the match, it's there to define what should not follow the previous rule

Example: `^Mircea(?! [P|c])`

```
Mircea Pîrv
Mircea Lazar
Mircea cel Bătrân
```

Lookbehind

`(?<= )` - positive lookbehind (something preceded by something else)
The group itself is not included in the match, it's there to define what should follow the previous rule

Example: `(?<=(\w+ ){2})Terrier`

`(?<! )` - negative lookbehind (something not preceded by something else)
The group itself is not included in the match, it's there to define what should not follow the previous rule

Example: `(?<!(\w+ ){2})Terrier`

```
Jack Russel Terrier
Bitbull Terrier
Scottish Terrier
```

Lookaround expressions can contain any regex without additional lookarounds. Capturing groups work, and can be referenced outside as well.

## JavaScript specifics


## When NOT to use a RegEx

### When there is a more readable solution
  **Example: check if a string is lowercase**

   `var isLowercase = myString === myString.toLowerCase();`


   instead of


   `var isLowercase = /^[a-z]*$/.test(myString);`
     
### When the regular expression is costly

 Parsing, tokenization, for example - always use specialized tools for concrete taasks when they exist, regex should practically be trivial.
   
## Good to know things

**Always be careful when using nested quantifiers or alternations***

They most likely can result in catastrophic backtracking (unless everything is mutually exclusive)

Example:
`(x+x+)+y`

`xxxxxxxxxxy`
`xxxxxxxxxx`
`xxxxxxxxxxxxxxxxx`

```If you try this regex on a 10x string in RegexBuddy's debugger, it'll take 2558 steps to figure out the final y is missing. For an 11x string, it needs 5118 steps. For 12, it takes 10238 steps. Clearly we have an exponential complexity of O(2^n) here. At 21x the debugger bows out at 2.8 million steps, diagnosing a bad case of catastrophic backtracking.``` [quoted from here](https://www.regular-expressions.info)

**RegEx is greedy by default**

Quantifiers in a RegEx will always try to match as much as possible, unless followed by `?`, which in this case mark the group lazy - i.e. try to match as little as possible.

Example:
`<span>.*<\/span>` vs `<span>.*?<\/span>`

`<span>This is the first span. We want only this.</span><span>Not this!</span>`

**Free spacing RegEx and comments**

`/x` in some engines - doesn't work in ES6

## Resources

#### Online RegEx testers:

[RegEx 101](https://regex101.com/)

[RegExer](https://regexr.com/)

[RegEx Testing](https://regexr.com/)


#### Cheat sheets

[RegexEgg](https://www.rexegg.com/regex-quickstart.html) - long, distinguishes between environments

[Cheatography](https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/) - concise and nicely layed out

[Printable PDF of the above](http://www.cbs.dtu.dk/courses/27610/regular-expressions-cheat-sheet-v2.pdf)

[KeyCDN](https://www.keycdn.com/support/regex-cheatsheet)

[Debuggex](https://www.debuggex.com/cheatsheet/regex/python) - FIlterable

#### Tutorials, Tips and Tricks

[GitHub RegEx intro](https://github.com/ziishaned/learn-regex)

[Google Analytics](https://analytics.googleblog.com/2009/04/regular-expression-tips-and-tricks.html)

#### JavaScript specific pages

[Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

[W3Schools](https://www.w3schools.com/jsref/jsref_obj_regexp.asp)

[Nice summary / tutorial](https://flaviocopes.com/javascript-regular-expressions/)

#### Fun articles

[Coding Horror](https://blog.codinghorror.com/regex-use-vs-regex-abuse/)

[relevant xkcd](https://xkcd.com/1313/)

[awesome loggly article](https://www.loggly.com/blog/regexes-the-bad-better-best/)
