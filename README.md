# Regular Expression Tutorial

## What are they good for?

Most things regular searching is good for, plus more!

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
