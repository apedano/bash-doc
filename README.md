# bash-doc

# Data types

## boolean

```bash
#!/bin/bash

# Declare a boolean variable using 'true' and 'false'
application_enabled=true

# Check the boolean variable
if $application_enabled; then
	echo "Application enabled."
else
	echo "Application disabled."
fi
```


## Array

### Initialization
```bash
#init files array
filesArray=()
```
Initializes the array with an output from a commans
```bash
#Finds the skip true in pom files
filesWithSkipDeployTrue=$(find ./ -type f -name "pom.xml" | xargs grep -loP "<maven.deploy.skip>([^f]*)</maven.deploy.skip>")

#init files array
filesArray=()

#fills the array
for i in $filesWithSkipDeployTrue; do filesArray+=($i) ; done

for fileMatched in "${filesArray[@]}"
do
  echo "Found file: $fileMatched"
done
```

### Index access

```bash
!/bin/bash
# Create an array
declare -A fruits

for index in "${fruits[@]}"; do
  echo "Index: $index, Value: ${fruits[$index]}"
done

# Add elements (fruit names) by array key named red and yellow 
fruits[red]='Cherries,  Watermelon'
fruits[yellow]='Apricots, Pineapples'
 
echo "Printing all elements of array named 'fruits':'"
echo "${fruits[@]}"
echo
 
echo "Printing all keys of array named 'fruits':'"
echo "${!fruits[@]}"
echo
 
echo "Printing all array element using the key named 'red':"
echo "${fruits[red]}"
echo
 
echo "Printing all array element using the key named 'yellow':"
echo "${fruits[yellow]}"
echo
 
echo "Printing bash array using a key and values:"
echo -e "-----------------------------------------"
echo -e "KEY\t\t -> VALUES:"
echo -e "-----------------------------------------"
for key in "${!fruits[@]}"
do
    echo -e "${key}\t\t -> ${fruits[$key]}"
done
```

### Array Length

```bash
$ a=(1 2 3 4)
$ echo ${#a[@]}
4
```

## Multivalued array, associative array
### Declaration
```bash
eoDeploymentRepoName="olo-kor-eo-deployment"
elDeploymentRepoName="olo-kor-el-deployment"

#Associative array definition
declare -A harborRepositoryToDeploymentRepoMap=(
  [olo-kor-eo-service]=$eoDeploymentRepoName
  [olo-kor-eo-mapper-service]=$eoDeploymentRepoName
  [olo-kor-el-service]=$elDeploymentRepoName
  [olo-kor-el-mapper-service]=$elDeploymentRepoName
  [olo-kor-mocks]=$elDeploymentRepoName
  [olo-kor-taxud-validation-service]=$elDeploymentRepoName
  )
```
### Operations
```bash
#Retrieving keys
for key in "${!harborRepositoryToDeploymentRepoMap[@]}"; do echo "$key"; done

#Retrieving values
for value in "${harborRepositoryToDeploymentRepoMap[@]}"; do echo "$value"; done

#Specific value
echo "Specific value: ${harborRepositoryToDeploymentRepoMap[olo-kor-eo-service]}"
echo "Specific value: ${harborRepositoryToDeploymentRepoMap['olo-kor-eo-service']}"

# Retrieving key-value pairs
for i in "${!harborRepositoryToDeploymentRepoMap[@]}"; do echo "${i}: ${harborRepositoryToDeploymentRepoMap[$i]}"; done #ex: olo-kor-eo-service: olo-kor-eo-deployment

# Number of elements
echo "${#harborRepositoryToDeploymentRepoMap[@]}" #6

#Adding/Overwriting element
harborRepositoryToDeploymentRepoMap[new-key]="new-value" #overwrites if the key exists
echo "${harborRepositoryToDeploymentRepoMap[new-key]}"

#Deleting pair
unset harborRepositoryToDeploymentRepoMap[new-key]
```

## `if` statement

```bash
if [ -r $1 ] && [ -s $1 ]
then
echo This file is useful.
fi
```
Alternative format

```bash
if [[ -z "$filesWithSkipDeployFalse" ]]; then 
  echo "No files have been found with skip deploy false. Exiting..."
  exit 132
else
  echo "Files with skip deploy false exist..."
fi
```

Ternary operation

````bash
# Declare a boolean variable using a conditional ternary operator
application_enabled=true

# Ternary operator simulation
message=$([ "$application_enabled" == true ] && echo "Application enabled." || echo "Application disabled.")

# Print the message
echo $message
```


### Checks

| Operator                                                 | Description                                                                 |
|----------------------------------------------------------|-----------------------------------------------------------------------------|
| ! EXPRESSION                                             | The EXPRESSION is false.                                                    |
| -n STRING                                                | The length of STRING is greater than zero.                                  |
| -z STRING                                                | The lengh of STRING is zero (ie it is empty).                               |
| Double-bracket syntax only:[[ STRING1 =~ REGEXPATTERN ]] | STRING1 matches REGEXPATTERN                                                |
| Double-bracket syntax only:[[ STRING1 == "PATTERN" ]]    | STRING1 is contained in the PATTERN
| STRING1 = STRING2                                        | STRING1 is equal to STRING2                                                 |
| STRING1 != STRING2                                       | STRING1 is not equal to STRING2                                             |
| INTEGER1 -eq INTEGER2                                    | INTEGER1 is numerically equal to INTEGER2                                   |
| INTEGER1 -gt INTEGER2                                    | INTEGER1 is numerically greater than INTEGER2                               |
| INTEGER1 -lt INTEGER2                                    | INTEGER1 is numerically less than INTEGER2                                  |
| -d FILE                                                  | FILE exists and is a directory.                                             |
| -f FILE                                                  | A regular file is neither a block or character special file nor a directory |
| -e FILE                                                  | FILE exists.                                                                |
| -r FILE                                                  | FILE exists and the read permission is granted.                             |
| -s FILE                                                  | FILE exists and it's size is greater than zero (ie. it is not empty).       |
| -w FILE                                                  | FILE exists and the write permission is granted.                            |
| -x FILE                                                  | FILE exists and the execute permission is granted.                          |

```bash
  if [[ $currentTag =~ ^.*([0-9]+\.){2}[0-9]+.*$ ]]; then
    echo "The tag matches semantic version pattern XX.YY.ZZ"
  fi
```


## String

### Replace

```bash
#tr to remove whitespaces
#tr to convert upper case to lower case characters
#tr to delete (-d) the complement (-c) of digits and letters 
echo $someString | tr -d '[:space:]' | tr '[:upper:]' '[:lower:]' | tr -dc '[:alnum:]'
```

## Arguments

### Argument check

`$#` represents the number of arguments

```bash
if [ $# -eq 0 ]
  then
    echo "No arguments supplied"
fi
```

### Argument read

```bash
if [ -z "$1" ]
  then
    echo "No argument supplied"
fi
```


## Flow control

### IF
```bash
if [ $? != 0 ]; then
  echo "Error in call to Harbor API"
  exit 1
else
  echo "API call ok"
fi
```

### FOR
```bash
for deploymentRepo in "${!uniqueDeploymentRepos[@]}"; do
  echo "Cloning repo [$deploymentRepo]"
  git clone ssh://git@git.belastingdienst.nl:7999/essentials/${deploymentRepo}.git
done
```

## Processes

### Process exit code check

```bash
oc project $PROJECT_NAME
if [ $? -eq 0 ] # the last command exit code is not 0 (not ok)
then
  echo "Project $PROJECT_NAME exists we can proceed with config apply"
else
  echo "Project $PROJECT_NAME dosn't exist. Aborting..."
  exit 1
fi
```

### Exit if any command in the script fail

```bash
#!/bin/bash

set -e 
...
```


### Run external script

Remember to add the `#!/bin/bash` line on top of the external script

```bash
#!/bin/bash
...
sh <PATH_TO_SH_FILE_INCLUDING_EXTENSION>
```

## File manipulation

### Find files based on content
```bash
#Finds the skip true in pom files
filesWithSkipDeployT=$(find ./ -type f -name "pom.xml" | xargs grep -loP "<maven.deploy.skip>([^f]*)</maven.deploy.skip>")
```

### Change directory based on file path

```bash
for fileMatched in "${filesArray[@]}" #example ./obp-AfhandelenAanmelding/pom.xml
do
  echo "Found file with skip deploy false: $fileMatched"
  cd $(dirname "$fileMatched") 
  echo "Current dir $(pwd)" #Current dir /c/ws/git/olo/olo-kor-contracts/obp-AfhandelenAanmelding
done
```

### Raplace content or replace xml tag value in file 
the `-i` option updates the current file
```bash
sed -i '/maven.deploy.skip/{s/false/true/}' ./pom.xml
```
* The first part do a search in the file for `maven.deploy.skip` which is the tag name we are looking for
* `/s/false/` the text to replace from the position found
* `true/` the replacement value





