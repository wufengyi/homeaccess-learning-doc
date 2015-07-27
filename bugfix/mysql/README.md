
Resolve mysql server 5.5 installation problem
================================

When I install mysql server on my ubuntu 14.04 machine,an error occur.

It says:"Package mysql-server-5.5 is not configured yet."

Every time I met with problems that I have no idea about,I turn to the search engine.Google or Baidu?Which one ?
definitely,Google.Google is the true engine trying to teach you something.not the engine give you some ads,make you uncomfortable.

Blow is the case:


View the problem solving process by other guys.
-------------------------------------------

1. Type "Package mysql-server-5.5 is not configured yet." into google search bar,press enter:


2. Go to *[this link](http://stackoverflow.com/questions/13276088/cant-start-mysql5-5-on-ubuntu-12-04-dpkg-dependency-problems).*:

3. View the first answer by Ihsan Kusasi:


4. Follow the same steps as Ihsan Kusasi said:


Verify the result
------------

1. In linux comand line,type blow:

        root@Server-U12:/home# mysql -u root -p

2. Enter the password ,in this case ,is "root":
        
        Enter password:
   
3. Verify the result:

        Welcome to the MySQL monitor.  Commands end with ; or \g.
        Your MySQL connection id is 48
        Server version: 5.5.44-0ubuntu0.14.04.1 (Ubuntu)

        Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

        Oracle is a registered trademark of Oracle Corporation and/or its
        affiliates. Other names may be trademarks of their respective
        owners.

        Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
        
        mysql> quit
        Bye


Useful link
--------------------

*[Mysql](http://dev.mysql.com/doc/refman/5.5/en/mysql-commands.html).*





