# How install oci8 and pdo_oci on ubuntu 14.01 and PHP 7.0
## Sources to download
`php 7.0:` http://us1.php.net/get/php-7.0.27.tar.bz2/from/a/mirror

`oracle:` http://www.oracle.com/technetwork/database/database-technologies/instant-client/downloads/index.html
### 1. Install Oracle Instant Client and SDK version 12.1.0.2.0

Find and Download just these two package: 
1. `instantclient-basic-linux.x64-12.1.0.2.0.zip`
2. `instantclient-sdk-linux.x64-12.1.0.2.0.zip`

Create directory instantclient.

```
mkdir -p /opt/oracle/instantclient
``` 

Unzip Package `instantclient-basic-linux.x64-12.1.0.2.0.zip` in /opt/oracle

```
unzip instantclient-basic-linux.x64-12.1.0.2.0.zip -d /opt/oracle
```
It'll create the directory, rename with next comand.

```
mv /opt/oracle/instantclient_12_1 /opt/oracle/instantclient/lib
```

The same with package sdk `instantclient-sdk-linux.x64-12.1.0.2.0.zip`

``` 
unzip instantclient-sdk-linux.x64-12.1.0.2.0.zip -d /opt/oracle

mv /opt/oracle/instantclient_12_1/sdk/include /opt/oracle/instantclient/include
```
Link files
```
ln -s /opt/oracle/instantclient/lib/libclntsh.so.12.1 /opt/oracle/instantclient/lib/libclntsh.so

ln -s /opt/oracle/instantclient/lib/libocci.so.12.1 /opt/oracle/instantclient/lib/libocci.so
```
Add libraries directory to system
```
echo /opt/oracle/instantclient/lib >> /etc/ld.so.conf

ldconfig
```
### 2. Install aditional package
```
apt-get install php7.0-dev php-pear build-essential libaio1
```
### 3. Install extension OCI8 in php 7.0

Run next comand for install and configure oci8
```
pecl install oci8
```
When you are prompted for the Instant Client location, enter the following:

```
instantclient,/opt/oracle/instantclient_12_1
```
NOTE: 
verify result with phpinfo.php. The result should be the same.
In my case this info is the same like as result when run comand `pecl install oci8`. It's not the same, oci8 don't work.

PHP API	20151012

PHP Extension	20151012

Zend Extension	320151012


### 4. Add extension oci8.so to php.ini

Run commands to add extension in php.ini

```
echo "extension = oci8.so" >> /etc/php/7.0/cli/php.ini
echo "extension = oci8.so" >> /etc/php/7.0/apache2/php.ini
```

### 5. Restart apache

```
service apache2 restart
```
Now check if the extension is enabled with next comand.
```
php -m | grep 'oci8'
```

If returns `oci8`, its works!

Now you can connect to Oracle DBMS from your PHP applications.
