# Admin Login Page SQL Injection From User Registration & Login and User Management System


**Affected Project**: User Registration & Login and User Management System 3.2

**Official Website**: https://phpgurukul.com/user-registration-login-and-user-management-system-with-admin-panel

**Version**: 3.2

**Related Code File**: admin

**Injection Parametre**: Username

## Vulnerability Description

`admin` is vulnerable to the tested SQLi payload: 'or 1=1-- - 
Application does not properly sanitize or validate the username input, this script could be executed in user's browser, leading to an SQLi attack.

## Demonstration

Below is how `admin` looks like:

![](https://github.com/Speecttre/IMAGE/blob/main/USER_REGISTER_ADMINLOGINSQLI.png)

View of using SQLMAP

![](https://github.com/Speecttre/IMAGE/blob/main/ADMINLOGIN_SQLMAP_SCAN.png)














