# Regular Expression Tutorial

## What are they good for?

Most things standard searching is good for, plus more!

### Finding lines in a file

Here I've used a regular expression to search for words with 2 `e` characters
next to each other.

```bash
grep ee < /usr/share/dict/words
abandonee
abeltree
Aberdeen
ableeze
absentee
```

### Advanced search and replace

Here I'm adding a comment character to the beginning of all my lines.

![searching for: ^ Replacing with //](https://raw.githubusercontent.com/ccouzens/regex_tutorial/master/advanced%20replace%20add%20comments.png)

### Finding strings that conform

Here I've written a naive Javascript function that checks to see if an
affirmative word is in the input text.

```javascript
function affirmative(text) {
  return /yes|yarp|yeah|ok|affirmative/i.test(text)
}
```

### Finding strings that don't conform

Here I've written a Javascript function that checks your telephone number does
not contain letters.

```javascript
function isTelephoneNumber(text) {
  // Telephone numbers cannot contain letters
  return ! /[A-z]/.test(text)
}
```

### Extracting information from a pattern

Here I've written a Javascript function that extracts the date of birth from a
complex string about a person.

```javascript
function dateOfBirth(text) {
  const matches = text.match(/DOB: ([0-9/]+)/)
  return matches[1]
}

const einstein = "Name: Albert Einstein, Nationality: American, Weimar, Swiss, Prussian, DOB: 14/04/1978, Died: 14/03/1955"
dateOfBirth(einstein) // Returns "14/04/1978"
```

As you can see, regular expressions can be used to perform a variety of
different functions, and can be used in a variety of different contexts.

## Limitations

Regular expression implementations vary.
The basic syntax can be expected to be well supported in all instances.
Advanced features may not always be present, or may work slightly differently.

Some problems can't be determined by a regular expression.
For example, we can't write a regular expression to determine if a string has matching open and close brackets.

```
# Can't be determined by a regular expression!
(((()))) # good
(((())) # bad
```

For more information, please study the computer science topic [automata](https://en.wikipedia.org/wiki/Regular_language#Location_in_the_Chomsky_hierarchy).

The tooling you're using may force you to use regular expressions in a particular way.
For example it may want you to provide a regular expression that matches positive input.
But it may be more convenient to write a regular expression that matches negative input.

## Writing/Reading Regular Expressions

Mozilla have a good
[reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#Writing_a_regular_expression_pattern).

### Simple patterns

If you want search for an exact match, simple use what you're searching for as
the regular expression.

Here we're searching for words that contain "cake".

```bash
egrep cake < /usr/share/dict/words
ashcake
battercake
bridecake
cake
cakebox
cakebread
cakehouse
cakemaker
cakemaking
caker
```

However, if your search has any of these characters:
`\^$[](){}*+.?`
then you need to escape them.
These are known as special characters, because they have extra meaning.
We'll cover them later.

### Escaping special characters

If you want to match a special character, you need to prepend a `\` (back
slash).

For example, if you want to search for `(text)` in this document, your regular
expression would be `\(text\)`.

Or if you want to search for file paths that match a Windows `C:\` drive, your
regular expression would be `C:\\`.
The backslash is escaped with a backslash!

### Matching at the start of a line

You can 'anchor' your regular expression to the start of a line by using the
`^` character.

```bash
egrep '^cake' < /usr/share/dict/words
cake
cakebox
cakebread
cakehouse
cakemaker
cakemaking
caker
cakette
cakewalk
cakewalker
```

Here we use it to search for words that start with 'cake'.

### Matching at the end of a line

You can 'anchor' your regular expression to the end of a line by using the
`$` character.

```bash
egrep 'cake$' < /usr/share/dict/words
ashcake
battercake
bridecake
cake
carcake
cheesecake
coffeecake
corncake
creamcake
cupcake
```

Here we use it to search for words that end with 'cake'.

```bash
egrep '^cake$' < /usr/share/dict/words
cake
```

Only 'cake' begins and ends with 'cake'.

### Matching multiple options

You can use allow multiple options by using the `|` or pipe character.

```bash
egrep 'apple|pear' < /usr/share/dict/words
appear
appearance
appearanced
appearer
apple
appleberry
appleblossom
applecart
appledrane
applegrower
```

Here we're looking for lines that contain apple or pear.

We can use brackets (non matching parentheses) if we want to only have some of
the expression multiple choice.

```bash
egrep --color '(?:apple|pear)$' < /usr/share/dict/words
appear
apple
Balmawhapple
boarspear
capple
coappear
compear
crapple
dapple
disappear
```

Here we're looking for lines that contian apple or pear, then immediately end.

We can combine 2 groups of multiple choice at the same time.

```bash
egrep '(?:a|e|i|o|u)(?:a|e|i|o|u)' < /usr/share/dict/words | head
aa
aal
aalii
aam
aardvark
aardwolf
Ababua
abacination
abaction
abaisance
```

Here we're looking for words that contain 2 vowels immediately after each other.

### Repeating ourselves

Regular expressions give us several ways of expressing the previous example
without the repetition.

The following are all equivalent:

```
(?:a|e|i|o|u)(?:a|e|i|o|u)
(?:a|e|i|o|u){2}
(?:a|e|i|o|u){2,2}
```

They all match vowels immediately after each other.

More specifically, `{n}` matches the previous expression precisely n times.
`{n, m}` matches the previous expression between n and m times.

```bash
egrep --color 'd(?:a|e|i|o|u){2,3}s' < /usr/share/dict/words
acanthocladous
acanthopodous
acediast
achlamydeous
acromyodous
adenodiastasis
adenopodous
adradius
aecidiospore
aecidiostage
```

The above searches for lines with a d followed by 2 or 3 vowels followed by an s.

### Repeating ourselves indefinitely

We can use `{n,}` to repeat ourselves n or more times.

```bash
egrep --color '^t(?:a|b|c|d|e|f|g|h|i|j|k|l|m|n|o|p|q|r|s|t|u|v|w|x|y|z){8,}s$' < /usr/share/dict/words
taboparalysis
taboparesis
taccaceous
tachygenesis
tachythanatous
tackleless
tactfulness
tactlessness
taeniosomous
taillessness
```

This searches for all words beginning with t, ending with s with 8 or more
characters in between.
This could also be described at all words beginning with t, ending with s
that are 10 or more characters long;
But it doesn't match the regular expression so well.

The shortcut for `{1,}` is `+`.
The shortcut for `{0,}` is `*`.
