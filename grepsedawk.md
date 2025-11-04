# grep, sed, and awk Cheat Sheet

## grep - Search Text Using Patterns

### Basic Syntax
```bash
grep [options] pattern [file...]
```

### Common Flags

#### `-i` - Case insensitive search
```bash
grep -i "error" logfile.txt
# Matches: error, Error, ERROR, ErRoR
```

#### `-v` - Invert match (show lines that DON'T match)
```bash
grep -v "success" logfile.txt
# Shows all lines without "success"
```

#### `-r` or `-R` - Recursive search through directories
```bash
grep -r "TODO" /path/to/project/
# Searches all files in directory and subdirectories
```

#### `-n` - Show line numbers
```bash
grep -n "function" script.py
# Output: 15:def function():
```

#### `-c` - Count matching lines
```bash
grep -c "error" logfile.txt
# Output: 42
```

#### `-l` - Show only filenames with matches
```bash
grep -l "import" *.py
# Output: main.py utils.py config.py
```

#### `-w` - Match whole words only
```bash
grep -w "cat" animals.txt
# Matches "cat" but not "catch" or "concatenate"
```

#### `-A NUM` - Show NUM lines after match
```bash
grep -A 3 "ERROR" logfile.txt
# Shows error line + 3 lines after
```

#### `-B NUM` - Show NUM lines before match
```bash
grep -B 2 "ERROR" logfile.txt
# Shows 2 lines before + error line
```

#### `-C NUM` - Show NUM lines before and after match
```bash
grep -C 2 "ERROR" logfile.txt
# Shows 2 lines before + error line + 2 lines after
```

#### `-E` - Use extended regex (same as egrep)
```bash
grep -E "error|warning|critical" logfile.txt
# Matches any of the three patterns
```

#### `-o` - Show only the matching part
```bash
grep -o "[0-9]\+" data.txt
# Output: 123
#         456
```

#### `-q` - Quiet mode (exit status only)
```bash
if grep -q "error" logfile.txt; then
    echo "Errors found!"
fi
```

#### `-x` - Match entire line exactly
```bash
grep -x "exact match" file.txt
# Only matches if the entire line is "exact match"
```

---

## sed - Stream Editor

### Basic Syntax
```bash
sed [options] 'command' [file...]
```

### Common Flags and Commands

#### `-i` - Edit file in-place
```bash
sed -i 's/old/new/g' file.txt
# Modifies file.txt directly (use -i.bak to create backup)
```

#### `-e` - Execute multiple commands
```bash
sed -e 's/foo/bar/' -e 's/cat/dog/' file.txt
# Applies both substitutions
```

#### `-n` - Suppress automatic output (use with p command)
```bash
sed -n '5p' file.txt
# Prints only line 5
```

### Substitution Commands

#### `s/pattern/replacement/` - Basic substitution (first occurrence)
```bash
sed 's/cat/dog/' pets.txt
# Replaces first "cat" on each line with "dog"
```

#### `s/pattern/replacement/g` - Global substitution (all occurrences)
```bash
sed 's/cat/dog/g' pets.txt
# Replaces all "cat" instances with "dog"
```

#### `s/pattern/replacement/2` - Replace nth occurrence
```bash
sed 's/cat/dog/2' pets.txt
# Replaces only the 2nd "cat" on each line
```

#### `s/pattern/replacement/i` - Case insensitive
```bash
sed 's/error/warning/gi' logfile.txt
# Replaces error/Error/ERROR globally
```

### Line Selection

#### `[address]d` - Delete lines
```bash
sed '3d' file.txt           # Delete line 3
sed '2,5d' file.txt         # Delete lines 2-5
sed '/pattern/d' file.txt   # Delete lines matching pattern
```

#### `[address]p` - Print lines
```bash
sed -n '1,10p' file.txt     # Print lines 1-10
sed -n '/error/p' file.txt  # Print lines with "error"
```

#### `[address]a\text` - Append text after line
```bash
sed '3a\New line here' file.txt
# Adds "New line here" after line 3
```

#### `[address]i\text` - Insert text before line
```bash
sed '1i\Header line' file.txt
# Inserts "Header line" before line 1
```

#### `[address]c\text` - Replace entire line
```bash
sed '2c\Completely new line' file.txt
# Replaces line 2 entirely
```

### Advanced Examples

#### Multiple operations in one command
```bash
sed '1d; s/foo/bar/g; /error/d' file.txt
```

#### Using regex capturing groups
```bash
sed 's/\([0-9]\{3\}\)-\([0-9]\{4\}\)/(\1) \2/' phone.txt
# 555-1234 becomes (555) 1234
```

#### Range-based substitution
```bash
sed '10,20s/old/new/g' file.txt
# Substitute only on lines 10-20
```

---

## awk - Pattern Scanning and Processing

### Basic Syntax
```bash
awk [options] 'pattern { action }' [file...]
```

### Common Flags

#### `-F` - Field separator
```bash
awk -F':' '{print $1}' /etc/passwd
# Uses colon as separator, prints first field
```

#### `-v` - Define variable
```bash
awk -v threshold=100 '$3 > threshold {print $0}' data.txt
# Uses variable threshold in condition
```

### Built-in Variables

```bash
NR    # Current record (line) number
NF    # Number of fields in current record
$0    # Entire current line
$1-$n # Individual fields (columns)
FS    # Field separator (default: whitespace)
OFS   # Output field separator (default: space)
RS    # Record separator (default: newline)
ORS   # Output record separator (default: newline)
```

### Basic Usage Examples

#### Print specific columns
```bash
awk '{print $1, $3}' data.txt
# Prints 1st and 3rd columns
```

#### Print with custom separator
```bash
awk -F',' '{print $1 "\t" $2}' data.csv
# CSV input, tab-separated output
```

#### Print line numbers
```bash
awk '{print NR, $0}' file.txt
# Output: 1 First line
#         2 Second line
```

#### Filter lines by condition
```bash
awk '$3 > 100' data.txt
# Prints lines where 3rd field > 100
```

#### Pattern matching
```bash
awk '/error/ {print $0}' logfile.txt
# Prints lines containing "error"
```

### Advanced Examples

#### Calculate sum of a column
```bash
awk '{sum += $2} END {print sum}' numbers.txt
# Sums all values in 2nd column
```

#### Calculate average
```bash
awk '{sum += $1; count++} END {print sum/count}' data.txt
```

#### Count occurrences
```bash
awk '{count[$1]++} END {for (word in count) print word, count[word]}' file.txt
# Counts frequency of values in first column
```

#### BEGIN and END blocks
```bash
awk 'BEGIN {print "Starting..."} {print $0} END {print "Done!"}' file.txt
```

#### Conditional formatting
```bash
awk '$3 > 100 {print $1 " - HIGH"} $3 <= 100 {print $1 " - NORMAL"}' data.txt
```

#### Multiple field separators
```bash
awk -F'[,:]' '{print $1, $3}' data.txt
# Splits on comma OR colon
```

#### Print specific range of lines
```bash
awk 'NR==10, NR==20' file.txt
# Prints lines 10 through 20
```

#### Custom output formatting
```bash
awk '{printf "%-10s %5d\n", $1, $2}' data.txt
# Left-aligned string, right-aligned number
```

#### Field manipulation
```bash
awk -F',' 'BEGIN {OFS="|"} {print $1, $2, $3}' input.csv
# Converts CSV to pipe-separated
```

#### Multi-line processing
```bash
awk '{total += $1} {print $0, "Running total:", total}' numbers.txt
# Shows running total with each line
```

---

## Quick Comparison Examples

### Count lines containing "error"
```bash
grep -c "error" file.txt
sed -n '/error/p' file.txt | wc -l
awk '/error/ {count++} END {print count}' file.txt
```

### Replace text
```bash
grep "old" file.txt | sed 's/old/new/g'
sed 's/old/new/g' file.txt
awk '{gsub(/old/, "new"); print}' file.txt
```

### Print specific lines
```bash
grep -n "pattern" file.txt
sed -n '/pattern/=' file.txt
awk '/pattern/ {print NR, $0}' file.txt
```
