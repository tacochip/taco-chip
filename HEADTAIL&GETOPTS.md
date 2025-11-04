# getopts, head, and tail Cheat Sheet

## getopts - Parse Command-Line Options in Shell Scripts

### Basic Syntax
```bash
getopts optstring name [args]
```

### How It Works
- Processes options one at a time in a loop
- `optstring` defines valid options (e.g., "abc" for -a, -b, -c)
- Add `:` after a letter to require an argument (e.g., "a:bc" means -a needs argument)
- `name` is the variable that receives the current option
- `OPTARG` holds the argument value for options that require one
- `OPTIND` tracks the index of the next argument to process

---

### Basic Example - Options Without Arguments
```bash
#!/bin/bash

while getopts "abc" opt; do
    case $opt in
        a)
            echo "Option -a was triggered"
            ;;
        b)
            echo "Option -b was triggered"
            ;;
        c)
            echo "Option -c was triggered"
            ;;
        \?)
            echo "Invalid option: -$OPTARG" >&2
            exit 1
            ;;
    esac
done

# Usage: ./script.sh -a -b -c
# Or: ./script.sh -abc
```

---

### Options With Arguments
```bash
#!/bin/bash

while getopts "f:o:v" opt; do
    case $opt in
        f)
            file="$OPTARG"
            echo "File: $file"
            ;;
        o)
            output="$OPTARG"
            echo "Output: $output"
            ;;
        v)
            verbose=true
            echo "Verbose mode enabled"
            ;;
        \?)
            echo "Invalid option: -$OPTARG" >&2
            exit 1
            ;;
        :)
            echo "Option -$OPTARG requires an argument" >&2
            exit 1
            ;;
    esac
done

# Usage: ./script.sh -f input.txt -o output.txt -v
```

---

### Silent Error Reporting (Leading Colon)
```bash
#!/bin/bash

# Leading : in optstring enables silent error reporting
while getopts ":a:b:c" opt; do
    case $opt in
        a)
            echo "Option -a: $OPTARG"
            ;;
        b)
            echo "Option -b: $OPTARG"
            ;;
        c)
            echo "Option -c"
            ;;
        :)
            # Missing argument
            echo "Option -$OPTARG requires an argument" >&2
            exit 1
            ;;
        \?)
            # Invalid option
            echo "Invalid option: -$OPTARG" >&2
            exit 1
            ;;
    esac
done

# The leading : prevents getopts from printing its own error messages
```

---

### Processing Remaining Arguments
```bash
#!/bin/bash

while getopts "f:v" opt; do
    case $opt in
        f)
            file="$OPTARG"
            ;;
        v)
            verbose=true
            ;;
    esac
done

# Shift past the processed options
shift $((OPTIND - 1))

# Now $@ contains only non-option arguments
echo "Remaining arguments: $@"

# Usage: ./script.sh -f test.txt -v arg1 arg2 arg3
# Output: Remaining arguments: arg1 arg2 arg3
```

---

### Complete Real-World Example
```bash
#!/bin/bash

usage() {
    cat << EOF
Usage: $0 [-h] [-i INPUT] [-o OUTPUT] [-v] [-n NUM]

Options:
    -h          Show this help message
    -i INPUT    Input file (required)
    -o OUTPUT   Output file (default: output.txt)
    -v          Enable verbose mode
    -n NUM      Number of iterations (default: 1)

Example:
    $0 -i data.txt -o results.txt -v -n 5
EOF
    exit 1
}

# Default values
output="output.txt"
verbose=false
iterations=1
input=""

while getopts ":hi:o:vn:" opt; do
    case $opt in
        h)
            usage
            ;;
        i)
            input="$OPTARG"
            ;;
        o)
            output="$OPTARG"
            ;;
        v)
            verbose=true
            ;;
        n)
            iterations="$OPTARG"
            # Validate that it's a number
            if ! [[ "$iterations" =~ ^[0-9]+$ ]]; then
                echo "Error: -n requires a numeric argument" >&2
                exit 1
            fi
            ;;
        :)
            echo "Error: Option -$OPTARG requires an argument" >&2
            usage
            ;;
        \?)
            echo "Error: Invalid option -$OPTARG" >&2
            usage
            ;;
    esac
done

# Check required arguments
if [ -z "$input" ]; then
    echo "Error: Input file is required (-i)" >&2
    usage
fi

# Check if input file exists
if [ ! -f "$input" ]; then
    echo "Error: Input file '$input' not found" >&2
    exit 1
fi

# Main script logic
[ "$verbose" = true ] && echo "Processing $input..."
[ "$verbose" = true ] && echo "Output will be written to $output"
[ "$verbose" = true ] && echo "Running $iterations iteration(s)"

# Your actual script logic here
echo "Script would process: $input -> $output (x$iterations)"
```

---

### Common Patterns

#### Boolean flags only
```bash
#!/bin/bash
debug=false
verbose=false

while getopts "dv" opt; do
    case $opt in
        d) debug=true ;;
        v) verbose=true ;;
    esac
done
```

#### Multiple options with arguments
```bash
#!/bin/bash
while getopts "u:p:h:d:" opt; do
    case $opt in
        u) username="$OPTARG" ;;
        p) password="$OPTARG" ;;
        h) host="$OPTARG" ;;
        d) database="$OPTARG" ;;
    esac
done
```

#### Mix of required and optional arguments
```bash
#!/bin/bash
while getopts ":i:o:f:v" opt; do
    case $opt in
        i) input="$OPTARG" ;;      # Required argument
        o) output="$OPTARG" ;;     # Optional argument
        f) format="$OPTARG" ;;     # Optional argument
        v) verbose=true ;;         # Boolean flag
    esac
done
```

---

### Important Notes

1. **getopts only handles single-character options** (e.g., `-a`, not `--all`)
2. **Options can be combined** (e.g., `-abc` is same as `-a -b -c`)
3. **Options must come before positional arguments**
4. **Use `getopt` (different tool) for long options like `--help`**
5. **Leading `:` in optstring enables silent mode**
6. **Always validate arguments that require specific formats**

---

## head - Output First Part of Files

### Basic Syntax
```bash
head [options] [file...]
```

---

### Common Flags

#### `-n NUM` or `-NUM` - Show first NUM lines (default: 10)
```bash
head -n 5 file.txt
# Shows first 5 lines

head -20 file.txt
# Shows first 20 lines (shorthand)

head file.txt
# Shows first 10 lines (default)
```

#### `-c NUM` - Show first NUM bytes
```bash
head -c 100 file.txt
# Shows first 100 bytes

head -c 1K file.txt
# Shows first 1 kilobyte (K, M, G, T suffixes supported)

head -c 50 file.txt
# Shows first 50 characters
```

#### `-q` - Quiet mode (suppress filenames with multiple files)
```bash
head -n 5 -q file1.txt file2.txt
# Doesn't print "==> file1.txt <==" headers
```

#### `-v` - Verbose mode (always show filenames)
```bash
head -n 5 -v file.txt
# Prints "==> file.txt <==" even for single file
```

---

### Practical Examples

#### Preview a file
```bash
head file.txt
# Quick peek at first 10 lines
```

#### Check CSV headers
```bash
head -n 1 data.csv
# Shows column names
```

#### View log file start
```bash
head -n 50 /var/log/syslog
# Check first 50 log entries
```

#### Preview multiple files
```bash
head -n 3 *.txt
# Shows first 3 lines of each .txt file
```

#### Show first line of all files
```bash
head -n 1 *.log
# Useful for checking headers across multiple files
```

#### Check file encoding/BOM
```bash
head -c 3 file.txt | od -A x -t x1z
# Check for UTF-8 BOM or other byte markers
```

#### Combine with other commands
```bash
# Get first 5 results from a search
grep "error" logfile.txt | head -n 5

# First 10 largest files
ls -lS | head -n 11

# First 20 processes by memory
ps aux --sort=-%mem | head -n 21

# Preview compressed file
zcat file.gz | head -n 10
```

#### Read from stdin
```bash
# First 5 lines from command output
ls -l | head -n 5

# First 100 characters from echo
echo "This is a long string..." | head -c 100
```

#### Sample a percentage
```bash
# Get approximately first 10% of lines
total_lines=$(wc -l < file.txt)
head -n $((total_lines / 10)) file.txt
```

---

### Special Use Cases

#### Skip files that are too short
```bash
# Only show files with at least 10 lines
for file in *.txt; do
    if [ $(wc -l < "$file") -ge 10 ]; then
        head "$file"
    fi
done
```

#### Compare file beginnings
```bash
diff <(head file1.txt) <(head file2.txt)
# Compare first 10 lines of two files
```

#### Monitor file growth (one-time check)
```bash
head -n 5 growing.log
# ... wait ...
head -n 5 growing.log
# Manually compare
```

---

## tail - Output Last Part of Files

### Basic Syntax
```bash
tail [options] [file...]
```

---

### Common Flags

#### `-n NUM` or `-NUM` - Show last NUM lines (default: 10)
```bash
tail -n 20 file.txt
# Shows last 20 lines

tail -5 file.txt
# Shows last 5 lines (shorthand)

tail file.txt
# Shows last 10 lines (default)
```

#### `-n +NUM` - Show everything starting from line NUM
```bash
tail -n +5 file.txt
# Shows from line 5 to end (skips first 4 lines)

tail -n +1 file.txt
# Shows entire file (skips nothing)
```

#### `-c NUM` - Show last NUM bytes
```bash
tail -c 100 file.txt
# Shows last 100 bytes

tail -c 1K file.txt
# Shows last 1 kilobyte

tail -c +50 file.txt
# Shows from byte 50 to end
```

#### `-f` - Follow mode (watch file for new content)
```bash
tail -f /var/log/syslog
# Continuously shows new lines as they're added
# Press Ctrl+C to exit
```

#### `-F` - Follow with retry (handles log rotation)
```bash
tail -F /var/log/apache2/access.log
# Keeps following even if file is rotated/recreated
# Automatically reopens if file is replaced
```

#### `-s NUM` - Sleep interval for follow mode (default: 1 second)
```bash
tail -f -s 5 file.txt
# Checks for updates every 5 seconds instead of 1
```

#### `--pid=PID` - Stop following when process dies
```bash
tail -f --pid=1234 app.log
# Stops following when process 1234 terminates
```

#### `-q` - Quiet mode (suppress filenames)
```bash
tail -n 5 -q file1.txt file2.txt
# No "==> filename <==" headers
```

#### `-v` - Verbose mode (always show filenames)
```bash
tail -n 5 -v file.txt
# Shows "==> file.txt <==" header
```

#### `--retry` - Keep trying to open a file
```bash
tail -f --retry file.txt
# Keeps trying even if file doesn't exist yet
```

---

### Practical Examples

#### View end of log file
```bash
tail /var/log/syslog
# Last 10 log entries
```

#### Monitor real-time logs
```bash
tail -f /var/log/apache2/error.log
# Watch errors as they happen
```

#### Follow multiple log files
```bash
tail -f /var/log/syslog /var/log/auth.log
# Monitor both files simultaneously
```

#### Skip header lines
```bash
tail -n +2 data.csv
# Skip CSV header (show from line 2 onward)
```

#### Show last 100 lines
```bash
tail -n 100 large.log
# Quick view of recent activity
```

#### Check recent errors
```bash
tail -n 50 error.log | grep "CRITICAL"
# Last 50 lines, filtered for critical errors
```

#### Monitor application output
```bash
./myapp > output.log &
tail -f output.log
# Watch application output in real-time
```

#### Follow log until process ends
```bash
./myapp &
APP_PID=$!
tail -f --pid=$APP_PID output.log
# Automatically stops when myapp finishes
```

#### Watch log with grep filter
```bash
tail -f access.log | grep "404"
# Show only 404 errors in real-time
```

#### Multiple files with pattern
```bash
tail -f /var/log/*.log
# Monitor all .log files in directory
```

---

### Advanced Examples

#### Rotating log files
```bash
# Use -F (capital F) for rotated logs
tail -F /var/log/application.log
# Handles logrotate automatically
```

#### Show last N lines from multiple files
```bash
tail -n 5 error.log access.log debug.log
# Last 5 lines from each file
```

#### Combine with grep and awk
```bash
tail -f access.log | grep "POST" | awk '{print $1, $7}'
# Real-time monitoring of POST requests, showing IP and path
```

#### Monitor with timestamps
```bash
tail -f app.log | while read line; do
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $line"
done
# Add timestamps to lines without them
```

#### Follow until pattern appears
```bash
tail -f deployment.log | grep -m 1 "Deployment complete"
# Stops after first match
```

#### Compare last lines of files
```bash
diff <(tail file1.txt) <(tail file2.txt)
# Compare last 10 lines of two files
```

#### Get lines between head and tail
```bash
head -n 20 file.txt | tail -n 5
# Shows lines 16-20 (last 5 of first 20)

tail -n +10 file.txt | head -n 5
# Shows lines 10-14 (first 5 starting from line 10)
```

#### Monitor multiple files with labels
```bash
tail -f access.log error.log
# Shows "==> access.log <==" headers automatically
```

#### Extract specific line range
```bash
# Get lines 50-60
tail -n +50 file.txt | head -n 11

# Alternative method
sed -n '50,60p' file.txt
```

#### Real-time word count
```bash
tail -f logfile.txt | wc -l
# Won't update in real-time (buffering issue)

# Better approach:
tail -f logfile.txt | while read line; do
    count=$((count + 1))
    echo "Lines: $count"
done
```

#### Monitor with color highlighting
```bash
tail -f /var/log/syslog | grep --color=auto -E "error|warning|critical|$"
# Highlights keywords in color ($ matches everything)
```

#### Follow compressed logs
```bash
# Won't work (can't follow compressed files)
tail -f logfile.gz

# Use zcat and tail instead
zcat logfile.gz | tail -n 100

# For monitoring growing compressed files, use:
tail -c +1 logfile.gz | zcat | tail -f
```

---

### head + tail Combinations

#### Extract specific lines
```bash
# Get line 50
head -n 50 file.txt | tail -n 1

# Get lines 40-50
head -n 50 file.txt | tail -n 11

# Skip first 10, show next 20
tail -n +11 file.txt | head -n 20
```

#### Sample random middle section
```bash
# Show 10 lines starting from line 100
tail -n +100 file.txt | head -n 10
```

#### Preview and end of file
```bash
(head -n 5 file.txt; echo "..."; tail -n 5 file.txt)
# Shows first 5, separator, last 5
```

---

### Performance Tips

1. **Use `-n +NUM` with tail to skip lines efficiently**
   ```bash
   tail -n +1000000 huge.txt  # Skip first million lines
   ```

2. **For large files, tail is faster than head for end-of-file operations**

3. **Combine with `grep` for filtered monitoring**
   ```bash
   tail -f app.log | grep --line-buffered "ERROR"
   ```

4. **Use `-F` instead of `-f` for rotated logs** to avoid missing data

5. **For real-time monitoring, pipe to `grep --line-buffered`** to avoid buffering delays

---

### Common Use Cases Summary

#### Development & Debugging
```bash
# Monitor application logs
tail -f app.log

# Check latest errors
tail -n 100 error.log | grep "Exception"

# Watch test output
pytest | tail -f
```

#### System Administration
```bash
# Monitor system logs
tail -f /var/log/syslog

# Check authentication attempts
tail -n 50 /var/log/auth.log

# Watch Apache access log
tail -F /var/log/apache2/access.log
```

#### Data Processing
```bash
# Skip CSV header
tail -n +2 data.csv

# Get last 1000 records
tail -n 1000 large_dataset.csv

# Preview file structure
(head -n 3; echo "..."; tail -n 3) < data.txt
```
