## Add user principal using cloudera-scm
<pre><code>
kadmin.local -p cloudera-scm@ARTHURBAUDRY.COM
addprinc arthurbaudry@ARTHURBAUDRY.COM
quit
</code></pre>

## Test that Kerberos is enabled
<pre><code>
su arthurbaudry
hdfs dfs -ls 

Caused by: GSSException: No valid credentials provided (Mechanism level: Failed to find any Kerberos tgt)
	at sun.security.jgss.krb5.Krb5InitCredential.getInstance(Krb5InitCredential.java:147)
	at sun.security.jgss.krb5.Krb5MechFactory.getCredentialElement(Krb5MechFactory.java:121)
	at sun.security.jgss.krb5.Krb5MechFactory.getMechanismContext(Krb5MechFactory.java:187)
	at sun.security.jgss.GSSManagerImpl.getMechanismContext(GSSManagerImpl.java:223)
	at sun.security.jgss.GSSContextImpl.initSecContext(GSSContextImpl.java:212)
	at sun.security.jgss.GSSContextImpl.initSecContext(GSSContextImpl.java:179)
	at com.sun.security.sasl.gsskerb.GssKrb5Client.evaluateChallenge(GssKrb5Client.java:193)
	... 41 more
ls: Failed on local exception: java.io.IOException: javax.security.sasl.SaslException: GSS initiate failed [Caused by GSSException: No valid credentials provided (Mechanism level: Failed to find any Kerberos tgt)]; Host Details : local host is: "ip-172-31-9-39.eu-west-1.compute.internal/172.31.9.39"; destination host is: "ip-172-31-9-37.eu-west-1.compute.internal":8020; 
</code></pre>

## kinit arthurbaudry
<pre><code>
kinit arthurbaudry
klist
Ticket cache: FILE:/tmp/krb5cc_1000
Default principal: arthurbaudry@ARTHURBAUDRY.COM

Valid starting     Expires            Service principal
11/17/16 11:50:26  11/18/16 11:50:26  krbtgt/ARTHURBAUDRY.COM@ARTHURBAUDRY.COM
	renew until 11/24/16 11:50:26

hdfs dfs -ls
Found 9 items
drwx------   - arthurbaudry arthurbaudry          0 2016-11-17 00:00 .Trash
drwx------   - arthurbaudry arthurbaudry          0 2016-11-15 17:36 .staging
drwxrwxrwx   - arthurbaudry arthurbaudry          0 2016-11-17 10:18 2015_11_18
drwxrwxrwx   - arthurbaudry arthurbaudry          0 2016-11-17 10:18 2015_11_19
drwxrwxrwx   - arthurbaudry arthurbaudry          0 2016-11-17 10:18 2015_11_20
drwxrwxrwx   - arthurbaudry arthurbaudry          0 2016-11-17 10:18 2015_11_21
drwxr-xr-x   - arthurbaudry arthurbaudry          0 2016-11-15 18:05 precious
drwxr-xr-x   - arthurbaudry arthurbaudry          0 2016-11-15 16:47 terasort
drwxr-xr-x   - arthurbaudry arthurbaudry          0 2016-11-15 17:36 terasort_out
</code></pre>

## Creating a keytab
<pre><code>
kadmin.local -p cloudera-scm@ARTHURBAUDRY.COM
xst -norandkey -k arthurbaudry.keytab arthurbaudry@ARTHURBAUDRY.COM
mv arthurbaudry.keytab /home/arthurbaudry/
chown arthurbaudry /home/arthurbaudry/arthurbaudry.keytab 
chmod 400 /home/arthurbaudry/arthurbaudry.keytab


kinit -kt arthurbaudry.keytab arthurbaudry@ARTHURBAUDRY.COM
</code></pre>