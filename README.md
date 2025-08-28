# MINI Project | Fast API + Docker + CICD Pipeline

## What This Project Does
This project shows how to build a small **secure web application** and automate the whole process using GitHub Actions, Docker, and AWS ECR.

---
## What this app does
- It is a simple API with one endpoint:
  - **GET /healthz** → responds with `{"status":"ok"}`  
- Includes a basic unit test to check if it works.
---

## Steps in the Pipeline
GitHub Actions workflow runs these steps automatically:

1. **Checkout Code** → Pull the code from GitHub.
2. **Build App** → Create a Docker image for the web app.
3. **Run Tests** → Execute unit tests inside the container.
4. **Generate SBOM** → Use *Syft* to create a Software Bill of Materials (what’s inside the image).
5. **Scan for Vulnerabilities** → Use *Grype* to check for known security issues.  
   - The build fails if a **critical issue** is found.
6. **Sign the Image** → Use *Cosign* to digitally sign the image.
7. **Verify the Signature** → Confirm the image is really ours and safe to use.
8. **Push to AWS ECR** → Store the signed and verified image in AWS Elastic Container Registry (ECR).

---

## Security checks coverd
- Runs as a **non-root user** inside the container.
- **SBOM + Vulnerability Scan**
- **Cosign Signing** proves the image is trusted.

---

## How to Test Locally
Build and run the container:

```bash
docker build -t fastapi-app .
docker run -p 8080:8080 fastapi-app

open another powershell terminal and run below command
curl http://localhost:8080/healthz

Expected output



StatusCode        : 200
StatusDescription : OK
Content           : {"status":"ok"}
RawContent        : HTTP/1.1 200 OK
                    Content-Length: 15
                    Content-Type: application/json
                    Date: Thu, 28 Aug 2025 14:29:49 GMT
                    Server: uvicorn
                    
                    {"status":"ok"}
Forms             : {}
Headers           : {[Content-Length, 15], [Content-Type, application/json], [Date, Thu, 28 Aug 2025 14:29:49 GMT], [Server, uvicorn]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 15

```
