# OmniRoute Deployment Guide

## What is OmniRoute?

- **Full-stack Node.js/TypeScript application**
- **Next.js frontend** (React)
- **Express/custom backend**
- **Runs on: Linux, macOS, Windows, Docker**

## Hosting Options

### 1. **Vercel** (Easiest for Next.js)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
cd /home/wym/OmniRoute
vercel

# Auto-deploys from git, custom domain support
# Free tier available
```

**Pros:** Optimized for Next.js, auto-scaling, CDN included
**Cons:** Limited backend capabilities on free tier

---

### 2. **Docker + Cloud Run / Heroku / Railway**

**Create Dockerfile:**

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

**Deploy to Railway (easiest):**

```bash
npm install -g railway
railway login
railway up
```

**Or Heroku:**

```bash
heroku login
heroku create your-app-name
git push heroku main
```

---

### 3. **VPS** (DigitalOcean, Linode, AWS EC2)

**Setup on Ubuntu/Debian:**

```bash
# SSH into server
ssh root@your-server-ip

# Install Node
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Clone repo
git clone https://github.com/yourname/omniroute.git
cd omniroute

# Install & build
npm install
npm run build

# Run with PM2 (production process manager)
npm install -g pm2
pm2 start npm --name "omniroute" -- start
pm2 save
pm2 startup
```

**Setup Nginx reverse proxy:**

```nginx
server {
    listen 80;
    server_name your-domain.com;

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

**Enable HTTPS (Let's Encrypt):**

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

---

### 4. **AWS (Full Control)**

**Using Elastic Beanstalk:**

```bash
npm install -g @aws-amplify/cli
eb init
eb create omniroute-env
eb deploy
```

**Or EC2 + Docker:**

```bash
# Launch EC2 instance
# SSH in
docker build -t omniroute .
docker run -d -p 80:3000 omniroute
```

---

### 5. **Fly.io** (Great for full-stack)

```bash
npm install -g flyctl
fly auth login
fly launch
fly deploy
```

---

## Environment Variables

Create `.env.production`:

```bash
NODE_ENV=production
DATABASE_URL=your-database-url
API_KEY=your-api-key
# Add all required env vars
```

---

## Recommended Setup

**Best for beginners:** Railway or Vercel
**Best for control:** VPS (DigitalOcean $5-6/month)
**Best for scale:** AWS / GCP
**Best for testing:** Docker locally first

---

## Performance Tips

1. **Enable compression:**

   ```bash
   npm install compression
   ```

2. **Use PM2 clustering:**

   ```bash
   pm2 start npm --name "omniroute" -i max -- start
   ```

3. **Setup monitoring:**

   ```bash
   pm2 install pm2-auto-pull
   pm2 install pm2-logrotate
   ```

4. **Database optimization:**
   - Use connection pooling
   - Add indexes
   - Cache frequently accessed data

---

## Security Checklist

- [ ] Enable HTTPS/SSL
- [ ] Set secure environment variables
- [ ] Enable CORS properly
- [ ] Rate limiting
- [ ] DDoS protection (Cloudflare)
- [ ] Regular backups
- [ ] Security headers
- [ ] Input validation
