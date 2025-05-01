# Networking

## OPENSSL
### Certificate chain verification agains a host

```bash
echo "" | openssl s_client -connect ccn2-wsgw.dmz.belastingdienst.nl:443 -showcerts 2>/dev/null
```

### Call with TSL1.2 support
```bash
openssl s_client -connect ccn2-wsgw.acc.dmz.belastingdienst.nl:443 -tls1_2
```

### Verify SSL/TLS handhshake using a CA bundle file
The CA bundle file should contain one or more verified and trusted CA certificates

```bash
openssl s_client -connect <host>:<port> -CAfile <path-to-bundle>
```
Example
```bash
openssl s_client -connect iaas.obj.belastingdienst.nl:5443 -CAfile ca-bundle.crt
```
The output 
Success -> `Verify return code: 0 (ok)`
Error (example) -> `Verify return code: 21 (unable to verify the first certificate)`

#### The crt file
It is very important that all certs in the file are sorted based on the verification order

For example we have from 

```
Certificate chain
Certificate 1:
Subject: CN=prod-bdc-p.obj.belastingdienst.nl,O=ODC Belastingdienst,C=NL
Issuer: CN=Infrastructuur CA - G3,O=ODC Belastingdienst,C=NL
Certificate 2:
Subject: CN=Infrastructuur CA - G3,O=ODC Belastingdienst,C=NL
Issuer: CN=ODC Belastingdienst Root CA - G1,O=ODC Belastingdienst,C=NL
Certificate 3 
Subject: CN=ODC Belastingdienst Root CA - G1,O=ODC Belastingdienst,C=NL
Issuer:  CN=ODC Belastingdienst Root CA - G1,O=ODC Belastingdienst,C=NL
 ```
The order must be `prod-bdc-p.obj.belastingdienst.nl -> G3 -> G1`
The file must be a concatenation of the certificate in **PEM** format
 ```
-----BEGIN CERTIFICATE-----
<base64 content>
-----END CERTIFICATE-----
 ```
It is allowed to have text in between certificates

If we want to verify the file we can use

```
 openssl x509 -in ca-bundle.crt -text -noout
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

