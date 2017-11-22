Unlock Jenkins
===============

Step 1 Unlock jenkins
----------------------

option 1
*********

Whether jenkins is locked is decided by if /var/lib/jenkins/secrets/initialAdminPassword exists
hence, one way to unlock it is manually creating the file and filling with the version info without
any line terminators, and the password will be the initialAdminPassword

option 2
*********

After executing jenkins-cli.jar safe-restart, the /var/lib/jenkins/secrets/initialAdminPassword
will be generated automatically, so jenkins is unlocked automatically, and the password is
initialAdminPassword as well


Step 2 change the username
---------------------------

It seems like the admin username is determined by the directory name under
/var/lib/jenkins/users/username, rename the directory, the username will
be changed at the same time, for example change from 'admin' to 'jenkins':

```
# mv /var/lib/jenkins/users/admin /var/lib/jenkins/users/jenkins
```
step 3 change the password
----------------------------

The password can be changed via changing the passwordHash element of
/var/lib/jenkins/users/username/config.xml.

The <passwordHash> element in /var/lib/jenkins/users/username/config.xml
accepts data of format

```
salt:sha256("password{salt}")
```

so, you can produce the SHA256 as:

```
echo -n 'password{username}' | sha256sum
```

then, take the hash and put it with the salt into <passwordHash>:

```
<passwordHash>username:hash-of-password</passwordHash>
```

Restart Jenkins, then, the new username/password should have worked.
