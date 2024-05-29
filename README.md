
# Hi, I'm Briann! 👋, i'll take you through Nginx Setup on Kali Linux.

So you've built a fantastic Node.js application and want to deploy it to the world! But what if you don't have a fancy domain name yet? No worries! This guide will show you how to set up your Node.js application on your kali server using Nginx as a reverse proxy, all with just your server's public IP address.

### Getting Ready

Before we dive in, make sure you have the following:

- A functional Node.js application on your kali server.
- Access to your server via SSH.
- A public IP address for your server.

### 1. Node.js Application Setup

First things first, ensure your Node.js application is ready to go. Navigate to the directory containing your application's files on your server using the cd command in your terminal.

### 2. Install PM2 (Process Manager)

PM2, a process manager for Node.js applications, will help us keep our application running smoothly. If you haven't installed it yet, use the following command in your terminal:

```bash
npm install pm2 -g

```

### 3. Start Your Application with PM2

Now, let's use PM2 to launch your Node.js application. Replace `app.js` with the actual filename of your application's entry point (the file that starts your application):

```bash
pm2 start app.js

```

### 4. Configure PM2 for Automatic Startup

We want our Node.js application to start automatically whenever the server boots up. Use the following command to configure PM2 for startup:

```bash
pm2 startup

```
PM2 will provide instructions on setting up startup scripts. Follow those instructions to ensure your application starts on its own.

### 5. Install Nginx (if not already installed)

Nginx, a powerful web server, will act as a reverse proxy for our Node.js application. If it's not already installed on your server, use this command:

```bash
sudo apt update
sudo apt install nginx
```

### 6. Configure Nginx

Now, it's time to configure Nginx to route traffic to our Node.js application. Here's what we need to do:

- Create a new configuration file for your application. Use `sudo nano /etc/nginx/sites-available/nodejsappname` in your terminal.

- Paste the following configuration into the file, replacing `your_public_ip` with your server's public IP address and assuming your application runs on port 3000:

```javascript
server {
  listen 80;
  server_name your_public_ip;

  location / {
    proxy_pass http://localhost:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
}
```

#### Explanation:

- `listen 80` : This line tells Nginx to listen for incoming traffic on port 80 (the standard HTTP port).
- `server_name your_public_ip`: This line specifies the server name, which in our case is your server's public IP address.
- `location /`: This block defines how Nginx handles requests for all URLs (the `/`).
- `proxy_pass http://localhost:3000`: This line tells Nginx to forward any incoming requests to your Node.js application running on `localhost` (your server) on port 3000.

### 7. Test Nginx Configuration (Important!)

Before enabling the configuration, it's crucial to test it for syntax errors. Use the following command:

```bash
sudo nginx -t
```
This command will check the configuration file for any errors. If there are no errors, you'll see a message like "nginx: configuration file is OK". If there are errors, you'll need to fix them before proceeding.

### 8. Enable the Configuration
Once you've saved the configuration file and confirmed it's error-free, create a symbolic link to enable it and reload Nginx using the following commands:

```bash
sudo ln -s /etc/nginx/sites-available/nodejsappname /etc/nginx/sites-enabled/
sudo systemctl reload nginx
```

### 9. Configure Firewall (if applicable)

If you're using a firewall like UFW (Uncomplicated Firewall), you'll need to allow traffic on port 80 (HTTP) to access your application. Use the following command:
```bash
sudo ufw allow 80/tcp
```
Make sure to adjust the command if you're using a different firewall.

### 10. Test Your Setup

Now comes the exciting part! Open a web browser and navigate to your server's public IP address (e.g., `http://your_public_ip`). If everything is set up correctly, you should see your Node.js application running.

### 11. Restarting Nginx Server
There might be situations where you make changes to your Nginx configuration file and need to restart Nginx for those changes to take effect. Here's how to do that:

~~~bash
sudo systemctl restart nginx
~~~

This command gracefully restarts Nginx, stopping the current worker processes and then starting new ones with the updated configuration.

`Congratulations!` You've successfully deployed your Node.js application on your Ubuntu server using Nginx as a reverse proxy, even without a domain name. Now your application is accessible to the world through your server's public IP address.



## Authors

- [@Briann_bn](https://www.github.com/addyxx-h4ck1)


## Feedback

If you have any feedback, please reach out to us at business.briann@gmail.com


## License

[MIT](https://choosealicense.com/licenses/mit/)

