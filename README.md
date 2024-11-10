# AWS-EC2-CREATION

Server setup steps on AWS EC2 Machine:

1.  Connect to ec2 machine - `link`.
2.  Clone the repo - `git clone <repoURL>`.
3.  Configure git credentails.
4.  Enter the cloned repo - `cd name`.
5.  Check if docker is installed in the machine, if not install the docker `yum install docker`.
6.  Configure .env.local file with appropriate creds needed based on env and RUN `docker-compose up --build -d`.
7.  Above command will spin up `project-api` and `project-postgres` docker containers.
8.  Since we are not using load balancer, we need to spin up ngnix for routing https requests.
9.  RUN `yum install ngnix` to install ngnix.
10. RUN `cd /etc/nginx/conf.d/`.
11. Create file api-test.conf.
12. Copy below content into this file.
`server {
    listen 443 ssl;
    server_name qa-api.vivaha.info; # make sure dns is configure with this url

    # certificate
    ssl_certificate ; # managed by Certbot
    ssl_certificate_key ; # managed by Certbot


    location / {
        proxy_pass http://127.0.0.1:port numr;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-Proto https;
    }
}`
13. Configure SSL to communicate ngnix via https, for this we are using `Certbot`.
14. RUN `sudo python3 -m venv /opt/certbot/`.
15. RUN `sudo /opt/certbot/bin/pip install --upgrade pip`.
16. RUN `sudo /opt/certbot/bin/pip install certbot certbot-nginx`.
17. RUN `sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot`.
18. RUN `sudo certbot --nginx`.
19. Configure crontab to renew certificate automatically.
20. RUN `echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null`
21. RUN `sudo nginx -t`.
22. RUN `sudo systemctl start nginx`.
23. RUN `sudo systemctl enable nginx`.
24. RUN `sudo systemctl status nginx`.
25. RUN `sudo systemctl reload nginx`.




# Note: Reference Link: https://dev.to/0xfedev/how-to-install-nginx-as-reverse-proxy-and-configure-certbot-on-amazon-linux-2023-2cc9
