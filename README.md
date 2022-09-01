# Terminal-server-with-web-interface
A Raspberry Pi based terminal server with a web interface.

```
Install and configure ttyd

2022-08-28

See also: General_Linux_Information#Linux_Terminal_on_Web_Browser

Check with pre-built binary.

hal@termserver-rpi:~ $ dpkg --print-architecture
armhf

2022-08-29

Downloaded ttyd.arm from Releases · tsl0922/ttyd

Created ~/ttyd and moved ttyd.arm to this directory.

2022-08-31

Used ttyd built from source.

Create shell script that start 4 instances of minicom using the ttyUSB ports connected to the RPi.

hal@termserver-rpi:~/ttyd $ cat ttyd_start.sh
# Starting ttyd instances
/home/hal/ttyd/ttyd/build/ttyd -p 7681 -w /home/hal bash > /home/hal/ttyd/console.log 2>&1 &
/home/hal/ttyd/ttyd/build/ttyd -p 7682 -w /home/hal minicom -D /dev/ttyUSB0 -b 9600 > /home/hal/ttyd/ttyUSB0.log 2>&1 &
/home/hal/ttyd/ttyd/build/ttyd -p 7683 -w /home/hal minicom -D /dev/ttyUSB2 -b 9600 > /home/hal/ttyd/ttyUSB2.log 2>&1 &
/home/hal/ttyd/ttyd/build/ttyd -p 7684 -w /home/hal minicom -D /dev/ttyUSB4 -b 9600 > /home/hal/ttyd/ttyUSB4.log 2>&1 &
/home/hal/ttyd/ttyd/build/ttyd -p 7685 -w /home/hal minicom -D /dev/ttyUSB6 -b 9600 > /home/hal/ttyd/ttyUSB6.log 2>&1 &

hal@termserver-rpi:~/ttyd $ crontab -e
no crontab for hal - using an empty one

Select an editor.  To change later, run 'select-editor'.
  1. /bin/nano        <---- easiest
  2. /usr/bin/vim.tiny
  3. /bin/ed

Choose 1-3 [1]: 1
crontab: installing new crontab

added the line

@reboot /home/hal/ttyd/ttyd_start.sh

Nginx configuretion:

hal@termserver-rpi:~/ttyd $ cat /etc/nginx/sites-available/termserver
# Terminal server configuration
#
server {
	listen 80 default_server;
	listen [::]:80 default_server;

        # Allow to use frame from same origin
        add_header X-Frame-Options SAMEORIGIN;

        # Proxy Config
        proxy_redirect          off;
        proxy_set_header        Host            $host;
        proxy_set_header        X-Real-IP       $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        client_max_body_size    10m;
        client_body_buffer_size 128k;
        proxy_connect_timeout   90;
        proxy_send_timeout      90;
        proxy_read_timeout      90;
        proxy_buffers           32 4k;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
	root /var/www/html;

	# Add index.php to the list if you are using PHP
	index index.html index.htm index.nginx-debian.html;

	server_name _;

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}

        location /console/ {
          proxy_pass http://192.168.42.42:7681/;
        }
        location /ttyUSB0/ {
          proxy_pass http://192.168.42.42:7682/;
        }
        location /ttyUSB2/ {
          proxy_pass http://192.168.42.42:7683/;
        }
        location /ttyUSB4/ {
          proxy_pass http://192.168.42.42:7684/;
        }
        location /ttyUSB6/ {
          proxy_pass http://192.168.42.42:7685/;
        }
}
hal@termserver-rpi:~/ttyd $ sudo service nginx configtest
Testing nginx configuration:.
hal@termserver-rpi:~/ttyd $ sudo service nginx reload

Web page:

hal@termserver-rpi:~ $ cat /var/www/html/index.html
<!DOCTYPE html>
<html>
<head>
<title>Camelot Terminal Server</title>
<style>
body {
  font-size: 4vw;
}
@media screen and (min-width: 1000px) {
  body {
     font-size: 25px;
  }
}
</style>
</head>
<body>
<h1>Terminal server ports</h1>
<li><a href="http://192.168.42.42/console">Console (bash)</a></li>
<br>
<li><a href="http://192.168.42.42/ttyUSB0">Zendex Model 835/838 (ttyUSB0)</a></li>
<br>
<li><a href="http://192.168.42.42/ttyUSB2">Telebit Router TBC 2000 (ttyUSB2)</a></li>
<br>
<li><a href="http://192.168.42.42/ttyUSB4">SLICER computer (ttyUSB4)</a></li>
<br>
<li><a href="http://192.168.42.42/ttyUSB6">ACC Amazon Router (ttyUSB6)</a></li>
</body>
</html>
```

