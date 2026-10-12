# Day 1 — NGINX Troubleshooting Report

## Incident 1: NGINX configuration reload failed

### Symptoms
- `nginx -t` reports a configuration error.
- The reload does not apply the intended changes.
- The error may identify a file and line number.

### Diagnostic commands


sudo nginx -t
sudo systemctl status nginx
sudo journalctl -u nginx -n 50 --no-pager


### Root cause
Example: A missing semicolon, unmatched brace, invalid directive, or referenced file that does not exist.

### Fix
1. Read the error message and identify the file and line.
2. Correct the invalid directive or missing file.
3. Test again:


sudo nginx -t


4. Reload only after validation succeeds:


sudo systemctl reload nginx


### Verification
Run `sudo nginx -t` and `sudo systemctl status nginx`. Confirm the configuration test succeeds and the intended site behavior is correct.

### Evidence
Record the actual error message and the successful test result.

---

## Incident 2: Static page returns HTTP 404

### Symptoms
- The browser or `curl` receives `404 Not Found`.
- The expected HTML file exists, but the requested URL does not return it.

### Diagnostic commands


curl -i http://localhost:8082/
sudo nginx -T
ls -l /var/www/course-frontend/
sudo tail -n 30 /var/log/nginx/access.log
sudo tail -n 30 /var/log/nginx/error.log


### Root cause
Example: The NGINX `root` directive points to the wrong directory, or the requested file does not exist at the resolved path.

For a request to `/index.html` with:


root /var/www/wrong;


NGINX looks for:


/var/www/wrong/index.html


### Fix
Correct the document root:


root /var/www/course-frontend;
index index.html;


Then validate and reload:


sudo nginx -t
sudo systemctl reload nginx
curl -i http://localhost:8082/


### Verification
Confirm the home page returns HTTP 200 and the expected HTML content.

### Evidence
Record the actual URL, status code, configured root, and verification output.

---

## Incident 3: NGINX returns HTTP 502 for a backend

### Symptoms
- A proxied request returns `502 Bad Gateway`.
- The backend may be stopped, unreachable, or listening on a different address or port.

### Diagnostic commands


curl -i http://127.0.0.1:3000/
sudo ss -lntp | grep ':3000'
sudo tail -n 50 /var/log/nginx/error.log


If the backend is managed by systemd, inspect its service and logs:


sudo systemctl status YOUR_BACKEND_SERVICE
sudo journalctl -u YOUR_BACKEND_SERVICE -n 50 --no-pager


Replace `YOUR_BACKEND_SERVICE` with the actual service name. If you started the app manually, inspect that process and its terminal output instead.

### Root cause
Example: The backend process is not running, or NGINX points to the wrong upstream port.

### Fix
- Start or repair the backend using its documented startup method.
- Correct the upstream address or port if it is wrong.
- Confirm the backend responds directly before testing through NGINX.

### Verification

curl -i http://127.0.0.1:3000/
curl -i http://localhost:8082/


The second command tests a backend only if your NGINX configuration actually proxies that URL. For the static-only configuration in this Day 1 lab, a 502 is not expected.

### Evidence
Record the backend's actual listening state, the relevant error-log message, and the result after the fix.

---

## Incident 4: Port 8082 is already in use

### Symptoms
- NGINX reports that it cannot bind to port 8082.
- The configuration test or startup reports an address-in-use error.

### Diagnostic commands


sudo ss -lntp | grep ':8082'
sudo nginx -t
sudo systemctl status nginx


### Root cause
Example: Another process or an existing NGINX server block is already listening on the same address and port.

### Fix
1. Identify the process that owns the listener.
2. Determine whether it is an existing service that should remain running.
3. Reuse or deliberately reconfigure the existing listener, or select another port.
4. Validate and apply the intended configuration.

Do not kill a process until you understand its purpose.

### Verification

sudo ss -lntp | grep ':8082'
curl -i http://localhost:8082/


Confirm the expected process owns the listener and the site responds as intended.

### Evidence
Record the original listener, the chosen fix, and the final HTTP response.

---

## Incident report template

Use this template for future incidents:

- **Incident:**
- **Date:**
- **Symptoms:**
- **Diagnostic commands:**
- **Observed evidence:**
- **Root cause:**
- **Fix:**
- **Verification:**
- **Lesson learned:**


