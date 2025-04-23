# Networking

## Certificate chain verification agains a host

```bash
echo "" | openssl s_client -connect olo.kor.eo.ont.belastingdienst.nl:443 -showcerts 2>/dev/null
```

## Create Basic authentication string

```bash
echo "<username>:<password>" | base64
```
So that the basic auth header becomes

```bash
Authorization: Basic <base64_string>
```

