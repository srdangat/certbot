apt update && apt install -y nginx
sudo apt update && suo apt install certbot python3-certbot-nginx

sudo mkdir -p /var/www/devopsblog.online/html
sudo chown -R $USER:$USER /var/www/devopsblog.online/html
sudo chmod -R 755 /var/www/devopsblog.online

sudo vi /var/www/devopsblog.online/html/index.html
<html>
<head>
    <title>Welcome to devopsblog.online!</title>
</head>
<body>
    <h1>Success! The devopsblog.online server block is working!</h1>
</body>
</html>


sudo vi /etc/nginx/sites-available/devopsblog.online
server {
    listen 80;
    listen [::]:80;

    root /var/www/devopsblog.online/html;
    index index.html index.htm index.nginx-debian.html;

    # Define the server names
    server_name devopsblog.online
                 failover.devopsblog.online 
                 latency.devopsblog.online 
                 weighted.devopsblog.online 
                 geolocation.devopsblog.online 
                 www.devopsblog.online;

    # Main location block
    location / {
        try_files $uri $uri/ =404;
    }
}


sudo ln -s /etc/nginx/sites-available/devopsblog.online /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx


sudo certbot certonly \
--agree-tos \
--email sanket.r.dangat@gmail.com \
--manual \
--preferred-challenge=dns \
-d "*.devopsblog.online" \
--server https://acme-v02.api.letsencrypt.org/directory



certbot --nginx
