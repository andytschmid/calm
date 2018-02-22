# MSSQL 2014 Blueprint - Instructions

### IMPORTANT: This blueprint is for POC/Demo or to be used as a starting point only. Do not put this into production without modifying it to meet your organizations requirements, Nutanix Best Practices, and Microsoft SQL Best Practices.

Certain prerequisites must be met before installation will succeed. The following must be configured:
```
MSSQL installation requires CredSSP to be enabled on Karan host

Account running karan service must have the following privileges (SE_ASSIGNPRIMARYTOKEN_NAME, SE_INCREASE_QUOTA_NAME)
```

The following variables should be configured before use: 
```
On the SQL Service, set the variable "install_location" to external to download ISO from internet, or internal to download from a file share. If set to external, no further variables need to be modified.

If set to internal, you will need to fill out the following variables:
file_share_user
file_share_password
file_server_ip
file_share_name
sql_iso_path (include path and iso file)
mapped_drive
```

### To Enable CredSSP on the Karan host, please follow steps below:

1. On the Karan Host run the following command to enable CredSSP as a client role and allow Karan host to Delegate credentials to all computers ( Wild card mask "*")
 Enable-WSManCredSSP -Role Client -DelegateComputer *

2. From command prompt window run “gpedit.msc”

3. In the group policy editor window Goto Computer-configuration -> administrative templates -> system ->credential delegation

4. Double click on “Allow Delgating Fresh Credentials with NTLM-only server authentication”

5. Select Enable radio button

6. Click on the show button

7. In the value field add  “WSMAN/*”. This allows delegate fresh credentials to WSMAN running in any remote computer


### To assign the correct privileges, please follow the steps below:

1. Idenitfy the user account that the Karan service is running as 

2. From the Start menu, point to Administrative Tools, and then click Local Security Policy.

3. In the Local Security Settings dialog box, double-click Local Policies, and then double-click User Rights Assignment.

4. In the details pane, double-click Adjust memory quotas for a process. This is the SE_INCREASE_QUOTA_NAME user right.

5. Click Add User or Group, and, in the Enter the object names to select box, type the user or group name to which you want to assign the user right, and then click OK.

6. Click OK again, and then, in the details pane, double-click Replace a process level token. This is the SE_ASSIGNPRIMARYTOKEN_NAME user right.

7. Click Add User or Group, and, in the Enter the object names to select box, type the user or group name to which you want to assign the user right, and then click OK.

8. Restart the Karan service.
