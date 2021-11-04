# JSON

[back](../README.md)

JSON (JavaScript Object Notation) is a lightweight data-interchange format. It is easy for humans to read and write. It is easy for machines to parse and generate. It is a text format that is completely language independent but uses conventions that are familiar to to programmers of the C-family of languages (C, C++, C#, Java, JavaScript, Perl, Python, etc). These properties make JSON an ideal data-interchange language.

JSON is built on two structures:

- A collection of name/value pairs. In various languages, this is realized as an _object_, record, struct, dictionary, hash table, keyed list, or associative array. Example: `{}` empty object.
- An ordered list of values. In most languages, this is realized as an array, vector, list, or sequence. Example: `[]` empty array.

In JSON, they take on these forms:

- An _object_ is an unordered set of name/value pairs. An object begins with **{** _left brace_ and ends with **}** _right brace_. Each name is followed by **:** _colon_ and the name/value pairs are separated by **,** _comma_.

```json
  {}

  {
    "property": "value"
  }

  {
    "property1": "value1",
    "property2": "value2",
    "property3": "value3"
  }
```

- An _array_ is an ordered collection of values. An array begins with **[** _left bracket_ and ends with **]** _right bracket_. Values are separated by **,** _comma_.

```json
  []

  [
    "value1",
    "value2",
    "value3"
  ]
```

- A _value_ can be a _string_ in double quotes, or a _number_, or `true` or `false` or `null`, or an _object_ or an _array_. These structures can be nested.

```json
  {
    "property1": "value1",
    "property2": 123,
    "property3": true,
    "property4": false,
    "property5": null,
    "property6": {},
    "property7": []
  }

  [
    "value1",
    123,
    true,
    false,
    null,
    {},
    []
  ]
```

- A _string_ is a sequence of zero or more Unicode characters, wrapped in double quotes, using backslash escapes. A character is represented as a single character string. A string is very much like a C or Java string.

```json
[
  "Any codepoint except \" or \\ or control characters",
  "\" quotation mark",
  "\\ reverse solidus",
  "/ solidus",
  "\b backspace",
  "\f formfeed",
  "\n linefeed",
  "\r carriage return",
  "\t horizontal tab",
  "\u1234 4 hex digits"
]
```

- A number is very much like a C or Java number, except that the octal and hexadecimal formats are not used.

```json
[1, 12, -5, -56, 1.12, 1.12e12, 1.12e-10, 1.12e30, 1.12e-30]
```

- Whitespace can be inserted between any pair of tokens. Excepting a few encoding details, that completely describes the language.

[back](../README.md)
