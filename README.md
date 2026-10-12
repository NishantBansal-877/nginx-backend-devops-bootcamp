# NGINX Backend & DevOps Bootcamp

## Day 1 — NGINX Fundamentals and Troubleshooting

### Objective
Learn basic NGINX configuration, static-file hosting, Linux networking, and troubleshooting.

### Topics covered
- NGINX installation and service management
- Configuration validation and reloads
- Server blocks and listening ports
- Static-file routing using `root`, `index`, and `try_files`
- HTTP status codes: 200, 403, 404, and 502
- Linux network interfaces and routing tables
- Listening ports and socket inspection
- NGINX access and error logs

### Lab configuration
- Port: `8082`
- Document root: `/var/www/course-frontend`
- Homepage: `/var/www/course-frontend/index.html`

### Validation

sudo nginx -t
sudo systemctl status nginx
sudo ss -lntp
curl -i http://localhost:8082/
curl -i http://localhost:8082/missing.html


### Troubleshooting
See [day-01/troubleshooting.md](day-01/troubleshooting.md) for incident reports, diagnostic commands, root causes, and fixes.

### Evidence
Screenshots and command outputs will be added after running and verifying the lab.

### Learning outcome
Understand how NGINX serves static files, how Linux exposes listening ports, and how to investigate common web-server errors.
