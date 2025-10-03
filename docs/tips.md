# Troubleshooting Tips

- Check if Apache or Nginx is using the same port.
- Use `systemctl status <service>` and `journalctl -xe` for logs.
- Ensure firewall allows HTTP/HTTPS or custom ports.
- Restart services after changing configuration files.
