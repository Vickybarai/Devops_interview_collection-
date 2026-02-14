# Module 8: Shell Scripting

## Table of Contents
1. [What is a Shell Script?](#1-what-is-a-shell-script)
2. [How to Create and Run a Shell Script](#2-how-to-create-and-run-a-shell-script)
3. [What are Shell Variables?](#3-what-are-shell-variables)
4. [What are Environment Variables vs Shell Variables?](#4-what-are-environment-variables-vs-shell-variables)
5. [What is Shebang (#!/bin/bash)?](#5-what-is-shebang-usrbinbash)
6. [How to Use Conditional Statements (if-else)](#6-how-to-use-conditional-statements-if-else)
7. [How to Use Loops (for, while)](#7-how-to-use-loops-for-while)
8. [How to Use Functions in Shell Script](#8-how-to-use-functions-in-shell-script)
9. [How to Read User Input](#9-how-to-read-user-input)
10. [How to Pass Arguments to a Script](#10-how-to-pass-arguments-to-a-script)

---

## 1. What is a Shell Script?

### Definition
A shell script is a computer program designed to be run by the Unix/Linux shell. It's a text file containing a series of commands that the shell executes one by one.

### Explanation (For Beginners)
- Think of a shell script as a **recipe** for your computer
- Instead of typing commands one by one manually, you write them all in a file
- When you run the script, the shell reads and executes each command automatically
- This saves time and reduces mistakes when you need to do the same task repeatedly
- Shell scripts are written in plain text and can be edited with any text editor
- The most common shell for scripting is **Bash** (Bourne Again SHell)

**Why use shell scripts?**
- Automate repetitive tasks (backups, file management, system monitoring)
- Schedule tasks to run automatically (via cron jobs)
- Create custom commands for complex operations
- Make your work faster and more consistent

### Use
- Automate daily system administration tasks
- Create startup scripts for applications
- Monitor system health and send alerts
- Perform batch operations on files
- Deploy applications and services
- Create custom tools and utilities

### Command
```bash
# Create a simple shell script
cat > hello.sh << 'EOF'
#!/bin/bash
echo "Hello, World!"
echo "This is my first shell script"
EOF

# Make it executable
chmod +x hello.sh

# Run the script
./hello.sh

# Alternative way to run (without executable permission)
bash hello.sh
```

---

## 2. How to Create and Run a Shell Script

### Definition
Creating a shell script involves writing commands in a text file with a `.sh` extension and making it executable, then running it using the shell.

### Explanation (For Beginners)
**Creating a shell script:**
1. Open a text editor (vim, nano, or any editor you prefer)
2. Start with a **shebang line** (#!/bin/bash) - this tells the system which interpreter to use
3. Write your commands, one per line (or use semicolons for multiple commands on one line)
4. Save the file with a `.sh` extension

**Making it executable:**
- Use `chmod +x filename.sh` to give execute permission
- This allows you to run the script directly with `./filename.sh`

**Running the script:**
- `./script.sh` - Run directly (needs execute permission)
- `bash script.sh` - Run with bash (no execute permission needed)
- `sh script.sh` - Run with sh (more compatible, but fewer features)

**Best Practices:**
- Always add comments (#) to explain what your script does
- Use descriptive variable names
- Test your script with `bash -x script.sh` to see each command as it executes
- Add error handling with `set -e` to stop on errors

### Use
- Create reusable scripts for common tasks
- Automate system maintenance
- Build deployment pipelines
- Create monitoring scripts
- Develop custom command-line tools

### Command
```bash
# Create a script
nano myscript.sh

# Add this content:
#!/bin/bash
# This is a comment
echo "Starting backup..."
date
echo "Backup completed"

# Save and exit (Ctrl+X, then Y, then Enter in nano)

# Make executable
chmod +x myscript.sh

# Run the script
./myscript.sh

# Run with debugging (shows each command)
bash -x myscript.sh

# Check syntax without running
bash -n myscript.sh
```

---

## 3. What are Shell Variables?

### Definition
Shell variables are named storage locations that hold data (text or numbers) that can be used and manipulated within a shell script.

### Explanation (For Beginners)
- Variables are like **containers** or **boxes** where you can store information
- Each variable has a **name** and a **value**
- You can store text (strings), numbers, or command outputs in variables
- Variable names are case-sensitive (Name and name are different)

**Creating variables:**
- No spaces around the `=` sign
- Variable name on the left, value on the right
- Example: `NAME="John"`

**Using variables:**
- Use `$` before the variable name to access its value
- Example: `echo $NAME` will print "John"

**Variable naming rules:**
- Start with a letter or underscore
- Can contain letters, numbers, and underscores
- Don't use reserved words (if, then, else, etc.)
- Use uppercase for environment variables, lowercase for local variables (convention)

**Types of data:**
- Strings (text): `MESSAGE="Hello World"`
- Numbers: `COUNT=10`
- Command output: `DATE=$(date)`

### Use
- Store user input
- Hold configuration values
- Store command results for later use
- Pass data between functions
- Build dynamic commands
- Track counters and states in loops

### Command
```bash
#!/bin/bash

# Creating variables
name="Alice"
age=25
city="New York"

# Using variables
echo "Name: $name"
echo "Age: $age"
echo "City: $city"

# Variables with spaces (use quotes)
message="Hello, how are you?"
echo "$message"

# Storing command output in variable
current_date=$(date)
echo "Current date: $current_date"

# Alternative syntax for command output
files=`ls -l | wc -l`
echo "Number of files: $files"

# Arithmetic operations
num1=10
num2=20
sum=$((num1 + num2))
echo "Sum: $sum"

# Combining variables
full_name="$name $city"
echo "Full info: $full_name"

# Clear a variable
unset name
echo "After unset: $name"  # Will be empty
```

---

## 4. What are Environment Variables vs Shell Variables?

### Definition
- **Shell Variables**: Local variables that exist only in the current shell session and are not passed to child processes.
- **Environment Variables**: Variables that are exported to the current shell and all child processes (programs, scripts) started from it.

### Explanation (For Beginners)
**Shell Variables (Local):**
- Like a **private note** that only you can see
- Created and used in the current shell only
- When you run a new script or command, it won't see these variables
- Example: `myvar="value"`

**Environment Variables (Global):**
- Like a **public notice** that everyone can see
- Exported so that child processes can access them
- When you run a script or command from this shell, it inherits these variables
- Example: `export PATH="/usr/bin:/bin"`

**Common Environment Variables:**
- `PATH` - List of directories where the shell looks for commands
- `HOME` - Current user's home directory
- `USER` - Current username
- `SHELL` - Current shell being used
- `PWD` - Present working directory

**Viewing Variables:**
- `set` - Shows all shell variables (local + environment)
- `env` - Shows only environment variables
- `echo $VARIABLE_NAME` - Shows a specific variable's value

**Setting Variables:**
- `VAR=value` - Creates a shell variable
- `export VAR=value` - Creates and exports as environment variable
- `export VAR` - Exports an existing shell variable

### Use
**Shell Variables:**
- Store temporary data within a script
- Hold loop counters and intermediate results
- Store configuration for the current script only

**Environment Variables:**
- Set system-wide configuration
- Pass data to child processes
- Define paths and program locations
- Store user preferences
- Configure application behavior

### Command
```bash
# Create a shell variable (local)
myvar="Hello"
echo "Shell variable: $myvar"

# Create and export environment variable
export myenv="World"
echo "Environment variable: $myenv"

# Test with a new shell
bash -c 'echo "In new shell - myvar: $myvar"'  # Empty (local not passed)
bash -c 'echo "In new shell - myenv: $myenv"'  # Shows "World" (exported)

# View all environment variables
env | less

# View all variables (shell + environment)
set | less

# View a specific environment variable
echo $PATH
echo $HOME
echo $USER

# Set PATH temporarily (current session only)
export PATH="$PATH:/my/new/path"

# Make it permanent (add to ~/.bashrc or ~/.bash_profile)
echo 'export PATH="$PATH:/my/new/path"' >> ~/.bashrc

# Create environment variable from shell variable
local_var="some value"
export local_var  # Now available to child processes

# Unset (delete) a variable
unset myvar
unset myenv
```

---

## 5. What is Shebang (#!/bin/bash)?

### Definition
The shebang (#!) is the first line in a shell script that tells the operating system which interpreter (program) should be used to execute the script.

### Explanation (For Beginners)
- **Shebang** = **#** (hash) + **!** (bang)
- It's always the **first line** of a script
- Must be at the very beginning of the file (no spaces or comments before it)
- Tells Linux: "Use this program to run my script"

**Common shebangs:**
- `#!/bin/bash` - Use Bash shell (most common, has many features)
- `#!/bin/sh` - Use POSIX shell (more basic, works everywhere)
- `#!/usr/bin/env bash` - Find bash in the system PATH (more portable)
- `#!/bin/python3` - Use Python interpreter
- `#!/usr/bin/perl` - Use Perl interpreter

**How it works:**
1. When you run `./script.sh`, the kernel reads the first line
2. It sees `#!/bin/bash` and knows to use `/bin/bash`
3. It passes the script to bash for execution
4. Bash then reads and executes all the commands in the script

**Why is it important?**
- Ensures the correct interpreter is used
- Makes scripts executable and self-contained
- Allows scripts to run with just `./script.sh` instead of `bash script.sh`
- Different shells have different features - shebang ensures compatibility

**Best practices:**
- Use `#!/bin/bash` for scripts using bash-specific features
- Use `#!/bin/sh` for maximum portability
- Use `#!/usr/bin/env bash` when bash might be in different locations on different systems

### Use
- Specify which shell interpreter to use
- Ensure scripts run correctly across different systems
- Make scripts portable and self-documenting
- Allow direct execution without specifying the shell

### Command
```bash
# Script with bash shebang
cat > bash_script.sh << 'EOF'
#!/bin/bash
echo "Running with Bash"
# This works in bash
array=(one two three)
echo "${array[@]}"
EOF

# Script with sh shebang
cat > sh_script.sh << 'EOF'
#!/bin/sh
echo "Running with sh"
# Basic syntax that works everywhere
echo "Hello World"
EOF

# Make them executable
chmod +x bash_script.sh sh_script.sh

# Run them
./bash_script.sh
./sh_script.sh

# Check which interpreter is being used
head -n 1 bash_script.sh
head -n 1 sh_script.sh

# Using env for portability
cat > portable_script.sh << 'EOF'
#!/usr/bin/env bash
echo "This is more portable - finds bash in PATH"
EOF

chmod +x portable_script.sh
./portable_script.sh

# Test if bash is in different location
which bash
```

---

## 6. How to Use Conditional Statements (if-else)

### Definition
Conditional statements (if-else) allow you to execute different commands based on whether a condition is true or false.

### Explanation (For Beginners)
- Conditional statements are like **decision-making** in real life
- They check if something is true or false, then act accordingly
- Think: "IF it's raining, THEN take umbrella, ELSE wear sunglasses"

**Basic syntax:**
```bash
if [ condition ]; then
    # commands to run if condition is true
else
    # commands to run if condition is false
fi
```

**Common comparison operators:**
- Numeric comparisons:
  - `-eq` - equal to
  - `-ne` - not equal to
  - `-gt` - greater than
  - `-lt` - less than
  - `-ge` - greater than or equal
  - `-le` - less than or equal

- String comparisons:
  - `=` - equal
  - `!=` - not equal
  - `-z` - string is empty
  - `-n` - string is not empty

- File comparisons:
  - `-f` - file exists
  - `-d` - directory exists
  - `-e` - exists (file or directory)
  - `-r` - readable
  - `-w` - writable
  `-x` - executable

**Multiple conditions:**
- `&&` - AND (both must be true)
- `||` - OR (at least one must be true)
- `!` - NOT (reverse the condition)

**elif (else if):**
```bash
if [ condition1 ]; then
    # commands
elif [ condition2 ]; then
    # commands
else
    # commands
fi
```

### Use
- Check if files exist before operating on them
- Validate user input
- Make decisions based on system state
- Handle different scenarios in your script
- Create interactive menus
- Implement error handling

### Command
```bash
#!/bin/bash

# Simple if-else
number=10
if [ $number -gt 5 ]; then
    echo "$number is greater than 5"
else
    echo "$number is not greater than 5"
fi

# Check if file exists
filename="test.txt"
if [ -f "$filename" ]; then
    echo "File $filename exists"
else
    echo "File $filename does not exist"
fi

# String comparison
name="Alice"
if [ "$name" = "Alice" ]; then
    echo "Welcome, Alice!"
else
    echo "Who are you?"
fi

# Using elif
score=85
if [ $score -ge 90 ]; then
    echo "Grade: A"
elif [ $score -ge 80 ]; then
    echo "Grade: B"
elif [ $score -ge 70 ]; then
    echo "Grade: C"
else
    echo "Grade: F"
fi

# Multiple conditions with AND
age=25
if [ $age -ge 18 ] && [ $age -lt 65 ]; then
    echo "You are an adult"
fi

# Multiple conditions with OR
file="document.pdf"
if [ "$file" = "*.pdf" ] || [ "$file" = "*.txt" ]; then
    echo "This is a document file"
fi

# Check if directory exists and is writable
dir="/tmp/mydir"
if [ -d "$dir" ] && [ -w "$dir" ]; then
    echo "Directory exists and is writable"
else
    echo "Cannot write to directory"
fi

# NOT operator
value=10
if [ ! $value -eq 20 ]; then
    echo "Value is not 20"
fi

# Nested if
age=20
if [ $age -ge 18 ]; then
    if [ $age -lt 21 ]; then
        echo "Adult but can't drink in US"
    else
        echo "Adult and can drink"
    fi
fi
```

---

## 7. How to Use Loops (for, while)

### Definition
Loops allow you to execute a block of commands multiple times, either for a fixed number of iterations (for loop) or while a condition is true (while loop).

### Explanation (For Beginners)
- Loops are like **repeating tasks** automatically
- Instead of writing the same command many times, use a loop
- Think: "Do this 10 times" or "Keep doing this until condition is met"

**For Loop:**
- Used when you **know how many times** to repeat
- Iterates through a list of items
- Stops after processing all items

**Basic for loop syntax:**
```bash
for item in list; do
    # commands
done
```

**Common for loop patterns:**
- Loop through numbers: `for i in 1 2 3 4 5`
- Loop through range: `for i in {1..10}`
- Loop through files: `for file in *.txt`
- Loop with counter: `for ((i=0; i<10; i++))`

**While Loop:**
- Used when you **don't know** how many times to repeat
- Continues as long as condition is true
- Stops when condition becomes false

**Basic while loop syntax:**
```bash
while [ condition ]; do
    # commands
done
```

**Common while loop patterns:**
- Loop until counter reaches value
- Loop while file exists
- Loop while command succeeds
- Infinite loop (with break)

**Until Loop:**
- Similar to while, but runs **until** condition becomes true
- Continues while condition is **false**

**Loop control:**
- `break` - Exit the loop immediately
- `continue` - Skip to next iteration

### Use
**For loops:**
- Process multiple files
- Iterate through a list of items
- Generate numbered files
- Repeat commands a specific number of times
- Create backups of multiple files

**While loops:**
- Monitor system status
- Wait for a condition to be met
- Process data until end
- Create countdown timers
- Retry failed operations

### Command
```bash
#!/bin/bash

# FOR LOOP - Loop through numbers
echo "For loop with numbers:"
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# For loop with range
echo -e "\nFor loop with range {1..5}:"
for i in {1..5}; do
    echo "Count: $i"
done

# For loop with range and step
echo -e "\nFor loop with step {1..10..2}:"
for i in {1..10..2}; do
    echo "Odd number: $i"
done

# For loop - C-style (like other programming languages)
echo -e "\nC-style for loop:"
for ((i=0; i<5; i++)); do
    echo "Index: $i"
done

# For loop - iterate through files
echo -e "\nLoop through .txt files:"
for file in *.txt; do
    if [ -f "$file" ]; then
        echo "Found file: $file"
    fi
done

# For loop - iterate through array
echo -e "\nLoop through array:"
fruits=("apple" "banana" "cherry")
for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done

# WHILE LOOP - Basic counter
echo -e "\nWhile loop with counter:"
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done

# While loop - read file line by line
echo -e "\nRead file line by line:"
echo -e "Line 1\nLine 2\nLine 3" > tempfile.txt
while read -r line; do
    echo "Read: $line"
done < tempfile.txt
rm tempfile.txt

# While loop - wait for file
echo -e "\nWait for file (example):"
# This would wait, but we'll just show the pattern
# while [ ! -f "/tmp/signal.txt" ]; do
#     sleep 1
# done

# While loop - infinite with break
echo -e "\nInfinite loop with break:"
counter=0
while true; do
    echo "Counter: $counter"
    counter=$((counter + 1))
    if [ $counter -eq 3 ]; then
        break
    fi
done

# Continue statement
echo -e "\nUsing continue (skip even numbers):"
for i in {1..5}; do
    if [ $((i % 2)) -eq 0 ]; then
        continue  # Skip this iteration
    fi
    echo "Odd number: $i"
done

# UNTIL loop
echo -e "\nUntil loop:"
count=5
until [ $count -eq 0 ]; do
    echo "Countdown: $count"
    count=$((count - 1))
done
```

---

## 8. How to Use Functions in Shell Script

### Definition
Functions are reusable blocks of code that perform a specific task. They allow you to organize code into logical sections and avoid repetition.

### Explanation (For Beginners)
- Functions are like **mini-programs** within your script
- They group related commands together
- You give the function a name and can call it multiple times
- Think of them as "recipes" you can use again and again

**Why use functions?**
- Avoid writing the same code multiple times (DRY - Don't Repeat Yourself)
- Make your script more organized and readable
- Easier to debug and maintain
- Can accept parameters (inputs) and return values (outputs)

**Basic function syntax:**
```bash
function_name() {
    # commands
}

# Or alternative syntax:
function function_name {
    # commands
}

# Call the function
function_name
```

**Functions with parameters:**
- Pass values to function: `function_name arg1 arg2`
- Access parameters inside function: `$1`, `$2`, `$3`, etc.
- `$#` - Number of parameters
- `$@` - All parameters as separate words
- `$*` - All parameters as single word

**Return values:**
- Functions can return exit status (0-255): `return 0`
- Default return is the exit status of last command
- Use `echo` to output data and capture with `$()`

**Local variables:**
- Use `local` keyword to create variables that only exist within the function
- Prevents variable name conflicts
- Good practice for clean code

**Scope:**
- Variables created inside functions are global by default
- Use `local` to make them local to the function
- Parameters are automatically local

### Use
- Organize complex scripts into smaller, manageable pieces
- Create reusable code blocks
- Handle specific tasks (logging, error handling, validation)
- Simplify main script logic
- Make code easier to test and debug
- Share code between multiple scripts (by sourcing)

### Command
```bash
#!/bin/bash

# Simple function
greet() {
    echo "Hello, World!"
}

# Call the function
greet

# Function with parameter
greet_user() {
    echo "Hello, $1!"
}

greet_user "Alice"
greet_