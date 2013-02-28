# phpenv and php-build installation

*NOTE*: these are quick notes. I used phpenv and php-build to create an environment suitable for the Symfony Locale component development. The definitions files are old, just for documentation purposes. I will post a better how to at my [blog](http://blog.eriksen.com.br). Commands tested in a 64bit Debian Squeeze.

First, install some dependencies to compile PHP:

    sudo aptitude install re2c flex bison make
    sudo aptitude install libxml2-dev libcurl4-dev libjpeg-dev libpng-dev libmcrypt-dev lemon libtidy-dev libxslt-dev

Install phpenv (php-build and php-build-plugin-phpunit as its plugins):

    git clone git://github.com/phpenv/phpenv.git .phpenv
    git clone git://github.com/humanshell/phpenv.git .phpenv
    cat .bash_profile
    mkdir -p ~/.phpenv/plugins
    cd .phpenv/plugins/
    git clone git://github.com/CHH/php-build.git
    git clone git://github.com/CHH/php-build-plugin-phpunit.git
    ls php-build/share/php-build/after-install.d/
    ln -s `pwd`/php-build-plugin-phpunit/share/php-build/after-install.d/phpunit php-build/share/php-build/after-install.d/phpunit

To make php-build working as a phpenv plugin, rename the `phpenv-install` script (as suggested in [this comment](https://github.com/CHH/php-build/issues/68#issuecomment-6231388)):

    $ cd php-build/bin/
    $ cp phpenv-install phpenv-build

To prevent a failed install of any PEAR library, change the `pyrus.sh` script to force auto-discovery of dependencies:

    $ cd ../share/php-build/plugins.d/
    $ vi pyrus.sh

Change line 59:

    59     <auto_discover>1</auto_discover>

Build (yeah, you need to change to the definitions directory - build ICU 49 first):

    $ cd ~/definitions
    $ phpenv build 5.4.12-icu-49

    phpenv v0.0.2

    [Info]: Loaded apc Plugin.
    [Info]: Loaded pyrus Plugin.
    [Info]: Loaded xdebug Plugin.
    [Info]: Loaded xhprof Plugin.
    [Info]: php.ini-production gets used as php.ini
    [Info]: Building 5.4.12-icu-49 into /home/vagrant/.phpenv/versions/5.4.12-icu-49
    [Downloading]: http://www.php.net/distributions/php-5.4.12.tar.bz2
    [Preparing]: /var/tmp/php-build/source/5.4.12-icu-49
    [Compiling]: /var/tmp/php-build/source/5.4.12-icu-49
    [Pyrus]: Downloading from http://pear2.php.net/pyrus.phar
    [Pyrus]: Installing executable in /home/vagrant/.phpenv/versions/5.4.12-icu-49/bin/pyrus
    [XDebug]: Downloading http://xdebug.org/files/xdebug-2.2.0.tgz
    [XDebug]: Compiling in /var/tmp/php-build/source/xdebug-2.2.0
    [XDebug]: Installing XDebug configuration in /home/vagrant/.phpenv/versions/5.4.12-icu-49/etc/conf.d/xdebug.ini
    [XDebug]: Cleaning up.
    [After Install Trigger]: phpunit
    Pyrus version 2.0.0a4 SHA-1: 72271D92C3AA1FA96DF9606CD538868544609A52
    Using PEAR installation found at /home/vagrant/.phpenv/versions/5.4.12-icu-49/share/pyrus/.pear
    Discovery of channel components.ez.no successful
    Pyrus version 2.0.0a4 SHA-1: 72271D92C3AA1FA96DF9606CD538868544609A52
    Using PEAR installation found at /home/vagrant/.phpenv/versions/5.4.12-icu-49/share/pyrus/.pear
    Discovery of channel pear.symfony-project.com successful
    Pyrus version 2.0.0a4 SHA-1: 72271D92C3AA1FA96DF9606CD538868544609A52
    Using PEAR installation found at /home/vagrant/.phpenv/versions/5.4.12-icu-49/share/pyrus/.pear
    Discovery of channel pear.symfony.com successful
    Pyrus version 2.0.0a4 SHA-1: 72271D92C3AA1FA96DF9606CD538868544609A52
    Using PEAR installation found at /home/vagrant/.phpenv/versions/5.4.12-icu-49/share/pyrus/.pear
    Mime-type: text/plain
    Mime-type: text/plain================================================================================>] 100% (230/230 kb)
    Downloading components.ez.no/Base====================================================================>] 100% (849/849 kb)
    Downloading components.ez.no/ConsoleTools
    Installed components.ez.no/Base-1.8
    Installed components.ez.no/ConsoleTools-1.6.1
    Pyrus version 2.0.0a4 SHA-1: 72271D92C3AA1FA96DF9606CD538868544609A52
    Using PEAR installation found at /home/vagrant/.phpenv/versions/5.4.12-icu-49/share/pyrus/.pear
    Downloading pear.phpunit.de/PHPUnit
    Mime-type: application/x-tgz
    Downloading pear.phpunit.de/File_Iterator============================================================>] 100% (114/114 kb)
    Mime-type: application/x-tgz
    Downloading pear.phpunit.de/Text_Template============================================================>] 100% ( 5/ 5 kb)
    Mime-type: application/x-tgz
    Downloading pear.phpunit.de/PHP_CodeCoverage=========================================================>] 100% ( 3/ 3 kb)
    Mime-type: application/x-tgz
    Downloading pear.phpunit.de/PHP_TokenStream==========================================================>] 100% (155/155 kb)
    Mime-type: application/x-tgz
    Downloading pear.phpunit.de/PHP_Timer================================================================>] 100% ( 9/ 9 kb)
    Mime-type: application/x-tgz
    Downloading pear.phpunit.de/PHPUnit_MockObject=======================================================>] 100% ( 3/ 3 kb)
    Mime-type: application/x-tgz
    Downloading pear.symfony.com/Yaml====================================================================>] 100% (19/19 kb)
    Mime-type: application/x-tar
    Installed pear.phpunit.de/PHPUnit-3.7.14=============================================================>] 100% (38/38 kb)
    Installed pear.phpunit.de/File_Iterator-1.3.3
    Installed pear.phpunit.de/Text_Template-1.1.4
    Installed pear.phpunit.de/PHP_CodeCoverage-1.2.9
    Installed pear.phpunit.de/PHP_Timer-1.0.4
    Installed pear.phpunit.de/PHPUnit_MockObject-1.2.3
    Installed pear.symfony.com/Yaml-2.1.8
    Installed pear.phpunit.de/PHP_TokenStream-1.1.5
    Optional dependencies that will not be installed, use --optionaldeps:
    pear.phpunit.de/PHP_Invoker depended on by pear.phpunit.de/PHPUnit
    [Info]: The Log File is not empty, but the Build did not fail. Maybe just warnings got logged. You can review the log in /tmp/php-build.5.4.12-icu-49.20130228041127.log
    [Success]: Built 5.4.12-icu-49 successfully.
    phpenv v0.0.2

Check the available versions in your environment:

    $ phpenv versions
    phpenv v0.0.2

    * system (set by /home/vagrant/.phpenv/version)
      5.4.12-icu-49
      
    $ php -v
    PHP 5.4.7-1~dotdeb.0 (cli) (built: Sep 15 2012 09:53:20) 
    Copyright (c) 1997-2012 The PHP Group
    Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies

Set a different PHP version to the global environment:

    $ phpenv global 5.4.12-icu-49
    $ phpunit --version
    PHPUnit 3.7.14 by Sebastian Bergmann.

    $ php -v
    PHP 5.4.12 (cli) (built: Feb 28 2013 04:16:38)
    Copyright (c) 1997-2013 The PHP Group
    Zend Engine v2.4.0, Copyright (c) 1998-2013 Zend Technologies
        with Xdebug v2.2.0, Copyright (c) 2002-2012, by Derick Rethans

    $ php -i | grep intl
    Configure Command =>  './configure'  '--without-pear' '--with-gd' '--enable-sockets' '--with-jpeg-dir=/usr' '--with-png-dir=/usr' '--enable-exif' '--enable-zip' '--with-zlib' '--with-zlib-dir=/usr' '--with-kerberos' '--with-openssl' '--with-mcrypt=/usr' '--enable-soap' '--enable-xmlreader' '--with-xsl' '--enable-ftp' '--enable-cgi' '--with-curl=/usr' '--with-tidy' '--with-xmlrpc' '--enable-sysvsem' '--enable-sysvshm' '--enable-shmop' '--with-mysql=mysqlnd' '--with-mysqli=mysqlnd' '--with-pdo-mysql=mysqlnd' '--with-pdo-sqlite' '--enable-pcntl' '--with-readline' '--enable-mbstring' '--disable-debug' '--enable-fpm' '--enable-intl' '--with-icu-dir=/opt/c/icu/49' '--with-config-file-path=/home/vagrant/.phpenv/versions/5.4.12-icu-49/etc' '--with-config-file-scan-dir=/home/vagrant/.phpenv/versions/5.4.12-icu-49/etc/conf.d' '--prefix=/home/vagrant/.phpenv/versions/5.4.12-icu-49'
    PHP Warning:  Unknown: It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected the timezone 'UTC' for now, but please set date.timezone to select your timezone. in Unknown on line 0
    intl
    intl.default_locale => no value => no value
    intl.error_level => 0 => 0

Now you're done.

## Building multiple ICU versions

ICU is simple to build.

    $ cd /opt/
    $ sudo mkdir src c
    $ cd src/
    $ sudo mkdir icu

ICU 4.0 (patched):

    $ cd /opt/src/icu
    $ sudo wget http://download.icu-project.org/files/icu4c/4.0.1/icu4c-4_0_1-src.tgz
    $ sudo tar xzvf icu4c-4_0_1-src.tgz
    $ sudo mv icu 4.0
    $ cd 4.0
    $ sudo wget https://raw.github.com/gist/893710/ce5f865d4be7b36419b39e82d64d718bcde9f878/icu-4.0-gcc-4.4.patch
    $ sudo patch --verbose -p0 < icu-4.0-gcc-4.4.patch
    $ cd source
    $ sudo mkdir -p /opt/c/icu/4.0
    $ sudo ./configure --prefix=/opt/c/icu/4.0
    $ sudo make
    $ sudo make install

ICU 4.2:

    $ cd /opt/src/icu
    $ sudo wget http://download.icu-project.org/files/icu4c/4.2.1/icu4c-4_2_1-src.tgz
    $ sudo tar xzvf icu4c-4_2_1-src.tgz
    $ sudo mv icu 4.2
    $ cd 4.2/source
    $ sudo mkdir -p /opt/c/icu/4.2
    $ sudo ./configure --prefix=/opt/c/icu/4.2
    $ sudo make
    $ sudo make install

ICU 4.4:

    $ cd /opt/src/icu
    $ sudo wget http://download.icu-project.org/files/icu4c/4.4.2/icu4c-4_4_2-src.tgz
    $ sudo tar xzvf icu4c-4_4_2-src.tgz
    $ sudo mv icu 4.4
    $ cd 4.4/source
    $ sudo mkdir -p /opt/c/icu/4.4
    $ sudo ./configure --prefix=/opt/c/icu/4.4
    $ sudo make
    $ sudo make install

ICU 4.6:

    $ cd /opt/src/icu
    $ sudo wget http://download.icu-project.org/files/icu4c/4.6.1/icu4c-4_6_1-src.tgz
    $ sudo tar xzvf icu4c-4_6_1-src.tgz
    $ sudo mv icu 4.6
    $ cd 4.6/source
    $ sudo mkdir -p /opt/c/icu/4.6
    $ sudo ./configure --prefix=/opt/c/icu/4.6
    $ sudo make
    $ sudo make install

ICU 4.8:

    $ cd /opt/src/icu
    $ sudo wget http://download.icu-project.org/files/icu4c/4.8.1.1/icu4c-4_8_1_1-src.tgz
    $ sudo tar xzvf icu4c-4_8_1_1-src.tgz
    $ sudo mv icu 4.8
    $ cd 4.8/source
    $ sudo mkdir -p /opt/c/icu/4.8
    $ sudo ./configure --prefix=/opt/c/icu/4.8
    $ sudo make
    $ sudo make install

ICU 49:

    $ cd /opt/src/icu
    $ sudo wget http://download.icu-project.org/files/icu4c/49.1.2/icu4c-49_1_2-src.tgz
    $ sudo tar xzvf icu4c-49_1_2-src.tgz
    $ sudo mv icu 49
    $ cd 49/source
    $ sudo mkdir -p /opt/c/icu/49
    $ sudo ./configure --prefix=/opt/c/icu/49
    $ sudo make
    $ sudo make install

To build the res files, I recommend using the `genrb` command from the same ICU install. The problem is that using a specific `genrb` (e.g. `/opt/c/icu/4.0/bin/genrb`) do not work:

    $ ldd /opt/c/icu/4.0/bin/genrb 
    	linux-vdso.so.1 =>  (0x00007fff3fbba000)
    	libicui18n.so.40 => not found
    	libicutu.so.40 => not found
    	libicuuc.so.40 => not found
    	libicudata.so.40 => not found
    	libpthread.so.0 => /lib/libpthread.so.0 (0x00007f1ac547e000)
    	libstdc++.so.6 => /usr/lib/libstdc++.so.6 (0x00007f1ac5169000)
    	libm.so.6 => /lib/libm.so.6 (0x00007f1ac4ee7000)
    	libgcc_s.so.1 => /lib/libgcc_s.so.1 (0x00007f1ac4cd1000)
    	libc.so.6 => /lib/libc.so.6 (0x00007f1ac496e000)
    	/lib64/ld-linux-x86-64.so.2 (0x00007f1ac56a3000)

You need to export `LD_LIBRARY_PATH` to make it working:

    $ export LD_LIBRARY_PATH='/opt/c/icu/4.0/lib:/opt/c/icu/4.2/lib:/opt/c/icu/4.4/lib:/opt/c/icu/4.6/lib:/opt/c/icu/4.8/lib:/opt/c/icu/49/lib'

Check:

    $ ldd /opt/c/icu/4.0/bin/genrb 
    	linux-vdso.so.1 =>  (0x00007fffe17ff000)
    	libicui18n.so.40 => /opt/c/icu/4.0/lib/libicui18n.so.40 (0x00007f4b2b462000)
    	libicutu.so.40 => /opt/c/icu/4.0/lib/libicutu.so.40 (0x00007f4b2b245000)
    	libicuuc.so.40 => /opt/c/icu/4.0/lib/libicuuc.so.40 (0x00007f4b2aefe000)
    	libicudata.so.40 => /opt/c/icu/4.0/lib/libicudata.so.40 (0x00007f4b29fb9000)
    	libpthread.so.0 => /lib/libpthread.so.0 (0x00007f4b29d96000)
    	libstdc++.so.6 => /usr/lib/libstdc++.so.6 (0x00007f4b29a82000)
    	libm.so.6 => /lib/libm.so.6 (0x00007f4b29800000)
    	libgcc_s.so.1 => /lib/libgcc_s.so.1 (0x00007f4b295e9000)
    	libc.so.6 => /lib/libc.so.6 (0x00007f4b29287000)
    	/lib64/ld-linux-x86-64.so.2 (0x00007f4b2b7f5000)