
# 🐚 Zsh If-Statement Syntax Summary

Zsh uses the standard Bourne-style if/then/fi structure but offers powerful features like double brackets [[ ]] and arithmetic double parentheses (( )).

---
## 1. Basic Structure

The most important rule is that every if must end with a fi (if spelled backwards).

    if [[ condition ]]; then
        # code to run
    elif [[ second_condition ]]; then
        # code to run
    else
        # code to run
    fi

---

## 2. Comparison Types

### A. String & General Comparisons (`[[ ... ]]`)
Use double brackets for most checks. They are safer and more flexible than single brackets.

| Comparison      | Syntax                 | Description                    |
| :-------------- | :--------------------- | :----------------------------- |
| **Equality**    | `[[ $A == $B ]]`       | True if strings are identical. |
| **Inequality**  | `[[ $A != $B ]]`       | True if strings are different. |
| **Regex Match** | `[[ $A =~ ^[0-9]+$ ]]` | True if A matches the pattern. |
| **Empty Check** | `[[ -z $A ]]`          | True if string is empty.       |

### B. Mathematical Comparisons ((( ... )))
For numbers, using double parentheses allows you to use standard math symbols.

| Symbol | Meaning          | Example               |
| :----- | :--------------- | :-------------------- |
| >      | Greater than     | `if (( count > 10 ))` |
| <      | Less than        | `if (( count < 5 ))`  |
| ==     | Equal to         | `if (( count == 0 ))` |
| >=     | Greater or equal | `if (( count >= 1 ))` |

### C. File System Checks
Used inside `[[ ... ]]` to check the status of files or folders.

| Flag | Meaning           | Example                    |
| :--- | :---------------- | :------------------------- |
| -f   | File exists       | `[[ -f "config.txt" ]]`    |
| -d   | Directory exists  | `[[ -d "/tmp" ]]`          |
| -x   | Executable        | `[[ -x "script.sh" ]]`     |
| -e   | Exists (any type) | `[[ -e "path/to/thing" ]]` |

---

## 3. Best Practices for Zsh

1. Always use `[[ ]]` over `[ ]`: Double brackets handle empty variables and spaces much more gracefully.
2. The Semicolon: If "then" is on the same line as "if", you must use a semicolon: `if [[ cond ]]; then`.
3. Indentation: Always indent the code inside the block for readability.
4. Quotes: While `[[ ]]` often makes quoting optional, it is still a "best practice" to use `"$VAR"`.
---
## 4. Example: Timestamp File Checker

    # Check if a specific file exists before doing something
    FILE="name_$(date +%s).txt"

    if [[ -f "$FILE" ]]; then
        echo "Error: $FILE already exists."
    else
        touch "$FILE"
        echo "Created: $FILE"
    fi


# 🛠️ Zsh Functions: Syntax and Usage

Functions allow you to group commands, use logic, and accept arguments. They are more flexible than aliases.

---
## 1. Defining a Function

The standard way to write a function is:

    function_name() {
        # Your commands here
    }

---
## 2. Handling Arguments

Shell functions use numeric variables to represent inputs:

| Variable      | Description                                      |
| :------------ | :----------------------------------------------- |
| **$1, $2...** | The 1st, 2nd, etc., arguments passed.            |
| **$@**        | Represents all arguments passed to the function. |
| **$#**        | The count of how many arguments were provided.   |

---
## 3. Local Variables

Always define variables inside functions using the `local` keyword. This keeps your shell environment clean.

    my_func() {
        local my_var="I only exist inside this function"
        echo $my_var
    }

---
## 4. Real-World Example: Timestamp Creator

This function creates a file with a timestamp. If you provide a name, it uses it; otherwise, it defaults to "log".

    mktempfile() {
        local name="${1:-log}"
        local ts=$(date +%s)
        local fname="${name}_${ts}.txt"
        
        touch "$fname"
        echo "Created: $fname"
    }

    # Usage:
    # mktempfile data    -> creates data_1733659565.txt
    # mktempfile         -> creates log_1733659565.txt


## 5. ↩️ Return in Zsh Functions

### Exit Status (The Integer) 
The `return` keyword only sends back a status code (0-255).
- `return 0`: Success *
- `return 1+`: Error 
-  Access via: `$?`


###  Returning Data (The String) 
To send text or data back to the caller, use `echo` inside the function and capture it with `$()`. 

    get_name() {
        echo "Fortnox-Project"
    }
    result=$(get_name)

### Comparison Table 
| Feature | `return` | `echo` + `$( )` | 
| :--- | :--- | :--- | 
| **Purpose** | Success/Failure signal | Passing actual data | 
| **Data Type** | Integer (0-255) | String / Text | 
| **Usage** | `if my_func; then` | `var=$(my_func)` |