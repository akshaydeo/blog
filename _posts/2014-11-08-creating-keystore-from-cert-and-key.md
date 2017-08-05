---
title: Creating .keystore from .cert and .key
author: Deo Akshay
layout: post
comments: true
permalink: /creating-keystore-from-cert-and-key/
post_to_facebook_timeline:
  - 0
dsq_thread_id:
  - 3204807154
categories:
  - Java
  - keystore
---
A quick not for creating .keystore using a set of certificates including root, intermediate and domain certs.

First create a .p12 using private key and domain certificate.  
{% highlight bash %}openssl pkcs12 -export -in <domain_cert> -inkey
<private_key> -certfile <domain_cert> -name "<name>" -out
<p12_name>.p12{% endhighlight %}

Create keystore file using  
{% highlight bash %}keytool -importkeystore -destkeystore <keystore_name>.keystore -srckeystore
<p12_name>.p12 -srcstoretype pkcs12 -alias <alias_name>{% endhighlight %}

Then import the other certificates using  
{% highlight bash %}keytool -import -trustcacerts -alias root -file <cert_file> -keystore <keystore_name>.keystore{% endhighlight %}
