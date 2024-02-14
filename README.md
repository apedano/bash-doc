# bash-doc

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
