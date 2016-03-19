title: GPG

[TOC]

## [How to change the expiration date of a GPG key](http://www.g-loaded.eu/2010/11/01/change-expiration-date-gpg-key/)

```
$ gpg --list-keys
pub   1024D/B989893B 2007-03-07 [expired: 2009-12-31]
uid                  George Notaras <gnotaras@example.org>
sub   4096g/320D81EE 2007-03-07 [expired: 2009-12-31]
```
其中 B989893B 就是 ID

```
$ gpg --edit-key B989893B
```

```
gpg> list

pub  1024D/B989893B  created: 2007-03-07  expired: 2009-12-31  usage: SCA
                     trust: ultimate       validity: ultimate
sub  4096g/320D81EE  created: 2007-03-07  expired: 2009-12-31  usage: E
[ ultimate] (1). George Notaras <gnotaras@example.org>
```

选择 key 0
```
gpg> key 0

pub  1024D/B989893B  created: 2007-03-07  expired: 2009-12-31  usage: SCA
                     trust: ultimate       validity: ultimate
sub  4096g/320D81EE  created: 2007-03-07  expired: 2009-12-31  usage: E
[ ultimate] (1). George Notaras <gnotaras@example.org>
```

修改期限
```
gpg> expire
Changing expiration time for the primary key.
Please specify how long the key should be valid.
         0 = key does not expire
        = key expires in n days
      w = key expires in n weeks
      m = key expires in n months
      y = key expires in n years
Key is valid for? (0) 2y
Key expires at 10/28/12 03:51:07
Is this correct? (y/N) y

You need a passphrase to unlock the secret key for
user: "George Notaras <gnotaras@example.org>"
1024-bit DSA key, ID XXXXXXXX, created 2007-03-07


pub  1024D/B989893B  created: 2007-03-07  expires: 2012-10-28  usage: SCA
                     trust: ultimate       validity: ultimate
sub  4096g/320D81EE  created: 2007-03-07  expired: 2009-12-31  usage: E
[ ultimate] (1). George Notaras <gnotaras@example.org>

```

选择 子 key 1
```
gpg> key 1

pub  1024D/B989893B  created: 2007-03-07  expires: 2012-10-28  usage: SCA
                     trust: ultimate       validity: ultimate
sub*  4096g/320D81EE  created: 2007-03-07  expired: 2009-12-31  usage: E
[ ultimate] (1). George Notaras <gnotaras@example.org>
```

修改期限
```
gpg> expire
Changing expiration time for a subkey.
Please specify how long the key should be valid.
         0 = key does not expire
        = key expires in n days
      w = key expires in n weeks
      m = key expires in n months
      y = key expires in n years
Key is valid for? (0) 2y
Key expires at 10/28/12 03:02:43
Is this correct? (y/N) y

You need a passphrase to unlock the secret key for
user: "George Notaras <gnotaras@example.org>"
1024-bit DSA key, ID XXXXXXXX, created 2007-03-07


pub  1024D/B989893B  created: 2007-03-07  expires: 2012-10-28  usage: SCA
                     trust: ultimate       validity: ultimate
sub* 4096g/320D81EE  created: 2007-03-07  expires: 2012-10-28  usage: E
[ ultimate] (1). George Notaras <gnotaras@example.org>
```

保存修改
```
gpg> save
```

上传服务器
```
$ gpg --keyserver pgp.mit.edu --send-keys B989893B
gpg: sending key B989893B to hkp server pgp.mit.edu
```

## [输出密钥](http://www.ruanyifeng.com/blog/2013/07/gpg.html)
公钥文件（.gnupg/pubring.gpg）以二进制形式储存，armor参数可以将其转换为ASCII码显示。
```
gpg --armor --output public-key.txt --export [用户ID]
```

"用户ID"指定哪个用户的公钥，output参数指定输出文件名（public-key.txt）。
类似地，export-secret-keys参数可以转换私钥。

```
gpg --armor --output private-key.txt --export-secret-keys
```

