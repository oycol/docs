```shell
CREATE DATABASE samba_auth;

CREATE TABLE users (
  uid int(6) NOT NULL,
  gid int(6) NOT NULL,
  username varchar(16) NOT NULL,
  password varchar(16) NOT NULL,
  PRIMARY KEY (uid)
);

  gecos varchar(16) NUll,
  homedir varchar(16) default "/home/huangwx" ,
  shell varchar(16) default "/usr/bin/bash" ,
  
INSERT INTO users VALUES ('3000', '3000','huangwx',ENCRYPT('123456'));
INSERT INTO users VALUES ('3001', '3001','user1',ENCRYPT('123456'));
INSERT INTO users VALUES ('3002', '3002','user2',ENCRYPT('123456'));

INSERT INTO users VALUES ('0', '0','root',ENCRYPT('123456'));
```

