# Testcase - Log Content Requirements
### Verify that the application does not log credentials or payment details

### Preconditions
* Payment Application with application log

### Input
* Username
* Password
* Credit Card Number

### Actions
1.	Click “Login”
2.	Login with Username and Password
3.	Login as “testuser01” and password as “P@ssw0rd123”
4.	Click “Payment menu”
5.	Payment with Credit Card Number as “9999-9999-9999-9999”
6.	Click “Logout”
7.	Verify Log File


### Expected Results
* Application dose not log Credential in Log File
* Application dose not log Credit Card Number in Log File


### Team Author 
Ekawut Chairat

Hachol Dabthong