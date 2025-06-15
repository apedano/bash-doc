# Scipt command execution

## Looping through results of a command

```bash
cluster_names=$(kind get clusters)
for cluster_name in $cluster_names; do echo "Cluster name: $cluster_name" ; done
```
