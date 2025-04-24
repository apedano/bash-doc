# Functions

## Declaration and invocation

```bash
# syntax.sh
# Declaring functions using the reserved word function
# Multiline
function f1 {
        echo Hello I\'m function 1
        echo Bye!
}
# One line
function f2 { echo Hello I\'m function 2; echo Bye!; }

# Declaring functions without the function reserved word
# Multiline
f3 () { 
        echo Hello I\'m function 3
        echo Bye!
}
# One line
f4 () { echo Hello I\'m function 4; echo Bye!; }

# Invoking functions
f4
f3
f2
f1
```

## Arguments

|Argument|Role|
|--- |--- |
|$0|Reserves the function's name when defined in the terminal. When defined in a bash script, $0 returns the script's name and location.|
|$1, $2, etc.|Corresponds to the argument's position after the function name.|
|$#|Holds the count of positional arguments passed to the function.|
|$@ and $*|Hold the positional arguments list and function the same when used this way.|
|"$@"|Expands the list to separate strings. For example "$1", "$2", etc.|
|"$*"|Expands the list into a single string, separating parameters with a space. For example "$1 $2" etc.|

Example
```bash
arguments () {
        echo The function location is $0
        echo There are $# arguments
        echo "Argument 1 is $1"
        echo "Argument 2 is $2"
        echo "<$@>" and "<$*>" are the same.
        echo List the elements in a for loop to see the difference!
        echo "* gives:"
        for arg in "$*"; do echo "<$arg>"; done
        echo "@ gives:"
        for arg in "$@"; do echo "<$arg>"; done
}
```
Invocation
```bash
arguments hello world

There are 2 arguments
Argument 1 is hello
Argument 2 is world
<hello world> and <hello world> are the same.
List the elements in a for loop to see the difference!
* gives:
<hello world>
@ gives:
<hello>
<world>
```

## Return value

### Using `return` statement
The return is the exist status code 

```bash
test_function() {
        echo Test
        return 100
}
echo The function\'s output is: 
test_function
echo The exit status is:
echo $?
```

### Using `echo` statement
An alternative method is to echo the function's result and assign the output to a variable.
```bash
test_function() {
        echo Test
}
result=$(test_function)
echo $result is saved in a variable for later use #Test is saved in a variable for later use
```

### Map function return into an array
What is echoed by the function is mapped to the array using the `mapfile` command

```bash
# ========== function to extract fields from input JSON content  ==========
# shellcheck disable=SC1009
function extractFieldsAsArrayFromJsonResponse() {
  local rawJsonContent="$1"
  local jqSelector="$2"
  jsonFields=$(echo "$rawJsonContent" | jq -cr $jqSelector)
  while read -r jsonField; do
    #The jsonValue contains some strange char which invalidates following tag matching.
    # sed replaces chars except the specified in the argument
    cleanedJsonField=$(echo $jsonField | sed 's/[^a-zA-z0-9]\-\.//g')
    #this echo will be captured in the mapfile from the output of the function
    echo "$cleanedJsonField"
  done <<< "$jsonFields"
}

#function call
repositoryDetailsResponse=$(curl -s ${repositoryDetailsUrl})
mapfile -t digestArray < <(extractFieldsAsArrayFromJsonResponse "$repositoryDetailsResponse" ".[].digest")

for digest in "${digestArray[@]}"; do
  echo "- $digest"
done
```
- `<(extractFieldsAsArrayFromJsonResponse "$repositoryDetailsResponse" ".[].digest")` is the process substitution treating the ouptut (the echo in the function) as a file
- `mapfile -t <array_name> < ...`
        -`mapfile` reads each line from the process substitution file into the <array_name>
        -`-t` removes the trailing newline




