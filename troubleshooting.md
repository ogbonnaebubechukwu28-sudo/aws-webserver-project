## Troubleshooting Log

### Issue 1 — Permission Denied During SSH (Unprotected Key File)

**Problem:**
When trying to connect to the EC2 instance via SSH, got the error:
```
WARNING: UNPROTECTED PRIVATE KEY FILE!
Permission denied (publickey)
```

**Cause:**
The `.pem` key file was stored in the Windows filesystem (`/mnt/c/Users/...`). The `chmod 400` command does not work on Windows-mounted directories in WSL.

**Solution:**
Copied the key file to the Linux home directory first, then set the correct permissions:
```bash
cp "my ec2 key.pem" ~/myec2key.pem
chmod 400 ~/myec2key.pem
ssh -i ~/myec2key.pem ubuntu@PUBLIC-IP
```


### Issue 2 — Website Showing Wrong Page (Two EC2 Instances Running)

**Problem:**
After configuring Nginx and creating a custom HTML page, the browser kept showing the default Nginx welcome page instead of the custom page.

**Cause:**
There were two EC2 instances running simultaneously from two different assignments. The public IP `13.48.6.29` was pointing to the old instance which still had the default Nginx page.

**Solution:**
Stopped the old EC2 instance from the AWS Console. This freed up the correct instance to serve the custom page.


### Issue 3 — Nginx Config Syntax Error (Stray Character)

**Problem:**
After editing the Nginx config file, Nginx failed to restart with the error:
```
Job for nginx.service failed because the control process exited with error code
```

**Cause:**
A stray `y` character was accidentally added at the end of the config file while saving in nano, corrupting the syntax.

**Solution:**
Opened the file again in nano, deleted the stray character, verified with:
```bash
sudo nginx -t
```
Then restarted Nginx:
```bash
sudo systemctl restart nginx
```


### Issue 4 — Site Not Reachable in Browser (Wrong Protocol)

**Problem:**
After installing Nginx and opening Port 80, the website was not loading in the browser.

**Cause:**
Was typing `https://` instead of `http://` in the browser. Port 443 (HTTPS) was not configured, only Port 80 (HTTP).

**Solution:**
Changed the URL to:
```
http://PUBLIC-IP
```
The page loaded immediately.

### Issue 5 — Key File Not Found (Spaces in Filename)

**Problem:**
Running `chmod 400 my-ec2-key.pem` returned:
```
chmod: cannot access 'my': No such file or directory
```

**Cause:**
The key file was named `my ec2 key.pem` with spaces, and the terminal interpreted each word as a separate argument.

**Solution:**
Wrapped the filename in quotes:
```bash
chmod 400 "my ec2 key.pem"
