# 📂 FileHub

**FileHub** is a self-hosted file and folder management service.  
Mount your directories, manage users and groups, and access everything via a clean web UI.  

Runs in Docker with minimal setup required.

---

## 📖 Source Code Availability

FileHub is licensed under GPL-3.0. This means the complete source code must always be available to users.  

- 🔹 **Inside Docker image**: Every published image on Docker Hub includes the full source tree under `/app`. You can inspect or extract it by running:  
  ```bash
  docker run --rm -it patgames36/filehub:latest sh
  cd /app && ls -la
  ```
- 🔹 **Future GitHub releases**: When version **v1.0.0** is released, the entire source code will also be mirrored and maintained on GitHub at:  
  [https://github.com/CipherWorkZ/filehub](https://github.com/CipherWorkZ/filehub)

This ensures compliance with GPL-3.0 while making the project easy to inspect, fork, and contribute to.

---

## 🚀 Quick Start

1. **Clone or create a working folder** for FileHub. Inside it, create a file named **`.env`** using the template below.

2. Create a **`docker-compose.yaml`** file:

   ```yaml
   version: "3.9"

   services:
     filehub:
       image: patgames36/filehub:latest
       container_name: filehub
       restart: unless-stopped
       ports:
         - "8008:8000"   # host:container
       volumes:
         - ./db:/app/db
         - /mnt/deven:/mnt/deven
         - /mnt/schme:/mnt/schme
         - /plex1:/plex1
       env_file:
         - .env
   ```

3. Start the service:

   ```bash
   docker compose up -d
   ```

4. Open your browser at:

   ```
   http://localhost:8008
   ```

   and log in with the admin credentials from your `.env`.

---

## ⚙️ Configuration

### Using `.env`

FileHub loads configuration from environment variables. It is strongly recommended to put them in a `.env` file.  

Here is a **complete `.env` template** you can start from:

<details>
<summary>.env example</summary>

```dotenv
########################################################
# Database
########################################################
DB_BACKEND=sqlite
BROWSER_ROOT=/
DB_URL=sqlite:///./db/storage.db

# For Postgres (future)
# DB_BACKEND=postgres
# DB_URL=postgresql://user:pass@localhost:5432/storage_db

########################################################
# Admin Bootstrap (first-time seed user)
########################################################
ADMIN_USERNAME=admin
ADMIN_EMAIL=admin@local
ADMIN_PASSWORD=SuperSecret123

########################################################
# General Settings
########################################################
DEFAULT_TIMEZONE=UTC
DEFAULT_LANGUAGE=en
GENERAL_BRAND_NAME=FileHub
GENERAL_SESSION_TIMEOUT_MINUTES=30

########################################################
# Authentication & Security
########################################################
AUTH_OIDC_ENABLED=true
AUTH_OIDC_ISSUER=https://auth.example.com
AUTH_OIDC_CLIENT_ID=folder-service
AUTH_OIDC_CLIENT_SECRET=changeme
AUTH_OIDC_GROUPS_CLAIM=groups
AUTH_OIDC_USERNAME_CLAIM=preferred_username
AUTH_OIDC_AUTO_FLOW=false

AUTH_LOCAL_ENABLED=true
AUTH_PASSWORD_POLICY_MIN_LENGTH=8
AUTH_PASSWORD_POLICY_REQUIRE_SPECIAL=true
AUTH_LOGIN_ATTEMPTS_MAX=5
AUTH_LOGIN_ATTEMPTS_WINDOW_MINUTES=15

########################################################
# Storage / File Management
########################################################
STORAGE_DEFAULT_QUOTA_MB=10240
STORAGE_MAX_UPLOAD_MB=512
STORAGE_ALLOWED_EXTENSIONS=pdf,docx,jpg,png
STORAGE_RECYCLE_BIN_ENABLED=true
STORAGE_RECYCLE_BIN_RETENTION_DAYS=30

########################################################
# Audit / Logging
########################################################
LOGGING_LEVEL=INFO
LOGGING_RETENTION_DAYS=90
LOGGING_EXTERNAL_SYSLOG=
LOGGING_AUDIT_ENABLED=true

########################################################
# API / Integrations
########################################################
API_RATE_LIMIT_REQUESTS_PER_MINUTE=120
API_ALLOW_CORS_ORIGINS=*
API_ENABLE_GRAPHQL=false
API_TOKEN_EXPIRY_DAYS=30

########################################################
# Notifications
########################################################
NOTIFY_EMAIL_ENABLED=false
NOTIFY_EMAIL_SMTP_HOST=
NOTIFY_EMAIL_SMTP_PORT=587
NOTIFY_EMAIL_SMTP_USER=
NOTIFY_EMAIL_SMTP_PASSWORD=
NOTIFY_EMAIL_FROM_ADDRESS=noreply@example.com
NOTIFY_SLACK_WEBHOOK_URL=
NOTIFY_DISCORD_WEBHOOK_URL=

########################################################
# Maintenance / System
########################################################
SYSTEM_MAINTENANCE_MODE=false
SYSTEM_BACKUP_ENABLED=true
SYSTEM_BACKUP_SCHEDULE_CRON=0 3 * * *
SYSTEM_UPDATE_CHANNEL=stable
PROXY_MODE=true
```
</details>

---

## 🌐 Access

- Web UI → [http://localhost:8008](http://localhost:8008)  
- Admin dashboard → `/admin`  
- User login → `/auth/login`

---

## 📦 Data Persistence

- Database stored in `./db` (bind-mounted on host)  
- Mounted paths (e.g. `/mnt/deven`, `/plex1`) appear in FileHub for browsing and management  

---

## ⚖️ License

This project is licensed under [GPL-3.0](./LICENSE.txt).  
The Docker image includes the full source code in `/app`.  
Source will also be available at: [https://github.com/CipherWorkZ/filehub](https://github.com/CipherWorkZ/filehub) once v1.0.0 is published.
