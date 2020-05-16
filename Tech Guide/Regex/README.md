# Regex

- [Regex](#regex)
  - [Basic Concepts](#basic-concepts)
  - [Practical Usage in Real World](#practical-usage-in-real-world)

I **LOVE** regexes (REGular EXpressions), and I believe there is more to them than meets the eyes.

## Basic Concepts

- Basic Characters

Meta-Character|Description|Example
-------------|-----------|-------
`.`|Any Character|
`\`|Escape Character|`\.` means only a dot
`\n`|New Line|
`\r`|Return|
`\t`|Tab|
`\0`|Null Character|

- Anchors

Meta-Character|Description|Example
-------------|-----------|-------
`^`|Start of string (multiline)|
`\A`|Start of string (singleline)|
`$`|End of string (multiline)|
`\Z`|End of string (single line)|
`\b`|A word boundary = performs a "whole words only" search|`\babc\b`
`\B`|Non-word boundary = matches only if the pattern is fully surrounded by word characters|`\Babc\B`

- Meta Sequences

Meta-Character|Description|Example
-------------|-----------|-------
`\s`|Any whitespace character|
`\S`|Any non-whitespace character|
`\d`|Any digit|
`\D`|Any non-digit|
`\w`|Any word character = alphanumeric and underscore = `[A-Za-z0-9_]`|
`\W`|Any non-word character|
`\x`|Hex Character|`\x20` means space in hex
`[\b]`|Backspace character|

- Quantifiers

Meta-Character|Description|Example
-------------|-----------|-------
`?`|Zero or one|`a?` means zero or one of a
`*`|Zero or more|`a*` means zero or more of a
`+`|One or more|`a+` means one or more of a
`{n}`|Exactly n times|`a{3}` exactly 3 of a
`{n,}`|More than n times|`a{3,}` 3 or more of a
`{n,m}`|Between n and m|`a{3,5}` between 3 and 5 of a
`*`|Greedy Quantifier|`a.*a` greedy c**an be dangerous a**t times
`*?`|Lazy Quantifier|`a.*?a` greedy c**an be da**ngerous at times

- Group Constructs

Meta-Character|Description|Example
-------------|-----------|-------
`()`|Capture everything enclosed|`(a\|b)`
`(?:)`|using ?: will disable the capturing group|`(:?a\|b)`
`\|`|Or|`cat\|dog` means cat or dog

- Character Classes

Meta-Character|Description|Example
-------------|-----------|-------
`[]`|A single character|`[abc]`=`[a-c]`=`a\|b\|c` a, b or c
`[^]`|A character except|`[^abc]` everything except a, b or c
`[-]`|A character in the range|`[a-z]` in range of a to z
`[^-]`|A character not in the range|`[^a-m]` not in range of a to m
`[--]`|A character in some ranges|`[a-zA-Z]` in range of a to z or A to Z
`[:alnum:]`|Alphanumeric characters = `[A-Za-z0-9]`|
`[:alpha:]`|Alphabetic characters = `[A-Za-z]`|
`[:blank:]`|Space and tab = `[ \t]`|
`[:cntrl:]`|Control characters = `[\x00-\x1F\x7F]`|
`[:digit:]`|Digits = `[0-9]`|
`[:graph:]`|Visible characters = `[\x21-\x7E]`|
`[:lower:]`|Lowercase letters = `[a-z]`|
`[:upper:]`|Uppercase letters = `[A-Z]`|
`[:print:]`|Visible characters and the space character = `[\x20-\x7E]`|
`[:punct:]`|Punctuation characters = ```[][!"#$%&'()*+,./:;<=>?@\^_`{\|}~-]```|
`[:space:]`|Whitespace characters = `[ \t\r\n\v\f]`|
`[:xdigit:]`|Hexadecimal digits = `[A-Fa-f0-9]`|

- Flags/Modifiers

Meta-Character|Description|Example
-------------|-----------|-------
`g`|Global does not return after the first match, restarting the subsequent searches from the end of the previous match|`/abc/g`
`m`|Multiline when enabled `^` and `$` will match the start and end of a line, instead of the whole string|`/abc/m`
`i`|Case insensitive|`/abc/i` = `/AbC/i`
`x`|Ignore Whitespace|`/abc/x`
`s`|Single line = Whole text will be analyzed as a single line|`/abc/s`
`u`|Enable encode support = Pattern strings will be treated as UTF-16|`/abc/u`
`a`|Restrict matches to ASCII only and python 2 doesn't support it|`/abc/a`

## Practical Usage in Real World

- ASCII: `[ -~]`

- Username: `^[a-z0-9_-]{3,15}$`

- Password Simple: `^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$ %^&*-]).{8,}$`

- Email Simple: `[^@ \t\r\n]+@[^@ \t\r\n]+\.[^@ \t\r\n]+`

- Email Complicated: `(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))`

- Emoji: `(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))`

- Date: `(?:(?:31(\/|-|\.)(?:0?[13578]|1[02]))\1|(?:(?:29|30)(\/|-|\.)(?:0?[13-9]|1[0-2])\2))(?:(?:1[6-9]|[2-9]\d)?\d{2})$|^(?:29(\/|-|\.)0?2\3(?:(?:(?:1[6-9]|[2-9]\d)?(?:0[48]|[2468][048]|[13579][26])|(?:(?:16|[2468][048]|[3579][26])00))))$|^(?:0?[1-9]|1\d|2[0-8])(\/|-|\.)(?:(?:0?[1-9])|(?:1[0-2]))\4(?:(?:1[6-9]|[2-9]\d)?\d{2})`

- UUID: `^[0-9a-fA-F]{8}\b-[0-9a-fA-F]{4}\b-[0-9a-fA-F]{4}\b-[0-9a-fA-F]{4}\b-[0-9a-fA-F]{12}$`

- Mac Address: `^[a-fA-F0-9]{2}(:[a-fA-F0-9]{2}){5}$`

- IPv4: `(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}`

- IPv4: `^([0-9]|[1-8][0-9]|9[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.([0-9]|[1-8][0-9]|9[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.([0-9]|[1-8][0-9]|9[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.([0-9]|[1-8][0-9]|9[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$`

- IPv6: `^\s*((([0-9A-Fa-f]{1,4}:){7}([0-9A-Fa-f]{1,4}|:))|(([0-9A-Fa-f]{1,4}:){6}(:[0-9A-Fa-f]{1,4}|((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){5}(((:[0-9A-Fa-f]{1,4}){1,2})|:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){4}(((:[0-9A-Fa-f]{1,4}){1,3})|((:[0-9A-Fa-f]{1,4})?:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){3}(((:[0-9A-Fa-f]{1,4}){1,4})|((:[0-9A-Fa-f]{1,4}){0,2}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){2}(((:[0-9A-Fa-f]{1,4}){1,5})|((:[0-9A-Fa-f]{1,4}){0,3}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){1}(((:[0-9A-Fa-f]{1,4}){1,6})|((:[0-9A-Fa-f]{1,4}){0,4}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(:(((:[0-9A-Fa-f]{1,4}){1,7})|((:[0-9A-Fa-f]{1,4}){0,5}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:)))(%.+)?\s*$`

- Bitcoin Address: `^(bc1|[13])[a-zA-HJ-NP-Z0-9]{25,39}$`

- International Phone Number: `^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$`

- Semantic Versioning: `^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$`

- HTML Tag: `<([\w]+).*>(.*?)<\/\1>`

- Find URLs in a text/string: `(http|ftp|https):\/\/([\w_-]+(?:(?:\.[\w_-]+)+))([\w.,@?^=%&:\/~+#-]*[\w@?^=%&\/~+#-])?`

- Extract Links from JavaScript Files: `(https?://)?/?[{}a-z0-9A-Z_\.-]{2,}/[{}/a-z0-9A-Z_\.-]+`

- Latitude and Longitude: `^((\-?|\+?)?\d+(\.\d+)?),\s*((\-?|\+?)?\d+(\.\d+)?)$`

- Jira Issue Ticket: `[A-Z]{2,}-\d+`
