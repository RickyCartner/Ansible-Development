
## Create certs
```console
openssl req -newkey rsa:4096 -nodes -keyout ~/ansible/certs/qa-ansible.key -x509 -days 3650 -out ~/ansible/certs/qa-ansible.crt -subj "/C=US/ST=SC/L=Columbia/O=BCBSSC/OU=ICT/CN=10.160.47.71/emailAddress=ricky.cartner@bcbssc.com"
```

## Copy certs to `etc/pki/tls/certs/`
```console
$ sudo cp ~/ansible/certs/qa-ansible.* /etc/pki/tls/certs/
```

## Update permissions
```console
$ sudo chmod 0644 /etc/pki/tls/certs/qa-ansible.*
```

```console
$ sudo chown root:wheel /etc/pki/tls/certs/qa-ansible.*
```

## Restore connections and restart services
```console
$ sudo restorecon -v /etc/pki/tls/certs/qa-ansible.crt && sudo restorecon -v /etc/pki/tls/certs/qa-ansible.key
```

```console
$ sudo systemctl daemon-reload
```

```console
$ sudo systemctl restart code-server@a-dv18
```


req – is used to create CSR (certificate signing request).

-newkey rsa:4096 – is used to create a new cert request with strength of 4096 RSA key.

-nodes -if private key is created it will not be encrypted.

-keyout /etc/pki/tls/private/bitwardentest.key – location where private key will be saved.

-x509 – output will be self signed cert, not a cert request.

-days 3650 – validity of my cert will be 10 years,365 would be a better option to enter. With 365 cert will be valid one year. Much better security practice.

-out /etc/pki/tls/certs/bitwaredentest.crt – specifies path for public part of self signed cert.

C= country

ST= state

L = city

O= organization name

OU = department

CN = domain name (you will enter name for your domain for which you need certificate)
    - qa.ansible.mgmt.bcbssc.com
    - 10.160.47.71

email address = valid email address