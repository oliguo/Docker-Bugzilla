# Docker Bugzilla
```
Using the external mysql as the persistence database;
Using the external phpmyadmin to manage the database if needed;
Using the Ubuntu 18.04 as OS to setup the Bugzilla;
Using the SSH of the OS to remote the site for source update and further development;
Setting the volume to mount the source of Bugzilla on the host side for further development easily: "./src/bugzilla:/var/www/html/bugzilla";
```

# 1 MySQL Info
```
Default password of the root of mysql: root;
Please change "MYSQL_ROOT_PASSWORD" its value in the docker-compose before running up;

MySQL:
Host: mysql-5.7
Account: root
Password: root
```

# 1 MySQL Setup
```
docker network create web
cd mysql-5.7 && docker-compose --compatibility up -d --force-recreate 
cd phpmyadmin-4.7 && docker-compose --compatibility up -d --force-recreate
```

# 2 Bugzilla Info
```
Using the version of bugzilla is: release-5.0-stable
Please change "release-5.0-stable" its value the version if you want in the Dockerfile before building up;

Default password of the root of SSH: 123456
Please change "RUN echo 'root:123456' |chpasswd" its value in the Dockerfile before building up;

Default account and password of bugzilla database in the docker-compose, 
please modify them like the created one by yours, 
if not, please create same database and account in the mysql first before running up;
```

# 2 Bugzilla Setup Steps
```
cd bugzilla && docker build . -t bugzilla-img:1.0.0 \
&& docker-compose --compatibility up -d --force-recreate 
```

# 2 Remote the bugzilla site to set admin and password
```
"
Software error:
The ./data/params.json file does not exist. You probably need to run checksetup.pl. at Bugzilla/Config.pm line 341.
Compilation failed in require at /var/www/html/bugzilla/index.cgi line 15.
BEGIN failed--compilation aborted at /var/www/html/bugzilla/index.cgi line 15.
For help, please send mail to the webmaster (abc@abc.com), giving this error message and the time and date of the error.
"

SSH Way:
    ssh root@dev.oli -p 8022                             
    The authenticity of host '[dev.oli]:8022 ([::1]:8022)' can't be established.
    ED25519 key fingerprint is SHA256:xxxxxx.
    This key is not known by any other names
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added '[dev.oli]:8022' (ED25519) to the list of known hosts.
    root@dev.oli's password: 
    root@dev:~# 
Or Docker:
    docker exec -it bugzilla /bin/sh 

Run:
cd /var/www/html/bugzilla && ./checksetup.pl 
---------------------------------------------------------------------
log reference
---------------------------------------------------------------------
    # cd /var/www/html/bugzilla && ./checksetup.pl
    * This is Bugzilla 5.0.6 on perl 5.26.1
    * Running on Linux 5.10.76-linuxkit #1 SMP Mon Nov 8 10:21:19 UTC 2021

    Checking perl modules...
    Checking for               CGI.pm (v3.51)     ok: found v4.38 
    Checking for           Digest-SHA (any)       ok: found v5.96 
    Checking for             TimeDate (v2.23)     ok: found v2.24 
    Checking for             DateTime (v0.75)     ok: found v1.46 
    Checking for    DateTime-TimeZone (v1.64)     ok: found v2.18 
    Checking for                  DBI (v1.614)    ok: found v1.64 
    Checking for     Template-Toolkit (v2.24)     ok: found v2.27 
    Checking for         Email-Sender (v1.300011) ok: found v1.300031 
    Checking for           Email-MIME (v1.904)    ok: found v1.946 
    Checking for                  URI (v1.55)     ok: found v1.73 
    Checking for       List-MoreUtils (v0.32)     ok: found v0.416 
    Checking for    Math-Random-ISAAC (v1.0.1)    ok: found v1.003 
    Checking for              JSON-XS (v2.01)     ok: found v3.04 

    Checking available perl DBD modules...
    Checking for           DBD-Oracle (v1.19)     not found 
    Checking for           DBD-SQLite (v1.29)     not found 
    Checking for               DBD-Pg (v2.7.0)    not found 
    Checking for            DBD-mysql (v4.001)    ok: found v4.046 

    The following Perl modules are optional:
    Checking for                   GD (v1.20)     ok: found v2.66 
    Checking for                Chart (v2.4.1)    ok: found v2.4.10 
    Checking for          Template-GD (any)       not found 
    Checking for           GDTextUtil (any)       ok: found v0.86 
    Checking for              GDGraph (any)       ok: found v1.48 
    Checking for           MIME-tools (v5.406)    ok: found v5.509 
    Checking for          libwww-perl (any)       ok: found v6.31 
    Checking for             XML-Twig (any)       ok: found v3.50 
    Checking for          PatchReader (v0.9.6)    not found 
    Checking for            perl-ldap (any)       ok: found v0.65 
    Checking for          Authen-SASL (any)       ok: found v2.16 
    Checking for         Net-SMTP-SSL (v1.01)     ok: found v1.04 
    Checking for           RadiusPerl (any)       ok: found v0.26 
    Checking for            SOAP-Lite (v0.712)    ok: found v1.26 
    Checking for          XMLRPC-Lite (v0.712)    ok: found v0.717 
    Checking for             JSON-RPC (any)       ok: found v1.06 
    Checking for           Test-Taint (v1.06)     ok: found v1.06 
    Checking for          HTML-Parser (v3.67)     ok: found v3.72 
    Checking for        HTML-Scrubber (any)       ok: found v0.17 
    Checking for               Encode (v2.21)     ok: found v2.88 
    Checking for        Encode-Detect (any)       ok: found v1.01 
    Checking for          Email-Reply (any)       not found 
    Checking for HTML-FormatText-WithLinks (v0.13)     ok: found v0.15 
    Checking for          TheSchwartz (v1.07)     ok: found v1.12 
    Checking for       Daemon-Generic (any)       ok: found v0.85 
    Checking for             mod_perl (v1.999022) ok: found v2.000010 
    Checking for     Apache-SizeLimit (v0.96)     ok: found v0.97 
    Checking for        File-MimeInfo (any)       ok: found v0.28 
    Checking for           IO-stringy (any)       ok: found v2.111 
    Checking for      Cache-Memcached (any)       not found 
    Checking for  File-Copy-Recursive (any)       not found 
    Checking for           File-Which (any)       ok: found v1.21 
    Checking for              mod_env (any)       ok 
    Checking for          mod_expires (any)       ok 
    Checking for          mod_headers (any)       ok 
    Checking for          mod_rewrite (any)       not found 
    Checking for          mod_version (any)       ok 
    ***********************************************************************
    * OPTIONAL MODULES                                                    *
    ***********************************************************************
    * Certain Perl modules are not required by Bugzilla, but by           *
    * installing the latest version you gain access to additional         *
    * features.                                                           *
    *                                                                     *
    * The optional modules you do not have installed are listed below,    *
    * with the name of the feature they enable. Below that table are the  *
    * commands to install each module.                                    *
    ***********************************************************************
    *         MODULE NAME * ENABLES FEATURE(S)                            *
    ***********************************************************************
    *         Template-GD * Graphical Reports                             *
    *         PatchReader * Patch Viewer                                  *
    *         Email-Reply * Inbound Email                                 *
    *     Cache-Memcached * Memcached Support                             *
    * File-Copy-Recursive * Documentation                                 *
    ***********************************************************************
    * APACHE MODULES                                                      *
    ***********************************************************************
    * Some Apache modules allow to extend Bugzilla functionalities.       *
    * These modules can be enabled in the Apache configuration file       *
    * (usually called httpd.conf or apache2.conf).                        *
    * - mod_headers, mod_env and mod_expires permit to automatically      *
    *   refresh the browser cache of your users when upgrading Bugzilla.  *
    * - mod_rewrite permits to write shorter URLs used by the REST API.   *
    * - mod_version permits to write rules in .htaccess specific to       *
    *   Apache 2.2 or 2.4.                                                *
    * The modules you need to enable are:                                 *
    *                                                                     *
    *    mod_rewrite                                                      *
    *                                                                     *
    ***********************************************************************
    COMMANDS TO INSTALL OPTIONAL MODULES:

        Template-GD: /usr/bin/perl install-module.pl Template::Plugin::GD::Image
        PatchReader: /usr/bin/perl install-module.pl PatchReader
        Email-Reply: /usr/bin/perl install-module.pl Email::Reply
    Cache-Memcached: /usr/bin/perl install-module.pl Cache::Memcached
    File-Copy-Recursive: /usr/bin/perl install-module.pl File::Copy::Recursive


    To attempt an automatic install of every required and optional module
    with one command, do:

    /usr/bin/perl install-module.pl --all

    Reading ./localconfig...

    OPTIONAL NOTE: If you want to be able to use the 'difference between two
    patches' feature of Bugzilla (which requires the PatchReader Perl module
    as well), you should install patchutils from:

        http://cyberelk.net/tim/software/patchutils/

    Checking for            DBD-mysql (v4.001)    ok: found v4.046 
    Checking for                MySQL (v5.0.15)   ok: found v5.7.36 

    Removing existing compiled templates...
    Precompiling templates...done.
    Fixing file permissions...
    Initializing "Dependency Tree Changes" email_setting ...
    Initializing "Product/Component Changes" email_setting ...
    Deriving regex group memberships...

    Looks like we don't have an administrator set up yet. Either this is
    your first time using Bugzilla, or your administrator's privileges
    might have accidentally been deleted.

    Enter the e-mail address of the administrator: abc@abc.com
    Enter the real name of the administrator: abc
    Enter a password for the administrator account: 
    Please retype the password to verify: 
    abc@abc.com is now set up as an administrator.
    Creating initial dummy product 'TestProduct'...

    Now that you have installed Bugzilla, you should visit the 'Parameters'
    page (linked in the footer of the Administrator account) to ensure it
    is set up as you wish - this includes setting the 'urlbase' option to
    the correct URL.
    checksetup.pl complete.
---------------------------------------------------------------------
```

# Others
```
You need to setup the SMTP in the bugzilla console setting since it needs to send email for some features like sending email for forget password, you can check here for information:
https://bugzilla.readthedocs.io/en/latest/installing/essential-post-install-config.html

I recommend to setup a new gmail with 2FA enabled to get the application password for it, you can check:
https://support.google.com/accounts/answer/185833?hl=en

"ssh root@dev.oli -p 8022
kex_exchange_identification: Connection closed by remote host
Connection closed by ::1 port 8022"
If cannot ssh to the bugzilla site like above error, please re-build the container again due to it seems ssh not open yet, after you rebuild and then can connect;
```

# Reference
```
Thanks for the sharing from https://github.com/cssdata/bugzilla
```
