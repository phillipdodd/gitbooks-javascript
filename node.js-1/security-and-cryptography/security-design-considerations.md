# Security Design Considerations

## Authentication

## Authorization

## Transport Protection

How you protect the **requests** and **responses** between the **browser and back**-**end** resources. This also applies to any **back-end to back-end** interactions between services.

Basically, just use **HTTPS** _everywhere._

![](../../.gitbook/assets/image%20%282%29.png)



## Cross Origin Resource Sharing \(CORS\)

![](../../.gitbook/assets/image%20%283%29.png)

{% hint style="info" %}
Angular's **HTTP Service** takes care of using the correct headers required for the CORS Protocol.
{% endhint %}



## Cross Site Request Forgery \(CSRF\)

![](../../.gitbook/assets/image%20%284%29.png)

If authorization is stored within a cookie, malicious code can potentially access them and use them to  perform priviledged actions.

## Cross Site Scripting \(XSS\)

Occurs when direct input is injected into the DOM and does things like execute script handlers.

![](../../.gitbook/assets/image%20%285%29.png)

Input should always be **sanitized.**

{% hint style="info" %}
Angular treats values that are put into the DOM through databinding as **untrusted** and will escape or sanitize them.
{% endhint %}

## 

