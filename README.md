# AI Content Creator Agent for n8n

An automated content creation workflow that generates SEO-friendly blog posts, creates accompanying images, and publishes content to WordPress and LinkedIn simultaneously using Google's Gemini AI.

## 🚀 Features

- **AI-Powered Content Generation**: Creates high-quality, SEO-optimized blog content using Google Gemini
- **Automated Image Creation**: Generates professional 1:1 square blog images with AI
- **Multi-Platform Publishing**: Automatically publishes to WordPress and LinkedIn
- **Cloud Storage Integration**: Uploads generated images to Google Drive
- **Memory Management**: Prevents content repetition with intelligent topic tracking
- **Customizable Categories**: Focuses on technology topics (AI, Web, Mobile, Trading Bots)

## 📋 Prerequisites

- Docker and Docker Compose installed
- Domain name with DNS configured (for SSL setup)
- WordPress website with admin access
- LinkedIn business/organization page
- Google account with API access
- Google Gemini API key

## 🔧 API Setup Instructions

### 1. Google Gemini API

1. Visit [Google AI Studio](https://aistudio.google.com/api-keys)
2. Sign in with your Google account
3. Click "Create API Key"
4. Copy the generated API key
5. In n8n, add the credential as "Google PaLM API" with your API key

### 2. LinkedIn API

**Prerequisites**: You must have a LinkedIn Company/Organization page

1. Go to [LinkedIn Developers](https://www.linkedin.com/developers/apps)
2. Click "Create App"
3. Fill in the required information:
   - App name: Your app name
   - LinkedIn Page: Select your company page
   - App logo: Upload a logo (optional)
4. In the "Products" tab, request access to:
   - Share on LinkedIn
   - Sign In with LinkedIn
5. In the "Auth" tab, add your redirect URLs
6. Copy the Client ID and Client Secret
7. In n8n, create a "LinkedIn OAuth2 API" credential with these details

### 3. WordPress API

1. Log into your WordPress admin dashboard
2. Go to Users → Add New
3. Create a new user with Administrator role
4. Generate an Application Password:
   - Go to Users → Your Profile
   - Scroll to "Application Passwords"
   - Add new application password
   - Copy the generated password
5. In n8n, create a "WordPress API" credential with:
   - URL: Your WordPress site URL
   - Username: The username you created
   - Password: The application password

### 4. Google Drive API

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Enable the Google Drive API
4. Create credentials (OAuth 2.0 Client ID)
5. Download the JSON credentials file
6. In n8n, create a "Google Drive OAuth2 API" credential
7. Follow the OAuth flow to authorize access

## Docker & n8n Setup

### 1. Docker Installation

#### Ubuntu/Debian

```bash
# Update package index
sudo apt update

# Install required packages
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Add user to docker group
sudo usermod -aG docker $USER
```

#### CentOS/RHEL

```bash
# Install required packages
sudo yum install -y yum-utils

# Add Docker repository
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# Install Docker
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Add user to docker group
sudo usermod -aG docker $USER
```

#### Windows

1. Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop)
2. Run the installer and follow the setup wizard
3. Restart your computer when prompted
4. Launch Docker Desktop and complete the initial setup

#### macOS

1. Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop)
2. Drag Docker to Applications folder
3. Launch Docker from Applications
4. Complete the initial setup process

### 2. n8n Docker Setup

#### Basic n8n Installation

```bash
# Create n8n directory
mkdir n8n-setup && cd n8n-setup

# Create docker-compose.yml
cat > docker-compose.yml << EOF
version: '3.8'

services:
  n8n:
    image: docker.n8n.io/n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=your_secure_password
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - WEBHOOK_URL=http://localhost:5678/
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  n8n_data:
EOF

# Start n8n
docker-compose up -d
```

#### Production Setup with Database

```bash
# Create production docker-compose.yml
cat > docker-compose.yml << EOF
version: '3.8'

services:
  postgres:
    image: postgres:13
    restart: always
    environment:
      - POSTGRES_USER=n8n
      - POSTGRES_PASSWORD=your_db_password
      - POSTGRES_DB=n8n
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -h localhost -U n8n']
      interval: 5s
      timeout: 5s
      retries: 10

  n8n:
    image: docker.n8n.io/n8nio/n8n
    restart: always
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=your_db_password
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=your_secure_password
      - N8N_HOST=your-domain.com
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - WEBHOOK_URL=https://your-domain.com/
      - GENERIC_TIMEZONE=UTC
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      postgres:
        condition: service_healthy

volumes:
  postgres_data:
  n8n_data:
EOF
```

### 3. Nginx & SSL Configuration

#### Install Nginx

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install nginx

# CentOS/RHEL
sudo yum install nginx
```

#### Install Certbot for SSL

```bash
# Ubuntu/Debian
sudo apt install certbot python3-certbot-nginx

# CentOS/RHEL
sudo yum install certbot python3-certbot-nginx
```

#### Nginx Configuration

```bash
# Create nginx configuration
sudo tee /etc/nginx/sites-available/n8n << EOF
server {
    listen 80;
    server_name your-domain.com;
    return 301 https://\$server_name\$request_uri;
}

server {
    listen 443 ssl http2;
    server_name your-domain.com;

    # SSL Configuration (will be added by certbot)

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
    add_header Content-Security-Policy "default-src 'self' http: https: data: blob: 'unsafe-inline'" always;

    # Proxy settings
    location / {
        proxy_pass http://localhost:5678;
        proxy_http_version 1.1;
        proxy_set_header Upgrade \$http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host \$host;
        proxy_set_header X-Real-IP \$remote_addr;
        proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto \$scheme;
        proxy_cache_bypass \$http_upgrade;

        # Increase timeouts for long-running workflows
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
    }

    # Webhook endpoint optimization
    location /webhook/ {
        proxy_pass http://localhost:5678/webhook/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade \$http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host \$host;
        proxy_set_header X-Real-IP \$remote_addr;
        proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto \$scheme;
        proxy_cache_bypass \$http_upgrade;
    }
}
EOF

# Enable the site
sudo ln -s /etc/nginx/sites-available/n8n /etc/nginx/sites-enabled/

# Test nginx configuration
sudo nginx -t

# Restart nginx
sudo systemctl restart nginx
```

#### Obtain SSL Certificate

```bash
# Get SSL certificate from Let's Encrypt
sudo certbot --nginx -d your-domain.com

# Test automatic renewal
sudo certbot renew --dry-run

# Set up automatic renewal (add to crontab)
echo "0 12 * * * /usr/bin/certbot renew --quiet" | sudo crontab -
```

#### Firewall Configuration

```bash
# Ubuntu/Debian (UFW)
sudo ufw allow 'Nginx Full'
sudo ufw allow ssh
sudo ufw enable

# CentOS/RHEL (firewalld)
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

### 4. Complete Setup Script

Create an automated setup script:

```bash
#!/bin/bash
# n8n-setup.sh

set -e

echo "🚀 Starting n8n production setup..."

# Variables
DOMAIN="your-domain.com"
DB_PASSWORD=$(openssl rand -base64 32)
N8N_PASSWORD=$(openssl rand -base64 16)

# Create directory structure
mkdir -p /opt/n8n
cd /opt/n8n

# Create docker-compose.yml
cat > docker-compose.yml << EOF
version: '3.8'

services:
  postgres:
    image: postgres:13
    restart: always
    environment:
      - POSTGRES_USER=n8n
      - POSTGRES_PASSWORD=${DB_PASSWORD}
      - POSTGRES_DB=n8n
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -h localhost -U n8n']
      interval: 5s
      timeout: 5s
      retries: 10

  n8n:
    image: docker.n8n.io/n8nio/n8n
    restart: always
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=${DB_PASSWORD}
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=${N8N_PASSWORD}
      - N8N_HOST=${DOMAIN}
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - WEBHOOK_URL=https://${DOMAIN}/
      - GENERIC_TIMEZONE=UTC
    ports:
      - "127.0.0.1:5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      postgres:
        condition: service_healthy

volumes:
  postgres_data:
  n8n_data:
EOF

# Start services
docker-compose up -d

# Save credentials
cat > credentials.txt << EOF
n8n Login Credentials:
Username: admin
Password: ${N8N_PASSWORD}
URL: https://${DOMAIN}

Database Password: ${DB_PASSWORD}
EOF

echo "✅ Setup complete!"
echo "📋 Credentials saved to credentials.txt"
echo "🌐 Access n8n at: https://${DOMAIN}"
```

## 📥 Installation

1. **Import the Workflow**:

   - Download the `AI-Agent_Content-Creator_Public.json` file
   - In n8n, go to Workflows → Import from File
   - Select the downloaded JSON file

2. **Configure Credentials**:

   - Update all credential references in the workflow nodes
   - Test each API connection to ensure they're working

3. **Customize Settings**:
   - Modify WordPress categories in the "Create Blog" node
   - Adjust content length limits if needed
   - Update LinkedIn posting preferences

## 🎯 How It Works

### Workflow Process

1. **Manual Trigger**: Start the workflow with a click
2. **Content Retrieval**: Fetches the latest WordPress post for context
3. **AI Content Generation**:
   - Uses Gemini AI to create original blog content
   - Generates SEO-friendly titles and descriptions
   - Creates image generation prompts
4. **Image Creation**: Generates professional blog images using Gemini
5. **Content Processing**: Cleans and formats the generated content
6. **Multi-Platform Publishing**:
   - Creates draft blog post in WordPress
   - Publishes post to LinkedIn
   - Uploads generated image to Google Drive

### Content Categories

The AI focuses on technology-related topics:

- AI Agents and Automation
- Desktop Application Development
- Mobile App Development
- Web Development
- Trading Bot Development
- Software Services

### Memory System

The workflow includes intelligent memory management that:

- Tracks the last 3 generated topics
- Prevents repetitive content creation
- Ensures content variety and freshness

## 🛠️ Customization Options

### Content Modification

- **Language**: Currently set to English (previously Turkish)
- **Length**: Maximum 2500 characters (adjustable)
- **Style**: Professional, SEO-optimized content
- **Topics**: Technology-focused with expandable categories

### Publishing Settings

- **WordPress**: Creates posts as drafts by default
- **LinkedIn**: Posts publicly with attribution
- **Google Drive**: Stores images in root folder (customizable)

### Image Generation

- **Format**: 1:1 square images
- **Style**: Modern, minimal, professional
- **Naming**: Automatic hyphenated titles based on content

## Docker & Infrastructure Troubleshooting

### Docker Issues

1. **Docker Service Not Running**:

   ```bash
   # Check Docker status
   sudo systemctl status docker

   # Start Docker if stopped
   sudo systemctl start docker

   # Enable Docker to start on boot
   sudo systemctl enable docker
   ```

2. **Permission Denied Errors**:

   ```bash
   # Add user to docker group
   sudo usermod -aG docker $USER

   # Log out and back in, or run:
   newgrp docker
   ```

3. **Container Won't Start**:

   ```bash
   # Check container logs
   docker-compose logs n8n

   # Check container status
   docker-compose ps

   # Restart containers
   docker-compose restart
   ```

### Nginx Issues

1. **Configuration Test Failed**:

   ```bash
   # Test nginx configuration
   sudo nginx -t

   # Check nginx status
   sudo systemctl status nginx

   # View nginx error logs
   sudo tail -f /var/log/nginx/error.log
   ```

2. **SSL Certificate Issues**:

   ```bash
   # Check certificate status
   sudo certbot certificates

   # Renew certificates manually
   sudo certbot renew

   # Test renewal process
   sudo certbot renew --dry-run
   ```

3. **Port Conflicts**:

   ```bash
   # Check what's using port 80/443
   sudo netstat -tlnp | grep :80
   sudo netstat -tlnp | grep :443

   # Kill conflicting processes if needed
   sudo systemctl stop apache2  # if Apache is running
   ```

### n8n Specific Issues

1. **Database Connection Failed**:

   ```bash
   # Check PostgreSQL container
   docker-compose logs postgres

   # Connect to database manually
   docker-compose exec postgres psql -U n8n -d n8n
   ```

2. **Webhook URLs Not Working**:

   - Ensure `WEBHOOK_URL` environment variable is set correctly
   - Check firewall settings
   - Verify domain DNS configuration

3. **Memory Issues**:
   ```bash
   # Increase Docker memory limits in docker-compose.yml
   services:
     n8n:
       deploy:
         resources:
           limits:
             memory: 2G
           reservations:
             memory: 1G
   ```

### Monitoring Commands

```bash
# Check system resources
htop
df -h
free -h

# Monitor Docker containers
docker stats

# Check n8n logs
docker-compose logs -f n8n

# Check nginx access logs
sudo tail -f /var/log/nginx/access.log

# Check SSL certificate expiry
echo | openssl s_client -servername your-domain.com -connect your-domain.com:443 2>/dev/null | openssl x509 -noout -dates
```

## 🔍 Troubleshooting

### Common Issues

1. **API Authentication Errors**:

   - Verify all credentials are correctly configured
   - Check API key validity and permissions
   - Ensure OAuth tokens haven't expired

2. **Content Generation Failures**:

   - Check Gemini API quota and limits
   - Verify the system message formatting
   - Review memory node configuration

3. **Publishing Errors**:
   - Confirm WordPress user permissions
   - Check LinkedIn page access rights
   - Verify Google Drive folder permissions

### Testing Individual Components

- Test each API connection separately
- Use n8n's manual execution to debug specific nodes
- Check the execution log for detailed error messages

## 📊 Monitoring and Analytics

- Monitor WordPress post performance
- Track LinkedIn engagement metrics
- Review Google Drive storage usage
- Analyze content generation patterns

## 🤝 Contributing

Feel free to:

- Report issues or bugs
- Suggest new features
- Submit improvements
- Share your customizations

## 📄 License

This workflow is provided as-is for educational and commercial use. Please ensure compliance with all API terms of service.

## 🆘 Support

If you encounter setup difficulties or need assistance:

- Check the troubleshooting section above
- Review n8n's official documentation
- Consider the API provider documentation links
- Feel free to reach out for additional support

---

**Note**: This workflow automates content creation but always review generated content before publishing to ensure it meets your quality standards and brand guidelines.
