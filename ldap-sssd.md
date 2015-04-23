
#Install the SSSD packages

    yum install sssd-*
    yum install authconfig authconfig-tui

> May already be installed. Depending on pattern

Run ` authconfig --test `

It should return a bunch of diagnostic information.

    [root@gamma-7 ~]# authconfig --test
    caching is disabled
    nss_files is always enabled
    nss_compat is disabled
    nss_db is disabled
    nss_hesiod is disabled
     hesiod LHS = ""
     hesiod RHS = ""
    nss_ldap is disabled
     LDAP+TLS is disabled
     LDAP server = ""
     LDAP base DN = ""
    nss_nis is disabled
     NIS server = ""
     NIS domain = ""
    nss_nisplus is disabled
    nss_winbind is disabled
     SMB workgroup = "MYGROUP"
     SMB servers = ""
     SMB security = "user"
     SMB realm = ""
     Winbind template shell = "/bin/false"
     SMB idmap range = "16777216-33554431"
    nss_sss is enabled by default
    nss_wins is disabled
    nss_mdns4_minimal is disabled
    DNS preference over NSS or WINS is disabled
    pam_unix is always enabled
     shadow passwords are enabled
     password hashing algorithm is sha512
    pam_krb5 is disabled
     krb5 realm = "#"
     krb5 realm via dns is disabled
     krb5 kdc = ""
     krb5 kdc via dns is disabled
     krb5 admin server = ""
    pam_ldap is disabled
     LDAP+TLS is disabled
     LDAP server = ""
     LDAP base DN = ""
     LDAP schema = "rfc2307"
    pam_pkcs11 is disabled
     use only smartcard for login is disabled
     smartcard module = ""
     smartcard removal action = ""
    pam_fprintd is enabled
    pam_ecryptfs is disabled
    pam_winbind is disabled
     SMB workgroup = "MYGROUP"
     SMB servers = ""
     SMB security = "user"
     SMB realm = ""
    pam_sss is disabled by default
     credential caching in SSSD is enabled
     SSSD use instead of legacy services if possible is enabled
    IPAv2 is disabled
    IPAv2 domain was not joined
     IPAv2 server = ""
     IPAv2 realm = ""
     IPAv2 domain = ""
    pam_pwquality is enabled (try_first_pass local_users_only retry=3 authtok_type=)
    pam_passwdqc is disabled ()
    pam_access is disabled ()
    pam_mkhomedir or pam_oddjob_mkhomedir is disabled (umask=0077)
    Always authorize local users is enabled ()
    Authenticate system accounts against network services is disabled


##Configure SSSD

Command line :

    authconfig \
    --enablesssd \
    --enablesssdauth \
    --enablelocauthorize \
    --enableldap \
    --enableldapauth \
    --ldapserver=ldap://172.16.31.35:389 \
    --disableldaptls \
    --ldapbasedn=dc=thorin,dc=co \
    --enablerfc2307bis \
    --enablemkhomedir \
    --enablecachecreds \
    --update


##After a while

It should return

Next : Restart sssd

`systemctl restart sssd`

Somewhere in the output should be:

    nss_ldap is enabled
    LDAP+TLS is disabled
    LDAP server = "ldap://172.16.31.35:389"
    LDAP base DN = "dc=thorin,dc=co"

##Running without TLS

For our purposes running without TLS is good enough.  Though in a production environment you should always use LDAP+TLS.

For this we need to modify two files:

1. /etc/openldap/ldap.conf

Add the line:

    TLS_REQCERT never

2. /etc/sysconfig/authconfig

Change the parameter:

    FORCELEGACY=yes

##Test

Run:  `id fili`

If you get back id 1005 you're searching ldap.

Now try ssh.
