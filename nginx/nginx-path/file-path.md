
# NGINX Configuration Notes

## 1. Important NGINX File Paths

| Path | Purpose |
|---|---|
| `/etc/nginx/nginx.conf` | Main configuration file used to manage global NGINX settings. |
| `/etc/nginx/sites-available/` | Stores website configuration files, including enabled and disabled websites. |
| `/etc/nginx/sites-enabled/` | Contains symbolic links to enabled website configuration files that NGINX loads. |
| `/var/www/html/` | Default Website Directory |
| `/var/log/nginx/access.log` | Access Logs |
| `/var/log/nginx/error.log` | Error Logs |

---

## 2. Create and Enable NGINX Website

### Step 1: Create Website Configuration

Create a configuration file inside `sites-available`.

```bash
sudo nano /etc/nginx/sites-available/mywebsite.conf
```

Add the following configuration:

```nginx
server {
    listen 80;
    server_name localhost;

    root /var/www/mywebsite;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

**Configuration Explanation:**

| Directive | Purpose |
|---|---|
| `listen 80` | Listen for HTTP requests on port 80 |
| `server_name localhost` | Define the website hostname |
| `root /var/www/mywebsite` | Define the website root directory |
| `index index.html` | Define the default index file |
| `location /` | Handle requests for the root URL path |
| `try_files` | Check whether the requested file or directory exists |

### Step 2: Create Website Directory

Create a directory to store website files.

```bash
sudo mkdir -p /var/www/mywebsite
```

Create an HTML file:

```bash
echo "<h1>Hello from NGINX!</h1>" | sudo tee /var/www/mywebsite/index.html
```

### Step 3: Enable Website (Symbolic Link)

Create a symbolic link from `sites-available` to `sites-enabled`.

```bash
sudo ln -s /etc/nginx/sites-available/mywebsite.conf /etc/nginx/sites-enabled/
```

**Command Explanation:**

| Command | Purpose |
|---|---|
| `sudo` | Run command as administrator |
| `ln` | Create a link |
| `-s` | Create a symbolic link |
| `/etc/nginx/sites-available/mywebsite.conf` | Source configuration file |
| `/etc/nginx/sites-enabled/` | Destination directory |

**Directory Structure:**

```text
/etc/nginx/
├── nginx.conf
├── sites-available/
│   ├── default
│   └── mywebsite.conf
└── sites-enabled/
    ├── default -> /etc/nginx/sites-available/default
    └── mywebsite.conf -> /etc/nginx/sites-available/mywebsite.conf
```

### Step 4: Check Enabled Websites

```bash
ls -l /etc/nginx/sites-enabled/
```

Expected output:

```text
mywebsite.conf -> /etc/nginx/sites-available/mywebsite.conf
```

### Step 5: Test NGINX Configuration

```bash
sudo nginx -t
```

Expected output:

```text
nginx: configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

### Step 6: Reload NGINX

```bash
sudo systemctl reload nginx
```

### Step 7: Disable Default Website (Optional)

For a test server without other websites, you can disable the default site to avoid conflicts.

```bash
sudo rm /etc/nginx/sites-enabled/default
```

Test and reload:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

### Step 8: Test Website

```bash
curl -H "Host: localhost" http://127.0.0.1
```

Expected output:

```html
<h1>Hello from NGINX!</h1>
```

---

## 3. Disable and Re-enable Website

### Disable Website

Remove the symbolic link from `sites-enabled`.

```bash
sudo rm /etc/nginx/sites-enabled/mywebsite.conf
```

**Note:** This removes only the symbolic link, not the original configuration file.

Apply changes:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

### Re-enable Website

```bash
sudo ln -s /etc/nginx/sites-available/mywebsite.conf /etc/nginx/sites-enabled/
```

Apply changes:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 4. Important NGINX Commands

| Command | Purpose |
|---|---|
| `sudo apt install nginx -y` | Install NGINX |
| `nginx -v` | Check NGINX version |
| `sudo systemctl start nginx` | Start NGINX service |
| `sudo systemctl stop nginx` | Stop NGINX service |
| `sudo systemctl restart nginx` | Restart NGINX service |
| `sudo systemctl reload nginx` | Reload NGINX configuration |
| `sudo systemctl enable nginx` | Enable NGINX at system startup |
| `sudo systemctl status nginx` | Check NGINX service status |
| `sudo nginx -t` | Test NGINX configuration syntax |
| `ls /etc/nginx/sites-available/` | List available website configurations |
| `ls -l /etc/nginx/sites-enabled/` | List enabled website configurations |
| `sudo nano /etc/nginx/nginx.conf` | Edit main NGINX configuration |
| `sudo tail -f /var/log/nginx/access.log` | Monitor access logs |
| `sudo tail -f /var/log/nginx/error.log` | Monitor error logs |

---

## 5. NGINX Configuration Workflow

### Enable a Website

```text
1. Create Website Configuration
              |
              v
2. Store in sites-available/
              |
              v
3. Create Symbolic Link (ln -s)
              |
              v
4. Link to sites-enabled/
              |
              v
5. Test Configuration (nginx -t)
              |
              v
6. Reload NGINX
              |
              v
7. Test Website in Browser
```

### Disable a Website

```text
1. Remove Symbolic Link
              |
              v
2. Test Configuration
              |
              v
3. Reload NGINX
              |
              v
4. Website Configuration Disabled
```

---

## 6. Quick Reference

**Create Configuration:**

```bash
sudo nano /etc/nginx/sites-available/mywebsite.conf
```

**Enable Website:**

```bash
sudo ln -s /etc/nginx/sites-available/mywebsite.conf /etc/nginx/sites-enabled/
```

**Disable Website:**

```bash
sudo rm /etc/nginx/sites-enabled/mywebsite.conf
```

**Test and Apply Configuration:**

```bash
sudo nginx -t && sudo systemctl reload nginx
```

**Check Logs:**

```bash
sudo tail -f /var/log/nginx/error.log
```

---

## 7. Important Notes

- `sites-available` stores website configuration files.
- `sites-enabled` usually contains symbolic links to active website configurations.
- Creating a configuration file does not automatically enable the website.
- Always run `sudo nginx -t` before reloading NGINX.
- Use `reload` to apply configuration changes without a full service restart.
- Removing a symbolic link does not delete the original configuration file.
- `/var/www/html/` is the default document root on Ubuntu, but it can be changed.
- The `sites-enabled` directory must be included by NGINX configuration for its files to be loaded.
- A website's `server_name` must match the request hostname for name-based virtual hosting.
- NGINX configuration paths may differ depending on the operating system and installation method.
