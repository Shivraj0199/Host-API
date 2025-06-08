## **🛠️ STEP-BY-STEP TO HOST NODE.JS API ON LINUX EC2**
 
**Step 1: Launch EC2 (Ubuntu OS)**

* Go to AWS EC2 Console
* Click Launch Instance
* Choose:
  1) Ubuntu 22.04
  2) t2.micro (Free Tier eligible)

* Create a key pair (.pem) if you don’t have one
    
* In Security Group, allow:
  1) Port 22 – SSH
  2) Port 80 – HTTP
  3) Port 3000 – (Node.js API default port)

**Step 2: SSH into EC2 Instance**

`ssh -i your-key.pem ubuntu@<EC2-PUBLIC-IP>`

**Step 3: Install Node.js and Git**
  1) sudo apt update
  2) sudo apt install nodejs npm git -y

**Step 4: Clone API from Github**
  1) git clone https://github.com/Shivraj0199/Node-app.git
  2) cd your-api
  3) npm install

**Step 5: Run the API**

`node app.js`

**Step 6: Test the API**

`http://<your-ec2-public-ip>:3000`

**Step 7: Use NGINX as Reverse Proxy**

  1) sudo apt install nginx -y
  2) sudo nano /etc/nginx/sites-available/default
  3) Replace with 

  ```
server {
    listen 80;
    server_name <your-ec2-ip>;

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

   4) sudo systemctl restart nginx
   5) http://<your-ec2-ip>

**Step 7 : Use PM2 to Keep App Running (Process Manager)**

   1) sudo npm install -g pm2
   2) pm2 start app.js
   3) pm2 save
   4) pm2 startup

 **Step 8: Add a Custom Domain (Mapping public IP to your Domain Name)**

   * Go to your domain registrar (GoDaddy, Namecheap, Hostinger etc.)
   * Add a DNS A record:

        1) Host: @ or api
        2) Value: <your-ec2-ip>

   * Wait 5–30 minutes for DNS to propagate




  
