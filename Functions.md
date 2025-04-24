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


```

