# Regular Expressions (Regex) Cheat Sheet

## Basic Syntax

### Literal Characters
```regex
cat         # Matches "cat" exactly
hello world # Matches "hello world"
```

---

## Character Classes

### `[...]` - Character set (matches any single character inside brackets)
```regex
[aeiou]     # Matches any vowel
[0-9]       # Matches any digit
[A-Z]       # Matches any uppercase letter
[a-zA-Z]    # Matches any letter
[^0-9]      # Matches any NON-digit (^ negates)
```

**Examples:**
- `gr[ae]y` matches "gray" or "grey"
- `[0-9][0-9]` matches any two-digit number
- `[^aeiou]` matches any consonant or non-letter

### Predefined Character Classes

#### `\d` - Any digit (equivalent to `[0-9]`)
```regex
\d\d\d      # Matches: 123, 456, 789
```

#### `\D` - Any non-digit (equivalent to `[^0-9]`)
```regex
\D+         # Matches: abc, xyz, !!!
```

#### `\w` - Word character (letters, digits, underscore: `[a-zA-Z0-9_]`)
```regex
\w+         # Matches: hello, test123, user_name
```

#### `\W` - Non-word character (equivalent to `[^a-zA-Z0-9_]`)
```regex
\W+         # Matches: !!!, @#$, ...
```

#### `\s` - Whitespace (space, tab, newline)
```regex
\s+         # Matches one or more whitespace characters
```

#### `\S` - Non-whitespace
```regex
\S+         # Matches any sequence of non-whitespace
```

#### `.` - Any character except newline
```regex
c.t         # Matches: cat, cot, c9t, c@t
```

---

## Anchors

### `^` - Start of line
```regex
^Hello      # Matches "Hello" only at the start of a line
^[A-Z]      # Line must start with uppercase letter
```

### `$` - End of line
```regex
end$        # Matches "end" only at the end of a line
\.$         # Line must end with a period
```

### `\b` - Word boundary
```regex
\bcat\b     # Matches "cat" but not "catch" or "concatenate"
\btest      # Matches "test" and "testing" but not "contest"
```

### `\B` - Non-word boundary
```regex
\Bcat\B     # Matches "cat" in "concatenate" but not standalone "cat"
```

**Examples:**
- `^\d{3}$` matches exactly three digits on a line
- `^$` matches empty lines
- `\bhello\b` matches the word "hello" as a complete word

---

## Quantifiers

### `*` - Zero or more times
```regex
ab*c        # Matches: ac, abc, abbc, abbbc
\d*         # Matches: "", "1", "123", "999999"
```

### `+` - One or more times
```regex
ab+c        # Matches: abc, abbc, abbbc (NOT "ac")
\d+         # Matches: "1", "123", "999999" (NOT "")
```

### `?` - Zero or one time (optional)
```regex
colou?r     # Matches: color, colour
https?      # Matches: http, https
```

### `{n}` - Exactly n times
```regex
\d{3}       # Matches: 123, 456 (exactly 3 digits)
a{5}        # Matches: aaaaa (exactly 5 a's)
```

### `{n,}` - n or more times
```regex
\d{3,}      # Matches: 123, 1234, 12345 (3+ digits)
a{2,}       # Matches: aa, aaa, aaaa (2+ a's)
```

### `{n,m}` - Between n and m times
```regex
\d{2,4}     # Matches: 12, 123, 1234 (2-4 digits)
a{1,3}      # Matches: a, aa, aaa (1-3 a's)
```

**Examples:**
- `^\d{3}-\d{4}$` matches phone numbers like "555-1234"
- `[A-Z][a-z]+` matches capitalized words
- `\w{5,10}` matches words 5-10 characters long

---

## Greedy vs Lazy Quantifiers

### Greedy (default) - Matches as much as possible
```regex
<.*>        # In "<b>text</b>", matches entire "<b>text</b>"
```

### Lazy - Matches as little as possible (add `?` after quantifier)
```regex
<.*?>       # In "<b>text</b>", matches "<b>" and "</b>" separately
\d+?        # Matches minimum digits needed
```

**Examples:**
- `".*"` matches `"hello" and "world"` (entire string)
- `".*?"` matches `"hello"` and `"world"` separately

---

## Alternation

### `|` - OR operator
```regex
cat|dog     # Matches: cat OR dog
red|blue|green  # Matches any of the three colors
(Mr|Mrs|Ms)\.   # Matches: Mr., Mrs., Ms.
```

**Examples:**
- `^(yes|no)$` matches "yes" or "no" on a line
- `\.(jpg|png|gif)$` matches file extensions

---

## Grouping and Capturing

### `(...)` - Capturing group
```regex
(\d{3})-(\d{4})     # Captures area code and number separately
(hello|hi) world    # Captures greeting separately
```

**Use in replacements:**
```bash
# Swap first and last name
s/(\w+) (\w+)/\2, \1/
# Input: John Doe
# Output: Doe, John
```

### `(?:...)` - Non-capturing group
```regex
(?:cat|dog)s?       # Groups alternatives but doesn't capture
(?:\d{3})-\d{4}     # Groups without capturing
```

### Backreferences - Reference captured groups
```regex
(\w+) \1            # Matches repeated words: "hello hello"
(["']).*?\1         # Matches quoted strings with same quote type
```

**Examples:**
- `(\d{2})/(\d{2})/(\d{4})` captures date parts
- `<(\w+)>.*?</\1>` matches HTML tags: `<div>...</div>`
- `\b(\w+)\s+\1\b` finds duplicate words

---

## Lookahead and Lookbehind

### `(?=...)` - Positive lookahead
```regex
\d+(?= dollars)     # Matches numbers before " dollars"
# "50 dollars" matches "50"
```

### `(?!...)` - Negative lookahead
```regex
\d+(?! dollars)     # Matches numbers NOT before " dollars"
# "50 euros" matches "50"
```

### `(?<=...)` - Positive lookbehind
```regex
(?<=\$)\d+          # Matches numbers after "$"
# "$50" matches "50"
```

### `(?<!...)` - Negative lookbehind
```regex
(?<!\$)\d+          # Matches numbers NOT after "$"
# "50" matches but "$50" doesn't match "50"
```

**Examples:**
- `(?<=@)\w+(?=\.)` matches domain in "user@example.com" → "example"
- `\b\w+(?=ing\b)` matches words ending in "ing" without "ing"

---

## Escape Sequences

### Special characters that need escaping
```regex
\. \* \+ \? \^ \$ \[ \] \{ \} \( \) \| \\
```

**Examples:**
```regex
\$\d+\.\d{2}        # Matches: $19.99, $5.00
\(optional\)        # Matches: (optional)
file\.txt           # Matches: file.txt
```

---

## Flags/Modifiers

### `i` - Case insensitive
```regex
/hello/i            # Matches: hello, Hello, HELLO, HeLLo
```

### `g` - Global (find all matches)
```regex
/cat/g              # Finds all occurrences of "cat"
```

### `m` - Multiline (^ and $ match line starts/ends)
```regex
/^start/m           # Matches "start" at beginning of any line
```

### `s` - Dotall (. matches newlines)
```regex
/begin.*end/s       # Matches across multiple lines
```

### `x` - Extended (ignore whitespace, allow comments)
```regex
/ \d{3}  # area code
  -      # separator
  \d{4}  # number
/x
```

---

## Common Patterns

### Email validation (basic)
```regex
\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b
```

### URL
```regex
https?://[^\s]+
https?://[\w\-]+(\.[\w\-]+)+[/#?]?.*$
```

### Phone number (US format)
```regex
\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}
# Matches: (555) 123-4567, 555-123-4567, 555.123.4567
```

### IP Address
```regex
\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b
# Better version with range validation:
\b(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\b
```

### Date formats
```regex
\d{4}-\d{2}-\d{2}           # 2024-12-31
\d{2}/\d{2}/\d{4}           # 12/31/2024
\d{1,2}-[A-Za-z]{3}-\d{4}   # 31-Dec-2024
```

### Credit card (basic pattern, not validation)
```regex
\b\d{4}[-\s]?\d{4}[-\s]?\d{4}[-\s]?\d{4}\b
```

### Hexadecimal color
```regex
#[0-9A-Fa-f]{6}\b           # #FF5733
#[0-9A-Fa-f]{3}\b           # #F57
```

### Username (alphanumeric and underscore, 3-16 chars)
```regex
^[a-zA-Z0-9_]{3,16}$
```

### Strong password (min 8 chars, 1 uppercase, 1 lowercase, 1 digit, 1 special)
```regex
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$
```

### HTML tags
```regex
<([a-z]+)([^<]+)*(?:>(.*)<\/\1>|\s+\/>)
<\/?[\w\s]*>|<.+[\W]>       # Simpler version
```

### Extract numbers from text
```regex
-?\d+\.?\d*                 # Integers and decimals
```

### File path
```regex
([A-Za-z]:)?[\\\/]?[\w\-\\\/\.]+
```

---

## Practical Examples by Use Case

### Validation Examples

**Valid email check:**
```regex
^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$
```

**Valid US ZIP code:**
```regex
^\d{5}(-\d{4})?$            # Matches: 12345 or 12345-6789
```

**Valid time (24-hour):**
```regex
^([01]\d|2[0-3]):([0-5]\d)$  # Matches: 00:00 to 23:59
```

### Extraction Examples

**Extract all URLs from text:**
```regex
https?://[^\s]+
```

**Extract hashtags:**
```regex
#\w+
```

**Extract @mentions:**
```regex
@\w+
```

**Extract all numbers:**
```regex
\b\d+\.?\d*\b
```

### Substitution Examples

**Remove all whitespace:**
```regex
\s+                         # Replace with ""
```

**Normalize phone numbers:**
```regex
\(?(\d{3})\)?[-.\s]?(\d{3})[-.\s]?(\d{4})
# Replace with: ($1) $2-$3
```

**Convert camelCase to snake_case:**
```regex
([a-z0-9])([A-Z])
# Replace with: $1_$2
```

### Cleaning Examples

**Remove HTML tags:**
```regex
<[^>]+>                     # Replace with ""
```

**Remove extra spaces:**
```regex
\s{2,}                      # Replace with single space
```

**Remove leading/trailing whitespace:**
```regex
^\s+|\s+$                   # Replace with ""
```

---

## Testing Your Regex

Use these tools to test and debug your regular expressions:
- **regex101.com** - Interactive tester with explanations
- **regexr.com** - Visual regex tester
- **regextester.com** - Simple online tester

**Command line testing:**
```bash
# grep with Perl regex
echo "test123" | grep -P '\d+'

# Python
python -c "import re; print(re.findall(r'\d+', 'test123'))"

# JavaScript (Node.js)
node -e "console.log('test123'.match(/\d+/g))"
```

---

## Quick Tips

1. **Start simple** - Build complex patterns incrementally
2. **Use raw strings** - In most languages: `r'\d+'` (Python) or `/\d+/` (JavaScript)
3. **Test edge cases** - Empty strings, special characters, extreme lengths
4. **Be specific** - `\d{3}` is better than `\d+` for 3-digit codes
5. **Avoid catastrophic backtracking** - Be careful with nested quantifiers like `(a+)+`
6. **Use non-capturing groups** - `(?:...)` when you don't need the capture
7. **Escape when in doubt** - It's safer to escape special characters
8. **Comment complex regex** - Use the `x` flag for readability
