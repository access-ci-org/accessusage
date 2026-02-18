**********************************************
** accessusage-0.4-1 Install Instructions
**********************************************

Instructions for installing accessusage from RPM (recommended) or from TAR.

Choose either RPM or TAR install.

**********************
** RPM Installation **
**********************

R1) Install the desired RPM release from [https://github.com/access-ci-org/accessusage/releases](https://github.com/access-ci-org/accessusage/releases).

   NOTE: This package automatically creates an "accessusage" account that owns the files and will
         execute accessusage via sudo.

The RPM installs files by default under “/usr/local/accessusage-0.4-1”
   # rpm -i ./accessusage-0.4-1-1.noarch.rpm
To install under a different prefix, use
   # rpm -i --prefix=<INSTALL_DIR> ./accessusage-0.4-1-1.noarch.rpm


R2) Edit /etc/sudoers to grant permission for everyone to run accessusage as the non_root user "accessusage" (created when the rpm was installed).
    This can be done by adding these lines (can copy specific lines from <INSTALL_DIR>/etc/accessusage.sudoers.example):

      Defaults!<INSTALL_DIR>/bin/accessusage env_keep="USER"
      Defaults!<INSTALL_DIR>/bin/accessusage runas_default=accessusage
      ALL ALL=(accessusage) NOPASSWD: <INSTALL_DIR>/bin/accessusage

R3) Create an accessusage_v2.conf (new ACDB database) file.   

    The following steps will need to be generate the /etc/accessusage_v2.conf file 
 
        a) Run the accessusage script as root and it will create an initial configuration file for you at 
           /etc/accessusage_v2.conf.  The script will set the ownership to root:accessusage and the file permissions to 640.

           You can also create the configuration file youself instead.
           Examine the example file <INSTALL_DIR>/etc/accessusage_v2.conf.example for further guidance.
           This example file may be used with minor editing. 
   
           The accessusage script looks for the configuration file in the following ordered locations:
               /etc/accessusage_v2.conf,
               /var/secrets/accessusage_v2.conf,
               <INSTALL_DIR>/etc/accessusage_v2.conf
           This file will contain secrets, so should NOT have world-readable permissions.
           Set its mode to 0640 with owned:group set to root:accessusage.

        b) An API key needs to be generated and configured in the accessusage_v2.conf files. A hash of that 
           API key also needs to be configured into the API that accessusage will call. Instructions for generating the API key 
           and hash and for getting the hash configured in the API are at https://allocations-api.access-ci.org/acdb.

           A resource_name and api_id also needs to be configured in the accessusage_v2.conf file. The resource_name is used by 
           accessusage to map usernames to people in the ACDB. The api_id (along with the api_key) are used to authenticate 
           accessusage to the API. Both of these values should be the same and must match the XDCDB Resource Name as listed at:
            
           https://operations-api.access-ci.org/wh2/cider/v1/access-active/?format=html&sort=info_resourceid
    
           An example accessusage_v2.conf file with the information needed by the API:
             resource_name     = ranger.tacc.teragrid
             rest_url_base     = https://allocations-api.access-ci.org/acdb
             api_id            = ranger.tacc.teragrid
             api_key           = abcdgzCvPliUd2Hxa2k6Z3KCQzbgs8uSzjQpn2O4+62mEO9aEDTYJqcRhktODxyz
             admin_name        = mshapiro

R4) Install an accessusage modules file to make it easier for users to access it. Copy the sample file in
    <INSTALL_DIR>/etc/accessusage.modules.example to /usr/local/Modules/modulefiles/accessusage/0.4-1
    (or a Module directory of your choosing) and mark the default by specifying 0.4-1 in the
    associated /usr/local/Modules/modulefiles/accessusage/.version file.

R5) Change to <INSTALL_DIR> and run:

    # ./bin/accessusage

    If there are config errors, running the script as root will help you identify where they are.  For example

    "Unable to find the 'api_id' value. 
The configuration file (accessusage_v2.conf) should have one entry for each of the following:
	api_key
	api_id
	resource_name
	rest_url_base"

    Otherwise, the output should look something like:

    "As an administrator, you will be given directions to set up accessusage to run on this machine, if needed.
    Where possible, you will also be given instructions to correct any errors that are detected.

    /opt/accessusage/bin/accessusage_v2.py: No projects and/or accounts found"

    This means there are no config errors and you can proceed with user testing.  
    Look at the docs/INSTALL_Testing document for a few test commands run.


**********************
** TAR Installation **
**********************

Prerequisites: sudo and Python3

Installation steps:

T1) Download the desired TAR release from [https://github.com/access-ci-org/accessusage/releases](https://github.com/access-ci-org/accessusage/releases).

T2) Untar the package:
    tar -xzvf accessusage-0.4-1.tgz

T3) Copy accessusage-0.4-1/bin/accessusage to a directory of your choosing, <INSTALL_DIR>.
    The accessusage file should have permissions 0555, owned by root:root, so that it is not
    inadvertently changed.  DO NOT make this file owned by the accessusage user described below.

T4) Create a non-root user and dedicated group that does not map to an actual person.
    We'll refer to this user as 'accessusage' and group 'accessusage', though you can use any name you
    choose.  Generally, you'll want to set this user with: 
      shell=/dev/null 
      password disabled ('*' in the shadow file) 
      home=/

T5) Follow steps R2) thru R5) above
