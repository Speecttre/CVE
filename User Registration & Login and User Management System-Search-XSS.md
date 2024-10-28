## Admin Page Search XSS from User Registration & Login and User Management System

**Affected Project**: User Registration & Login and User Management System 3.2

**Official Website**: https://phpgurukul.com/user-registration-login-and-user-management-system-with-admin-panel

**Version**: 3.2

**Related Code File**: search-result.php

**Injection Parametre**: searchkey

## Vulnerability Description

`search-result.php` is vulnerable to the tested XSS payload: "<svg/onerror=alert(document.cookie)>". 
Application does not properly sanitize or validate the username input, this script could be executed in user's browser, leading to an XSS attack.

## Demonstration

Below is how `search-result.php` looks like:

![](https://github.com/Speecttre/IMAGE/blob/main/ADMINSEARCHXSS.png)

View of Source Code

![](https://github.com/Speecttre/IMAGE/blob/main/ADMINSEARCH_SOURCECODE.png)














