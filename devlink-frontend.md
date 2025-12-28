# DevLink - Development Notes

## Frontend Setup

- Create a Vite + React application
- Remove unnecessary code and create a Hello World app
- Install tailwind CSS
- Install Daisy UI
- Add NavBar component to App.jsx
- Create a NavBar.jsx separate file Component file
- Install react router dom
- Create BrowserRouter > Routes > Route=/ Body > RouteChildren
- Create an Outlet in your Body Component
- Create a Footer
- Create a Login Page
- Install axios
- CORS - install cors in backend => add middleware with configuration: origin, credentials: true
- Whenever making an API call pass axios => { withCredentials: true }
- Install Redux Toolkit
- Install react-redux + @reduxjs/toolkit
- configureStore => Provider => createSlice => add reducer to store
- Add redux devtools in chrome
- Login and verify data is coming properly in the store
- NavBar should update as soon as user logs in
- Refactor code to add constants file + create a components folder
- Route protection - should not be able to access routes without login
- If token is not present, redirect user to login page
- Logout Feature
- Get the feed and add feed into the store
- Build the user card on the feed
- Edit Profile Feature
- Show toast message on save of Profile
- New Page - See all my connections
- New Page - See all my Connection Requests
- Feature - Accept/Reject Connection Request
- Send/Ignore the user on feed
- SignUp New User
- E2E Testing

## Route Structure
```
Body
  ├── NavBar
  ├── Routes
  │   ├── /feed (Feed)
  │   ├── /login (Login)
  │   ├── /connections (Connections)
  │   └── /profile (Profile)
  └── Footer
```

---

## Deployment Guide

### AWS EC2 Setup
- Sign up on AWS
- Launch EC2 instance
- Set permissions on key file: `chmod 400 <your-key>.pem`
- SSH into instance: `ssh -i "<your-key>.pem" ubuntu@<your-ec2-ip>`
- Install Node.js version 16.17.0 or higher
- Clone repository

### Frontend Deployment
- `npm install` - install dependencies
- `npm run build` - create production build
- `sudo apt update`
- `sudo apt install nginx`
- `sudo systemctl enable nginx`
- Copy build files: `sudo cp -r dist/* /var/www/html/`
- Enable port 80 on your EC2 instance

### Backend Deployment
- Update DB password in environment variables
- Allow EC2 instance public IP on MongoDB Atlas
- Install PM2 globally: `npm install pm2 -g`
- Start application: `pm2 start npm --name "your-app-name" -- start`
- Useful PM2 commands:
  - `pm2 logs` - view logs
  - `pm2 list` - list all processes
  - `pm2 flush <name>` - flush logs
  - `pm2 stop <name>` - stop process
  - `pm2 delete <name>` - delete process
- Configure Nginx: `/etc/nginx/sites-available/default`

### Nginx Configuration Example

```nginx
server {
    listen 80;
    server_name your-domain.com;

    # Frontend - serve static files
    location / {
        root /var/www/html;
        try_files $uri $uri/ /index.html;
    }

    # Backend API - proxy to Node.js
    location /api/ {
        proxy_pass http://localhost:7777/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

---

## Custom Domain Setup

- Purchase domain from registrar (GoDaddy, Namecheap, etc.)
- Sign up on Cloudflare
- Add your domain to Cloudflare
- Update nameservers on your domain registrar to point to Cloudflare
- Wait for nameserver propagation (~15 minutes to 48 hours)
- Add DNS record in Cloudflare:
  - Type: A
  - Name: @ (or subdomain)
  - Content: Your EC2 public IP
  - Proxy status: Proxied (for SSL)
- Enable SSL/TLS in Cloudflare (Full or Full Strict)

---

## AWS SES Email Setup

- Create an IAM user in AWS Console
- Attach policy: AmazonSESFullAccess
- Go to Amazon SES Console
- Create and verify an identity (domain or email)
- Verify your domain name (add DNS records)
- Verify email address identity
- Install AWS SDK v3: `npm install @aws-sdk/client-ses`
- Code examples: https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ses
- Setup SESClient with credentials
- Create access credentials in IAM under Security Credentials
- Add credentials to .env file:
  ```
  AWS_ACCESS_KEY_ID=your_key
  AWS_SECRET_ACCESS_KEY=your_secret
  AWS_REGION=your_region
  ```
- Write code for SESClient
- Write code for sending emails
- Make email function dynamic with parameters

---

## Scheduling Cron Jobs

- Install node-cron: `npm install node-cron`
- Learn cron expressions: https://crontab.guru
- Install date-fns: `npm install date-fns`
- Schedule jobs for:
  - Finding unique email IDs with connection requests from previous day
  - Sending daily digest emails
- Explore queue mechanisms for bulk emails:
  - bee-queue
  - bull
- Amazon SES bulk email capabilities
- Make sendEmail function dynamic

---

## Real-Time Chat (Socket.io)

- Build UI for chat window at `/chat/:targetUserId`
- Install Socket.io in backend: `npm install socket.io`
- Install Socket.io client in frontend: `npm install socket.io-client`
- Setup Socket.io server
- Implement real-time messaging
- Handle connection/disconnection events
- Emit and listen to custom events

---

## Environment Variables Template

```env
# Database
DATABASE_URL=your_mongodb_connection_string

# JWT
JWT_SECRET=your_jwt_secret

# Server
PORT=7777
NODE_ENV=production

# AWS SES
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_REGION=your_region
SENDER_EMAIL=your_verified_email

# Frontend URL (for CORS)
FRONTEND_URL=https://your-domain.com
```

---

## Security Checklist

- [ ] Never commit .env files
- [ ] Never expose API keys or secrets
- [ ] Use environment variables for all sensitive data
- [ ] Enable CORS with specific origins only
- [ ] Use HTTPS in production
- [ ] Implement rate limiting
- [ ] Validate all user inputs
- [ ] Use secure password hashing (bcrypt)
- [ ] Set secure cookie flags (httpOnly, secure, sameSite)
- [ ] Keep dependencies updated
- [ ] Use PM2 cluster mode for better performance
