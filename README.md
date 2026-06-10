# Nginx Config Boilerplate

A secure, performance, and reusable Nginx configuration layout optimized for modern web applications (Python, Go, Node.js, .NET) sitting behind Cloudflare.


## Configuration Workflow: `sites-available` vs `sites-enabled`

This repository follows the industry-standard Nginx directory convention:
* **`sites-available/`**: The storage room. It contains all your configuration files, whether the websites are currently live or not.
* **`sites-enabled/`**: The active stage. It contains symbolic links (shortcuts) to the files in `sites-available`. Nginx only loads configurations that are present in this folder.

This separation allows you to quickly enable or disable websites without deleting your actual configuration files.

### Enabling Your Configuration (Symlink)

To activate the configuration, create a symbolic link from `sites-available` to `sites-enabled` using the following command:

```bash
sudo ln -s /etc/nginx/sites-available/myconfig.conf /etc/nginx/sites-enabled/
```

### Verifying and Reloading Nginx
Always test your configuration for syntax errors before restarting the service:

```bash
Bash
# Test configuration
sudo nginx -t

# If successful, reload Nginx without dropping active connections
sudo systemctl reload nginx
```

### Why UNIX Sockets?
Instead of routing internal traffic via local network ports (e.g., http://127.0.0.1:8000), this setup uses UNIX Domain Sockets (proxy_pass http://unix:...).

Performance: Bypasses the network stack completely, reducing latency and kernel context-switching overhead.

Security: Access is locked down using native Linux file permissions (chmod/chown), preventing unauthorized local users from intercepting internal traffic.
