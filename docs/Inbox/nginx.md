自定义ssl

```shell
openssl genrsa -des3 -out ca.key 2048 
openssl req -new -x509 -days 7305 -key ca.key -out ca.crt 
openssl genrsa -des3 -out oycol.pem 2048  # centos版本如果是CentOS Linux release 8.0.1905 (Core)版本，私钥长度不能设置成1024位，必须2048位
openssl rsa -in oycol.pem -out oycol.key
openssl req -new -key oycol.pem -out oycol.csr
openssl ca -policy policy_anything -days 1460 -cert ca.crt -keyfile ca.key -in oycol.csr -out oycol.crt
```

