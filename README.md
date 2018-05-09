# How install oci8 and pdo_oci on ubuntu 14.01 and PHP 7.0
## Sources to download
`php 7.0:` https://github.com/php/php-src

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
### 2. Install extension OCI8 in php 7.0
Install php dev tools
```
apt-get install php5.6-dev
```

Access directory to php-src-PHP-7.2.4/ext/oci8 and exec next command
```
phpize

./configure --with-oci8=instantclient,/opt/oracle/instantclient/lib

make install
```
NOTE: 
verify result with phpinfo.php. The result should be the same.
In my case this info is the same like as result when run comand `phpize`. It's not the same, oci8 don't work.

PHP API	20151012

PHP Extension	20151012

Zend Extension	320151012


### 3. Add extension oci8.so to php.ini

Now create file with content oci8.so
```
touch /etc/php/7.0/mods-available/oci8.ini

echo 'extension=oci8.so' > /etc/php/7.0/mods-available/oci8.ini
```
Link files
```
ln -s /etc/php/7.0/mods-available/oci8.ini /etc/php/7.0/apache2/conf.d/20-oci8.ini
ln -s /etc/php/7.0/mods-available/oci8.ini /etc/php/7.0/cli/conf.d/20-oci8.ini

``
### 4. Restart apache

```
service apache2 restart
```

```
Download and extract this Source of php7.0, select version php from master. Then, copy folder `php-7.0.27/ext/pdo_oci` to `/tmp/`   
https://github.com/php/php-src
```
Access directory to php-src-PHP-7.2.4/ext/pdo_oci and exec next command
```
Prepare and build:
```
phpize

./configure --with-pdo-oci=/opt/oracle/instantclient

make install
```

NOTE: When you run `phpize` will show the same info of confinguration phpinfo.php. It's not the same, pdo_oci don't work.

PHP API	20151012

PHP Extension	20151012

Zend Extension	320151012



Now create file with content pdo_oci.so
```
touch /etc/php/7.0/mods-available/pdo_oci.ini

echo 'extension=pdo_oci.so' > /etc/php/7.0/mods-available/pdo_oci.ini
```
Link files
```
ln -s /etc/php/7.0/mods-available/pdo_oci.ini /etc/php/7.0/apache2/conf.d/20-pdo_oci.ini

ln -s /etc/php/7.0/mods-available/pdo_oci.ini /etc/php/7.0/cli/conf.d/20-pdo_oci.ini

```

restart apache

```
service apache2 restart
```

See phpinfo.php in seccion PDO and have `pdo_oci` enabled.





Now you can connect to Oracle DBMS from your PHP applications.
