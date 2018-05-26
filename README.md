# Repository Docker Registry

### Create certificate

Step 1 - Open file openssl.cnf
```bash
vim /usr/lib/ssl/openssl.cnf
```
Step 2 - modify the req parameters. Add an alternate_names section to openssl.cnf
```bash
[ alternate_names ]
DNS.1        = d2d.com.br
DNS.2        = docker.d2d.com.br
```

Step 3 - Next, add the following to the existing [ v3_ca ] section. Search for the exact string [ v3_ca ]:

```bash
subjectAltName      = @alternate_names
```
Step 4 - You might change keyUsage to the following under [ v3_ca ]:
```bash
keyUsage = digitalSignature, keyEncipherment
```
Step 5 - modify the signing parameters. Find this line under the CA_default section:

```bash
# Extension copying option: use with caution.
# copy_extensions = copy
```
And change it to:
```bash
# Extension copying option: use with caution.
copy_extensions = copy
```
Step 6 - Generate your self-signed:
```bash
cd nginx
openssl genrsa -out docker.d2d.com.br.key 3072
openssl req -new -x509 -key docker.d2d.com.br.key -sha256 -out docker.d2d.com.br.pem -days 730
```
Step 7 - Finally, examine the certificate:

```bash
openssl x509 -in certificate.pem -text -noout
```

Step 8 - Move certificate .crt

```bash
cp docker.d2d.com.br.crt  /usr/local/share/ca-certificates
update-ca-certificates
```

### UP Container

```bash
docker-compose up -d
```