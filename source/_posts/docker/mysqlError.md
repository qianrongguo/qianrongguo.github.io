 Client does not support authentication protocol requested by server; consider upgrading MySQL client
 
解决方法：

`mysql -u root -p`

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Ll123456'`

`flush privileges;`

相关连接：https://medium.com/@nickhuang_1199/client-does-not-support-authentication-protocol-requested-by-server-consider-upgrading-mysql-98540705553f

