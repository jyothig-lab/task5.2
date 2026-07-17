docker-security/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── .dockerignore

modified dockerfile by Creating Non-root User
docker build -t docker-security:v2 .

docker run --rm docker-security:v2 whoami  ----appuser

docker run --rm docker-security:v2 id
uid=100(appuser) gid=101(appgroup) groups=101(appgroup),101(appgroup)

trivy image docker-security:v2
Python (python-pkg)
===================
Total: 6 (UNKNOWN: 0, LOW: 2, MEDIUM: 4, HIGH: 0, CRITICAL: 0)

dockerfile update -------RUN apk update && \    apk upgrade
docker build -t docker-security:v3 .
trivy image docker-security:v3
Total: 6 (UNKNOWN: 0, LOW: 2, MEDIUM: 4, HIGH: 0, CRITICAL: 0)


docker run --rm docker-security:v3 whoami
appuser

docker run --rm docker-security:v3 id
uid=100(appuser) gid=101(appgroup) groups=101(appgroup),101(appgroup)
------------------------------------------------------------------------------------
Running as root (less secure):

uid=0(root) gid=0(root) groups=0(root)

Running as non-root (more secure):

uid=100(appuser) gid=101(appgroup)
-----------------------------------------------------------------------------------------
I focused on Docker image security. First, I created a dedicated non-root user and configured the container to run the application
using that user instead of the default root user, following the principle of least privilege. 
Next, I scanned the Docker image using Trivy to identify operating system and Python package vulnerabilities. 
Based on the scan results, I updated the Alpine packages, upgraded application dependencies where fixes were available, and removed unnecessary packages. 
After rebuilding the image, I scanned it again to verify that there were no Critical vulnerabilities and confirmed the container was running as the non-root user. 
These changes improved the overall security of the Docker image while keeping the application fully functional



Trivy is one of the most popular open-source security scanners.

