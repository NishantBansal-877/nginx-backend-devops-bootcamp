# Day 1 — NGINX Commands and Validation

## 1. Check NGINX service status

sudo systemctl status nginx


**Purpose:** Check whether the NGINX service is running.

## 2. Validate NGINX configuration


sudo nginx -t


**Purpose: Check configuration syntax and whether referenced files can be loaded.

Expected: a message indicating the syntax is OK and the test is successful.

## 3. Reload NGINX safely


sudo systemctl reload nginx


**Purpose:** Apply configuration changes without intentionally stopping the service.

Run `sudo nginx -t` first.

## 4. Inspect listening TCP ports


sudo ss -lntp


Filter for the lab port:


sudo ss -lntp | grep ':8082'


**Purpose:** Check whether a process is listening on the expected port.

## 5. Test the home page


curl -i http://localhost:8082/


**Purpose:** Send an HTTP request and inspect the response headers and body.

Expected: HTTP 200 when the site is configured correctly.

## 6. Test a missing file


curl -i http://localhost:8082/missing.html


**Purpose:** Verify the configured missing-file behavior.

Expected: HTTP 404.

## 7. Inspect access logs


sudo tail -n 30 /var/log/nginx/access.log


**Purpose:** Inspect recent HTTP requests and status codes.

## 8. Inspect error logs


sudo tail -n 30 /var/log/nginx/error.log


**Purpose:** Investigate filesystem errors, upstream failures, and configuration-related runtime problems.

## 9. Inspect network interfaces and routes


ip addr
ip route


**Purpose:** Inspect local IP addresses, interfaces, and the Linux routing table.

## 10. Check backend availability (if running)


curl -i http://127.0.0.1:3000/
sudo ss -lntp | grep ':3000'


**Purpose:** Check whether a local test backend is reachable and listening.

## Lab notes

- Configured port: 8082
- Document root: `/var/www/course-frontend`
- Expected home page status: 200
- Expected missing-file status: 404
- Configuration validation: record your actual result
- Reload result: record your actual result

