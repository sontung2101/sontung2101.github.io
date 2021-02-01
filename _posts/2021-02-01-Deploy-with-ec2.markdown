---
layout: post
title: "Deploy Django + EC2 + S3 + Nginx + Gunicorn +Ubuntu server"
date: 2021-02-01 11:11:18 +0700
image: ec2.jpg
tags: [Django, Deploy]
categories: Django
---
<p><span style="font-size: 14pt; color: #ff0000;">-<span style="font-family: 'comic sans ms', sans-serif;">Khi deploy Django, stack của ch&uacute;ng ta sẽ bao gồm c&aacute;c th&agrave;nh phần sau:</span></span></p>
<ul>
<li><span style="font-size: 14pt; font-family: 'comic sans ms', sans-serif; color: #ff0000;">NGINX l&agrave; server gateway đồng thời cũng l&agrave; một reverse proxy.</span></li>
<li><span style="font-size: 14pt; font-family: 'comic sans ms', sans-serif; color: #ff0000;">WSGI server (uWSGI hoặc gunicorn) chạy Django.</span></li>
<li><span style="font-size: 14pt; font-family: 'comic sans ms', sans-serif; color: #ff0000;">Server cơ sở dữ liệu (PostgreSQL, MySQL, ...).</span></li>
<li><span style="font-size: 14pt; font-family: 'comic sans ms', sans-serif; color: #ff0000;">Server kh&aacute;c nếu cần (memcached, Redis, ...)</span></li>
</ul>
<p><span style="font-size: 14pt;">Tutorial Deploy django s3 :&nbsp;<a href="https://www.youtube.com/watch?v=u0oEIqQV_-E&amp;list=PLX4uXM5lVU53JbQ_1ijxpU0qZIOrJOG--" target="_blank">Django+ec2</a></span></p>
<p><span style="font-size: 14pt;">-gồm 3 video :</span></p>
<p><span style="font-size: 14pt;">+ part 1 : hướng dẫn deploy l&ecirc;n ec2 v&agrave; cấu h&igrave;nh</span></p>
<p><span style="font-size: 14pt;">+ part 2: cấu h&igrave;nh static&nbsp;</span></p>
<p><span style="font-size: 14pt;">+ part 3 : kết nối với Database (RDS của AWS)&nbsp;</span></p>
<p><span style="font-size: 14pt;">----------------------------------------------------------</span></p>
<p style="text-align: center;"><strong><span style="font-size: 18pt;">Part 1 :&nbsp;</span></strong></p>
<p><strong><span style="font-size: 12pt;">sudo apt-get upgrade -y</span></strong><br /><strong><span style="font-size: 12pt;">sudo apt-get update -y</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">1. Deploy Python virtual env</span></strong></span><br /><strong><span style="font-size: 12pt;">sudo apt-get install python3-venv -y</span></strong></p>
<p><strong><span style="font-size: 12pt;">or&nbsp;<a href="https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-ubuntu-16-04#step-2-%E2%80%94-setting-up-a-virtual-environment">Venv</a>&nbsp;</span></strong><strong><span style="font-size: 12pt;">sudo apt-get install -y python3-venv</span></strong></p>
<p><br /><strong><span style="font-size: 12pt;">python3 -m venv&nbsp;venv</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">2. Activate env</span></strong></span><br /><strong><span style="font-size: 12pt;">source env/bin/activate</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">3. Install Django</span></strong></span><br /><strong><span style="font-size: 12pt;">pip3 install django</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">4. Clone the git project</span></strong></span><br /><strong><span style="font-size: 12pt;">git clone https://github.com/ShobiExplains/AwsDemo.git</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">5. Instal NGINX and GUNICORN</span></strong></span><br /><strong><span style="font-size: 12pt;">pip3 install gunicorn ## install without sudo..</span></strong><br /><strong><span style="font-size: 12pt;">sudo apt-get install nginx -y</span></strong><br /><strong><span style="font-size: 12pt;">pip install psycopg2-binary</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">6. Connect gunicorn</span></strong></span><br /><strong><span style="font-size: 12pt;">gunicorn --bind 0.0.0.0:8000 TestProject.wsgi:application ## similar to runserver</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">7. Install supervisor</span></strong></span><br /><strong><span style="font-size: 12pt;">sudo apt-get install -y supervisor ## This command holds the website after we logout</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">8. Config supervisor</span></strong></span><br /><strong><span style="font-size: 12pt;">cd /etc/supervisor/conf.d</span></strong><br /><strong><span style="font-size: 12pt;">sudo touch gunicorn.conf</span></strong></p>
<p><strong><span style="font-size: 12pt;">##In the file file do following###</span></strong><br /><strong><span style="font-size: 12pt;">[program:gunicorn]</span></strong><br /><strong><span style="font-size: 12pt;">directory=/home/ubuntu/AwsDemo</span></strong><br /><strong><span style="font-size: 12pt;">command=/home/ubuntu/env/bin/gunicorn --workers 3 --bind unix:/home/ubuntu/AwsDemo/app.sock TestProject.wsgi:application</span></strong><br /><strong><span style="font-size: 12pt;">autostart=true</span></strong><br /><strong><span style="font-size: 12pt;">autorestart=true</span></strong><br /><strong><span style="font-size: 12pt;">stderr_logfile=/var/log/gunicorn/gunicorn.err.log</span></strong><br /><strong><span style="font-size: 12pt;">stdout_logfile=/var/log/gunicorn/gunicorn.out.log</span></strong></p>
<p><strong><span style="font-size: 12pt;">[group:guni]</span></strong><br /><strong><span style="font-size: 12pt;">Program:gunicorn</span></strong><br /><strong><span style="font-size: 12pt;">####endfile####</span></strong></p>
<p><br /><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">9. Connect file to supervisor</span></strong></span><br /><strong><span style="font-size: 12pt;">sudo supervisorctl reread</span></strong></p>
<p><strong><span style="font-size: 12pt;">##if we receive the following error than we need to create the directory:</span></strong><br /><strong><span style="font-size: 12pt;">ERROR: CANT_REREAD: The directory named as part of the path /var/log/gunicorn/gunicorn.out.log does not exist. in section 'program:gunicorn' (file: '/etc/supervisor/conf.d/gunicorn.conf')</span></strong></p>
<p><strong><span style="font-size: 12pt;">sudo mkdir -p /var/log/gunicorn</span></strong><br /><strong><span style="font-size: 12pt;">sudo supervisorctl reread</span></strong><br /><strong><span style="font-size: 12pt;">sudo supervisorctl update</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">10. Check if gunicorn is running in background</span></strong></span><br /><strong><span style="font-size: 12pt;">sudo supervisorctl status</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">11. Create nginx configuration for django site</span></strong></span><br /><strong><span style="font-size: 12pt;">sudo nano/etc/nginx/sites-available/django.conf</span></strong></p>
<p><strong><span style="font-size: 12pt;">###inhalt##</span></strong><br /><strong><span style="font-size: 12pt;">server {</span></strong><br /><strong><span style="font-size: 12pt;"> listen 80;</span></strong><br /><strong><span style="font-size: 12pt;"> server_name 18.195.252.82;</span></strong></p>
<p><strong><span style="font-size: 12pt;">location / {</span></strong><br /><strong><span style="font-size: 12pt;"> include proxy_params;</span></strong><br /><strong><span style="font-size: 12pt;"> proxy_pass http://unix:/home/ubuntu/AwsDemo/app.sock;</span></strong><br /><strong><span style="font-size: 12pt;"> }</span></strong><br /><strong><span style="font-size: 12pt;">}</span></strong><br /><strong><span style="font-size: 12pt;">############</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">12. Test file configuration</span></strong></span></p>
<p><strong><span style="font-size: 12pt;">sudo ln /etc/nginx/sites-available/django.conf /etc/nginx/sites-enabled/</span></strong><br /><strong><span style="font-size: 12pt;">sudo nginx -t</span></strong></p>
<p><br /><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">13. restart NGINX server</span></strong></span><br /><strong><span style="font-size: 12pt;">sudo service nginx restart</span></strong></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;">&nbsp;NOTE :&nbsp;<span style="color: #000000;">Bước số 12 cần map từ avainable qua file enabled (nếu cấu h&igrave;nh lại x&oacute;a file b&ecirc;n enable rồi map lại) ,&nbsp;c&agrave;i supervisor để việc server bị kill sẽ đc restart lại&nbsp;</span></span></strong></span></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;"><span style="color: #000000;">---------------------------</span></span></strong></span></p>
<p><span style="color: #ff0000; font-size: 14pt;"><strong>SSL HTTPS&nbsp;</strong></span></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;"><span style="color: #000000;">B1 : cần tạo 1 domain v&agrave; map với ip (eg : asia.cloudns.net)</span></span></strong></span></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;"><span style="color: #000000;">B2: cấu h&igrave;nh file sites-available th&ecirc;m cổng 443&nbsp;</span></span></strong></span></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;"><span style="color: #000000;">c&oacute; thể cd v&agrave;o defauld để coppy nội dung&nbsp;</span></span></strong></span></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;"><span style="color: #000000;">---etc/nginx/sites-available/django.conf</span></span></strong></span></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;"><span style="color: #000000;">"""</span></span></strong></span></p>
<p>server {<br /> #listen 80;<br /> server_name <span style="color: #ff0000;"><strong>&lt;domain&gt;</strong></span>;<br /> # SSL configuration</p>
<p><br /> <strong>listen 443 ssl default_server;</strong><br /><strong> listen [::]:443 ssl default_server;</strong></p>
<p>location /staticfiles/ {<br /> autoindex on;<br /> alias /home/ubuntu/code/aws_drf_blog/staticfiles/;<br /> }<br /> location / {<br /> include proxy_params;<br /> proxy_pass http://unix:/home/ubuntu/code/aws_drf_blog/app.sock;<br /> }</p>
<p><strong>#ssl_certificate /etc/letsencrypt/live/aws.drfblog.cloudns.asia/fullchain.pem; # managed by Certbot</strong><br /><strong>#ssl_certificate_key /etc/letsencrypt/live/aws.drfblog.cloudns.asia/privkey.pem; # managed by Certbot</strong><br />}</p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;"><span style="color: #000000;">"""</span></span></strong></span></p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;"><span style="color: #000000;">B3: cần l&ecirc;n ec2 v&agrave;o "security Groub" để mở cồng https 433 ra nữa</span></span></strong></span></p>
<p>&nbsp;</p>
<p><span style="color: #ff0000;"><strong><span style="font-size: 12pt;"><span style="color: #000000;"><span style="color: #ff0000;">NOTE</span> : để c&oacute; được key private của ssl ta cần d&ugrave;ng Cerbot , Cerbot sẽ tự sinh cho ch&uacute;ng ta 2 file SSL nếu để n&oacute; tự cấu h&igrave;nh chi tiết tại&nbsp;<a href="https://viblo.asia/p/cai-dat-sslhttps-free-certbot-tren-aws-ec2-RQqKL9pOZ7z" target="_blank">viblo (tiếng việt)</a>&nbsp;hoặc&nbsp;<a href="https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx" target="_blank">certbot</a></span></span></strong></span></p>
<p>&nbsp;</p>
<p style="text-align: center;"><span style="color: #ff0000; font-size: 18pt;"><strong><span style="color: #000000;">------------------- PART 2-----------------</span></strong></span></p>
<p style="text-align: justify;"><span style="color: #ff0000; font-size: 12pt;"><span style="color: #000000;">Th&ecirc;m đoạn code sau v&agrave;o file sites-available/django.conf sau đ&oacute; restart lại nginx v&agrave; supervioser l&agrave; được</span></span></p>
<p style="text-align: justify;"><span style="color: #ff0000; font-size: 12pt;"><span style="color: #000000;">"""</span></span></p>
<p style="text-align: justify;"><span style="color: #ff0000; font-size: 12pt;"><span style="color: #000000;">location /staticfiles/ {<br />autoindex on;<br />alias /home/ubuntu/code/aws_drf_blog/staticfiles/;<br />}</span></span></p>
<p style="text-align: justify;"><span style="color: #ff0000; font-size: 12pt;"><span style="color: #000000;">"""</span></span></p>
<p style="text-align: justify;"><span style="color: #ff0000; font-size: 12pt;"><span style="color: #000000;"><strong>restart</strong> :&nbsp;</span></span><span style="color: #ff0000; font-size: 12pt;"><span style="color: #000000;"><br />sudo supervisorctl reload</span></span></p>
<p style="text-align: justify;"><span style="color: #ff0000; font-size: 12pt;"><span style="color: #000000;">sudo systemctl reload nginx</span></span></p>
<p style="text-align: justify;"><span style="color: #ff0000; font-size: 12pt;"><span style="color: #000000;">sudo nginx -t&nbsp;</span></span></p>
<p style="text-align: justify;">&nbsp;</p>
<p>&nbsp;</p>