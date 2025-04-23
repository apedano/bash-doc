# Networking

## OPENSSL
### Certificate chain verification agains a host

```bash
echo "" | openssl s_client -connect ccn2-wsgw.dmz.belastingdienst.nl:443 -showcerts 2>/dev/null
```

### Call with supported signature algorithms
```bash
echo "" | openssl s_client -connect ccn2-wsgw.dmz.belastingdienst.nl:443 -showcerts -tls1_2 2>/dev/null
```



## Curl

### With Basic Auth header

#### Create Basic authentication string

```bash
echo -n "<username>:<password>" | base64
```
So that the basic auth header becomes

```bash
Authorization: Basic <base64_string>
```
Use it in a call
```bash
curl -X 'DELETE' '<call_url_including_protocol>' -H 'accept: application/json' -H 'Authorization: Basic <base64_string>' 
```

