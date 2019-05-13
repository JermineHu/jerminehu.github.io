---
title: "Certificate Signed by Unknown Authority Error"
date: 2017-05-13T16:21:51+08:00
categories: ["All",docker,linux]
tags: [docker,linux]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: [docker,linux,x509 ]
description: "Certificate Signed by Unknown Authority Error"
---


## Certificate signed by unknown authority error 

x509: certificate signed by unknown authority
This error message means that you do not have a trusted certificate. You need to trust the default certificates generated during your Docker Trusted Registry (DTR) installation.

You can do so by running these commands on the nodes from where you want to access your DTR (be sure to replace <my-dtr-domain> with your DTR Domain name.):

**CentOS/RHEL**


```
export DOMAIN_NAME=hub.fi.vdo.pub
export TCP_PORT=443
openssl s_client -connect $DOMAIN_NAME:$TCP_PORT -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM | sudo tee /etc/pki/ca-trust/source/anchors/$DOMAIN_NAME.crt
update-ca-trust
systemctl restart docker.service
```

**Ubuntu**

```
export DOMAIN_NAME=hub.fi.vdo.pub

export TCP_PORT=443

openssl s_client -connect $DOMAIN_NAME:$TCP_PORT -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM | sudo tee /usr/local/share/ca-certificates/$DOMAIN_NAME.crt

update-ca-certificates

service docker restart
```

**Arch**

```
export DOMAIN_NAME=hub.fi.vdo.pub
export TCP_PORT=443
openssl s_client -connect $DOMAIN_NAME:$TCP_PORT -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM | sudo tee /etc/ssl/certs/$DOMAIN_NAME.crt
update-ca-trust
systemctl restart docker.service
```

其他错误：

比如没有域名，只有IP，注意在server端的openssl.conf里面设置subjectAltName：

https://www.tianmaying.com/tutorial/docker-registry