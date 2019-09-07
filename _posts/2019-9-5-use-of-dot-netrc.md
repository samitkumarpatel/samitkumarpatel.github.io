---
layout: post
title: use of .netrc for git
---


*NB:* This approach stores your BitBucket or Github credentials in clear text in your home folder. Therefore, do NOT use this approach on a public server/user

**Prerequisites:**

`GIT client version >= 0.99`

**Procedure (Linux):**
* Create a file in $HOME called .netrc: `$ touch ~/.netrc`
* Add the following contents to the file. Replace login with your Email address and password with your Bitbucket token: 

```
# Make changes to the file
$ vi ~/.netrc
# It should have the following format. Replace email and token with your own
$ cat ~/.netrc

machine github.com
login github@login
password git_password
```

* Make it executable by the user:

```
$ chmod u+x ~/.netrc
```

* Now, git clone/pull with the current user will not be prompted for password each time.

**Procedure (Windows):**
* Create a _netrc file under %USERPROFILE% with the following content. 
Replace USER and TOKEN with your email address and token:

```
C:\Users> cd %USERPROFILE%
C:\Users\MyUser> echo machine github.com > _netrc
C:\Users\MyUser> echo login USER >> _netrc
C:\Users\MyUser> echo password TOKEN >> _netrc
```

* The file should look something like this:

```
machine github.com
login USER
password TOKEN
```


NB: Notice that the email address must have @ here. I.e. using %40 (as earlier) will not work

* Now, git clone/pull with the current user will not be prompted for password each time
