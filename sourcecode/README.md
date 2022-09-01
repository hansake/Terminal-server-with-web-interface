# Build ttyd from source on RPi
```
hal@termserver-rpi:~/ttyd $ sudo apt install build-essential cmake git libjson-c-dev libwebsockets-dev
...
hal@termserver-rpi:~/ttyd $ git clone https://github.com/tsl0922/ttyd.git
Cloning into 'ttyd'...
remote: Enumerating objects: 4570, done.
remote: Counting objects: 100% (72/72), done.
remote: Compressing objects: 100% (57/57), done.
remote: Total 4570 (delta 33), reused 31 (delta 15), pack-reused 4498
Receiving objects: 100% (4570/4570), 13.34 MiB | 8.44 MiB/s, done.
Resolving deltas: 100% (3160/3160), done.
hal@termserver-rpi:~/ttyd $ cd ttyd && mkdir build && cd build
hal@termserver-rpi:~/ttyd/ttyd/build $ cmake ..
-- The C compiler identification is GNU 10.2.1
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Found Git: /usr/bin/git (found version "2.30.2") 
-- Found LIBUV: /usr/lib/arm-linux-gnueabihf/libuv.so  
-- Found JSON-C: /usr/lib/arm-linux-gnueabihf/libjson-c.so  
-- Found ZLIB: /usr/lib/arm-linux-gnueabihf/libz.so (found version "1.2.11") 
-- Looking for LWS_WITH_LIBUV
-- Looking for LWS_WITH_LIBUV - found
-- Looking for LWS_OPENSSL_SUPPORT
-- Looking for LWS_OPENSSL_SUPPORT - found
-- Looking for LWS_WITH_MBEDTLS
-- Looking for LWS_WITH_MBEDTLS - not found
-- Found OpenSSL: /usr/lib/arm-linux-gnueabihf/libcrypto.so (found version "1.1.1n")  
-- Configuring done
-- Generating done
-- Build files have been written to: /home/hal/ttyd/ttyd/build
hal@termserver-rpi:~/ttyd/ttyd/build $ make
Scanning dependencies of target ttyd
[ 16%] Building C object CMakeFiles/ttyd.dir/src/utils.c.o
[ 33%] Building C object CMakeFiles/ttyd.dir/src/pty.c.o
[ 50%] Building C object CMakeFiles/ttyd.dir/src/protocol.c.o
[ 66%] Building C object CMakeFiles/ttyd.dir/src/http.c.o
[ 83%] Building C object CMakeFiles/ttyd.dir/src/server.c.o
[100%] Linking C executable ttyd
[100%] Built target ttyd
```
Testing to run ttyd
```
hal@termserver-rpi:~/ttyd/ttyd/build $ ./ttyd bash
[2022/08/30 12:23:05:5698] N: ttyd 1.7.1-b8a88f6 (libwebsockets 4.0.20)
[2022/08/30 12:23:05:5702] N: tty configuration:
[2022/08/30 12:23:05:5702] N:   start command: bash
[2022/08/30 12:23:05:5703] N:   close signal: SIGHUP (1)
[2022/08/30 12:23:05:5703] N:   terminal type: xterm-256color
[2022/08/30 12:23:05:5706] N:  Using foreign event loop...
[2022/08/30 12:23:05:5711] N:  Listening on port: 7681
```
# Install NGINX web server

Used as web page with links to different serial port web terminals and to translate from devices to port numbers.
* [https://pimylifeup.com/raspberry-pi-nginx/ Build your own Raspberry Pi NGINX Web Server &#45; Pi My Life Up]
```
<pre>
hal@termserver-rpi:~ $ sudo apt update
...
hal@termserver-rpi:~ $ sudo apt upgrade
...
hal@termserver-rpi:~ $ sudo apt install nginx
...
hal@termserver-rpi:~ $ sudo systemctl start nginx
```
The test page is here:
```
hal@termserver-rpi:~ $ ls /var/www/html
index.nginx-debian.html
```
