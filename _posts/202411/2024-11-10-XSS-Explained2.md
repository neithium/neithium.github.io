---
title: Understanding XSS 02
description: >-
  A detailed blog on DOM Based XSS
author: anorak
date: 2024-11-10 00:10:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity, XSS, Web Security, JavaScript, Vulnerabilities]
pin: false
---

## What is DOM Based XSS?
DOM Based [XSS](https://github.com/neithium/neithium.github.io/blob/main/_posts/202411/2024-11-09-xss1.md) (or as it is called in some texts, “type-0 XSS”) is an XSS attack wherein the attack payload is executed as a result of modifying the DOM “environment” in the victim’s browser used by the original client side script, so that the client side code runs in an “unexpected” manner. That is, the page itself (the HTTP response that is) does not change, but the client side code contained in the page executes differently due to the malicious modifications that have occurred in the DOM environment.


This is in contrast to other XSS attacks (stored or reflected), wherein the attack payload is placed in the response page (due to a server side flaw).


## Example:


Suppose the following code is used to create a form to let the user choose their preferred language. A default language is also provided in the query string, as the parameter “default”.

```
Select your language:

<select><script>

document.write("<OPTION value=1>"+decodeURIComponent(document.location.href.substring(document.location.href.indexOf("default=")+8))+"</OPTION>");

document.write("<OPTION value=2>English</OPTION>");

</script></select>
```

The page is invoked with a URL such as:

```https://www.some.site/page.html?default=French```

A DOM Based XSS attack against this page can be accomplished by sending the following URL to a victim:

```https://www.some.site/page.html?default=<script>alert(document.cookie)</script>```

When the victim clicks on this link, the browser sends a request for:

```/page.html?default=<script>alert(document.cookie)</script>```

to www.some.site. The server responds with the page containing the above Javascript code. The browser creates a DOM object for the page, in which the document.location object contains the string:

```https://www.some.site/page.html?default=<script>alert(document.cookie)</script>```

The original Javascript code in the page does not expect the default parameter to contain HTML markup, and as such it simply decodes and echoes it into the page (DOM) at runtime. The browser then renders the resulting page and executes the attacker’s script:
```
alert(document.cookie)
```
Note that the HTTP response sent from the server does not contain the attacker’s payload. This payload manifests itself at the client-side script at runtime, when a flawed script accesses the DOM variable document.location and assumes it is not malicious. In addition, most browsers URL encode document.location by default which reduces the impact or possibility of many DOM XSS attacks.



## Testing Tools and Techniques

1. [The DOM XSS Wiki]( https://code.google.com/p/domxsswiki/) - The start of a Knowledgebase for defining sources of attacker controlled inputs and sinks which could potentially introduce DOM Based XSS issues. Its very immature as of 11/17/2011. Please contribute to this wiki if you know of more dangerous sinks and/or safe alternatives!!


2. [DOM Snitch](https://code.google.com/p/domsnitch/) - An experimental Chrome extension that enables developers and testers to identify insecure practices commonly found in client-side code. From Google.


3. [Defense Techniques](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html)

## References

1. [ “DOM Based Cross Site Scripting or XSS of the Third Kind” (WASC writeup), Amit Klein, July 2005](https://www.webappsec.org/projects/articles/071105.shtml)

2. [ “JavaScript Code Flow Manipulation, and a real world example advisory - Adobe Flex 3 Dom-Based XSS” (Watchfire blog), Ory Segal, June 17th, 2008](https://blo2g.watchfire.com/wfblog/2008/06/javascript-code.html)

3. [ “Attacking Rich Internet Applications” (RUXCON 2008 presentation), Kuza55 and Stefano Di Paola, November 2008](https://www.ruxcon.org.au/files/2008/Attacking_Rich_Internet_Applications.pdf)

4. [ “Subverting Ajax” (23C3 presentation), Stefano Di Paola and Giorgio Fedon, December 2006](https://events.ccc.de/congress/2006/Fahrplan/attachments/1158-Subverting_Ajax.pdf)

5. [ “Protecting Web Applications from Universal PDF XSS” (2007 OWASP Europe AppSec presentation) Ivan Ristic, May 2007](https://wiki.owasp.org/images/c/c2/OWASPAppSec2007Milan_ProtectingWebAppsfromUniversalPDFXSS.ppt)

