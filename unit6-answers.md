# Unit 6 Answers: Deployment and Final Testing

Subject: Full Stack Development  
Unit: Deployment and Final Testing

---

## 1. Describe the role of environment variables in configuring API endpoints in a production environment.

Environment variables are used to store configuration values outside the source code. In a production environment, they are commonly used to configure API endpoints, database URLs, secret keys, and other deployment-specific values.

For frontend applications, an API endpoint may be different in development and production.

Example:

```text
Development API:
http://localhost:8080/api

Production API:
https://api.example.com/api
```

Instead of hardcoding the API URL in the code, it is stored in an environment variable.

React example:

```env
VITE_API_BASE_URL=https://api.example.com/api
```

Usage:

```js
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

fetch(`${API_BASE_URL}/products`);
```

Role of environment variables:

- They separate configuration from source code.
- They allow different settings for development, testing, and production.
- They prevent hardcoding of backend API URLs.
- They make deployment easier on platforms like Netlify and Vercel.
- They reduce the risk of accidentally using local API URLs in production.

Flow:

```text
Deployment platform
      |
      v
Environment variable stores API URL
      |
      v
Frontend reads API URL during build/runtime
      |
      v
Frontend calls production backend
```

Environment variables improve flexibility and make the application easier to configure across environments.

---

## 2. Discuss the importance of build and deployment pipelines in modern web hosting platforms.

A build and deployment pipeline is an automated process that converts source code into a production-ready application and publishes it to a hosting platform. Platforms like Netlify and Vercel provide automatic pipelines for frontend applications.

Typical pipeline:

```text
Developer pushes code to GitHub
        |
        v
Hosting platform detects change
        |
        v
Installs dependencies
        |
        v
Runs build command
        |
        v
Deploys generated files
        |
        v
Website becomes live
```

Importance of build and deployment pipelines:

- They automate deployment.
- They reduce manual errors.
- They make deployment faster.
- They support continuous deployment from GitHub.
- They provide build logs for debugging.
- They allow rollback to previous deployments.
- They ensure the same build steps are followed every time.

Example build command for React:

```bash
npm run build
```

Example output folder:

```text
dist/
```

or

```text
build/
```

In modern web hosting, deployment pipelines help teams release updates quickly and safely. They also support preview deployments, where changes can be tested before being merged into the main production branch.

---

## 3. Illustrate the steps involved in deploying a web application using platforms like Netlify or Vercel

Deploying a web application on Netlify or Vercel usually involves connecting a Git repository, configuring build settings, and deploying the generated build output.

General deployment steps:

```text
Create frontend project
        |
        v
Push code to GitHub
        |
        v
Connect GitHub repo to Netlify/Vercel
        |
        v
Set build command and output folder
        |
        v
Add environment variables
        |
        v
Deploy application
        |
        v
Test production URL
```

Steps:

1. Build the frontend application locally and ensure it works.
2. Push the project code to GitHub.
3. Open Netlify or Vercel.
4. Import the GitHub repository.
5. Select the project framework, such as React.
6. Configure build command.

Example:

```text
npm run build
```

7. Configure output directory.

For Vite:

```text
dist
```

For Create React App:

```text
build
```

8. Add environment variables such as API base URL.
9. Click Deploy.
10. Test the deployed URL.

After deployment, check:

- Page loads correctly.
- API calls work.
- Environment variables are correct.
- No console errors are present.
- Routes work after refresh.

This process makes the frontend application publicly available on the internet.

---

## 4. Describe the process of integrating a deployed frontend with a backend API.

Integrating a deployed frontend with a backend API means configuring the frontend application so that it can send requests to the production backend server.

Basic integration flow:

```text
Deployed Frontend
      |
      v
API request using production API URL
      |
      v
Backend server
      |
      v
JSON response
      |
      v
Frontend displays data
```

Steps:

1. Deploy the backend on a cloud platform such as Render, AWS, Railway, or any server.
2. Note the backend base URL.

Example:

```text
https://my-backend.onrender.com/api
```

3. Configure this URL in the frontend environment variable.

```env
VITE_API_BASE_URL=https://my-backend.onrender.com/api
```

4. Use the environment variable in API calls.

```js
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

export async function getProducts() {
  const response = await fetch(`${API_BASE_URL}/products`);
  return await response.json();
}
```

5. Configure CORS in the backend to allow the deployed frontend domain.

Spring Boot example:

```java
@CrossOrigin(origins = "https://my-app.vercel.app")
@RestController
public class ProductController {
}
```

6. Redeploy the frontend and backend.
7. Test full frontend-backend communication.

Important checks:

- Correct production API URL.
- CORS allows frontend domain.
- Backend is publicly accessible.
- HTTPS is used.
- API response format matches frontend expectations.

---

## 5. Discuss key factors affecting performance in deployed web applications.

Performance in deployed web applications depends on frontend, backend, network, database, and hosting factors. A slow application can affect user experience and reduce reliability.

Key factors:

### Bundle size

Large JavaScript and CSS files increase page load time. Unused code and heavy libraries should be avoided.

### Image size

Large uncompressed images slow down loading. Images should be compressed and served in optimized formats.

### API response time

Slow backend APIs delay data display on the frontend.

### Database performance

Poor queries, missing indexes, and large unfiltered results slow down backend responses.

### Network latency

If the frontend and backend are hosted far from users, requests take more time.

### Caching

Caching static assets and API responses can improve performance.

### Hosting platform

Platforms like Netlify and Vercel use CDN-based hosting for frontend assets, which improves global delivery.

Performance flow:

```text
User opens app
      |
      v
HTML/CSS/JS assets load
      |
      v
Frontend calls backend API
      |
      v
Backend queries database
      |
      v
Response returned to frontend
```

Optimization techniques:

- Minify JavaScript and CSS.
- Compress images.
- Use lazy loading.
- Use pagination for large data.
- Cache static assets.
- Optimize backend queries.
- Use CDN hosting.
- Avoid unnecessary API calls.

Good performance testing before deployment helps ensure the application works smoothly in production.

---

## 6. Explain the need for final testing before deploying an application to production.

Final testing is required before production deployment to ensure that the application works correctly, securely, and efficiently for real users. An application may work locally but fail in production due to different URLs, missing environment variables, CORS issues, build errors, or backend configuration problems.

Need for final testing:

- To verify frontend-backend integration.
- To check that production API URLs are correct.
- To confirm environment variables are configured.
- To test all important user flows.
- To identify build errors before release.
- To check performance and loading speed.
- To ensure error messages work properly.
- To verify security-related settings.

Final testing checklist:

```text
Build succeeds
      |
      v
Frontend pages load
      |
      v
API calls work
      |
      v
Forms submit correctly
      |
      v
Errors are handled
      |
      v
Performance is acceptable
      |
      v
Application ready for production
```

Examples of tests:

- Login works.
- CRUD operations work.
- Forms validate input.
- API errors display proper messages.
- Protected routes behave correctly.
- Page refresh does not break routing.
- Production backend is reachable.

Final testing reduces production failures and improves user confidence.

---

## 7. Illustrate the steps to integrate a frontend deployed on Vercel with a backend hosted on a cloud server.

To integrate a frontend deployed on Vercel with a backend hosted on a cloud server, the frontend must be configured with the backend URL and the backend must allow requests from the Vercel domain.

Integration steps:

```text
Deploy backend on cloud server
        |
        v
Get backend API URL
        |
        v
Set API URL in Vercel environment variable
        |
        v
Configure backend CORS
        |
        v
Redeploy frontend
        |
        v
Test API calls
```

Step 1: Deploy backend.

Example backend URL:

```text
https://student-api.onrender.com/api
```

Step 2: Add environment variable in Vercel.

For Vite React:

```text
VITE_API_BASE_URL=https://student-api.onrender.com/api
```

Step 3: Use it in frontend code.

```js
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

fetch(`${API_BASE_URL}/students`);
```

Step 4: Configure CORS in backend.

```java
@Configuration
public class CorsConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                        .allowedOrigins("https://my-app.vercel.app")
                        .allowedMethods("GET", "POST", "PUT", "DELETE")
                        .allowedHeaders("*");
            }
        };
    }
}
```

Step 5: Redeploy frontend on Vercel.

Step 6: Test the deployed frontend URL and check browser Network tab for API responses.

---

## 8. Apply best practices to secure API keys using environment variables during deployment.

API keys should not be hardcoded directly in source code because code may be pushed to GitHub and become visible to others. Environment variables are used to store sensitive or deployment-specific values outside the codebase.

Bad practice:

```js
const API_KEY = "abc123secret";
```

Better practice:

```env
VITE_API_URL=https://api.example.com
```

Usage:

```js
const apiUrl = import.meta.env.VITE_API_URL;
```

Important security note: Frontend environment variables are still included in the browser bundle if they are used by frontend code. Therefore, truly secret API keys should not be placed in frontend code at all. They should be used only on the backend.

Best practices:

- Do not hardcode secrets in source code.
- Do not commit `.env` files containing secrets.
- Add `.env` to `.gitignore`.
- Configure environment variables in Netlify or Vercel dashboard.
- Use backend APIs to protect secret keys.
- Use different keys for development and production.
- Rotate exposed keys immediately.
- Restrict API keys by domain or usage when possible.

Deployment flow:

```text
Deployment dashboard
      |
      v
Environment variables configured
      |
      v
Build process reads variables
      |
      v
Application uses configuration safely
```

For secure design, frontend should call your backend, and the backend should call third-party services using secret keys.

---

## 9. Show how to update and redeploy an application after making changes in the codebase.

Updating and redeploying an application means modifying the code, testing it, pushing it to GitHub, and allowing the deployment platform to rebuild and publish the new version.

Steps:

```text
Make code changes
      |
      v
Test locally
      |
      v
Commit changes
      |
      v
Push to GitHub
      |
      v
Netlify/Vercel builds automatically
      |
      v
New version is deployed
```

Example workflow:

```bash
git status
git add .
git commit -m "Update API endpoint configuration"
git push origin main
```

After pushing:

1. Open Netlify or Vercel dashboard.
2. Check deployment status.
3. Review build logs.
4. Open the production URL.
5. Test changed functionality.

If environment variables changed:

1. Update them in the hosting dashboard.
2. Trigger a redeploy.
3. Verify that the new values are used.

Important checks after redeployment:

- Build completed successfully.
- Updated UI is visible.
- API calls work correctly.
- Browser console has no major errors.
- Old cached version is not being shown.

Redeployment is usually automatic on modern platforms, but final production testing is still required.

---

## 10. Design a deployment workflow for a full-stack application ensuring secure API communication and optimal performance.

A full-stack deployment workflow should safely deploy both frontend and backend, configure production API communication, and optimize performance.

Recommended workflow:

```text
Code repository
      |
      v
Frontend build and deployment
      |
      v
Backend deployment
      |
      v
Environment variables configured
      |
      v
CORS and HTTPS configured
      |
      v
Final integration testing
      |
      v
Production release
```

Frontend deployment:

- Deploy React frontend on Netlify or Vercel.
- Set build command such as `npm run build`.
- Set output folder such as `dist` or `build`.
- Configure environment variable for backend API URL.

Backend deployment:

- Deploy backend on a cloud server or platform.
- Configure database connection.
- Configure environment variables for secrets.
- Enable HTTPS.

Secure API communication:

- Use HTTPS URLs.
- Do not hardcode secrets in frontend.
- Configure backend CORS for only allowed frontend domains.
- Use authentication tokens if APIs are protected.

Performance optimization:

- Minify frontend bundle.
- Compress images.
- Use lazy loading.
- Cache static assets.
- Optimize database queries.
- Avoid unnecessary API calls.

Mental model:

```text
Frontend should know only:
  "Where is my backend?"

Backend should protect:
  "Secrets, database, authentication, business rules"
```

Final testing should verify that the deployed frontend can correctly call the deployed backend and handle success and error responses.

---

## 11. Analyze common deployment failures and propose solutions to overcome them.

Deployment failures occur when the application cannot build, run, connect to APIs, or work correctly in production. These failures are common when moving from local development to cloud hosting.

Common failures and solutions:

| Failure | Possible Cause | Solution |
|---|---|---|
| Build fails | Missing dependency or wrong build command | Check logs, install dependency, fix command |
| API calls fail | Wrong production API URL | Update environment variable |
| CORS error | Backend does not allow frontend domain | Configure CORS in backend |
| Blank page | Wrong output folder or routing issue | Check build folder and route config |
| Missing API key | Env variable not set in platform | Add variable and redeploy |
| 404 on page refresh | SPA routing not configured | Add redirect/rewrite rule |
| App works locally but not production | Different environment settings | Compare local and production config |

Debugging flow:

```text
Deployment issue
      |
      v
Check build logs
      |
      v
Check environment variables
      |
      v
Check browser console
      |
      v
Check Network tab
      |
      v
Check backend logs
      |
      v
Apply fix and redeploy
```

Examples:

- If build fails on Vercel due to dependency errors, run `npm install` locally and check `package.json`.
- If API call fails, inspect the final request URL in browser Network tab.
- If CORS error occurs, add the deployed frontend origin to backend CORS config.

Deployment failures should be solved systematically using logs and environment checks.

---

## 12. You have developed a React frontend and deployed it on Netlify, while your backend API is hosted on another server. Demonstrate how you will configure environment variables to connect the frontend with the backend securely.

When a React frontend is deployed on Netlify and the backend is hosted separately, the frontend should use an environment variable for the backend API base URL.

Example backend URL:

```text
https://student-backend.onrender.com/api
```

For Vite React, create an environment variable:

```text
VITE_API_BASE_URL=https://student-backend.onrender.com/api
```

In Netlify:

1. Open Netlify dashboard.
2. Select the site.
3. Go to Site configuration or Environment variables.
4. Add `VITE_API_BASE_URL`.
5. Set its value to the backend URL.
6. Redeploy the site.

Frontend usage:

```js
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

export async function getStudents() {
  const response = await fetch(`${API_BASE_URL}/students`);
  return await response.json();
}
```

Backend CORS configuration:

```java
registry.addMapping("/api/**")
        .allowedOrigins("https://my-site.netlify.app")
        .allowedMethods("GET", "POST", "PUT", "DELETE")
        .allowedHeaders("*");
```

Flow:

```text
Netlify frontend
      |
      v
Reads VITE_API_BASE_URL
      |
      v
Calls backend server
      |
      v
Backend allows Netlify origin
      |
      v
JSON response returned
```

Security note: Do not place private secret keys in frontend environment variables. Frontend variables are visible in browser builds. Keep secrets on the backend.

---

## 13. A deployed application on Vercel is not fetching data from the backend API due to incorrect endpoint configuration. Show how you would identify and fix the issue.

If a Vercel-deployed app is not fetching backend data, the issue may be an incorrect API endpoint, missing environment variable, wrong variable name, or CORS problem.

Steps to identify the issue:

```text
Open deployed app
      |
      v
Open browser developer tools
      |
      v
Check Console tab
      |
      v
Check Network tab
      |
      v
Inspect actual API request URL
      |
      v
Compare with backend production URL
```

Common incorrect code:

```js
fetch("http://localhost:8080/api/products");
```

This fails in production because `localhost` refers to the user's machine, not the backend server.

Correct approach:

```env
VITE_API_BASE_URL=https://product-api.onrender.com/api
```

Usage:

```js
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

fetch(`${API_BASE_URL}/products`);
```

Fix steps in Vercel:

1. Open Vercel project dashboard.
2. Go to Settings.
3. Open Environment Variables.
4. Add or update `VITE_API_BASE_URL`.
5. Redeploy the project.
6. Test again using the production URL.

Also verify backend CORS:

```java
.allowedOrigins("https://your-app.vercel.app")
```

The key is to inspect the actual request URL in the Network tab and ensure it points to the deployed backend.

---

## 14. After deployment, your application works locally but fails in production due to missing API keys. Apply the correct method to configure environment variables and redeploy the application.

If an application works locally but fails in production due to missing API keys, it means the environment variables are configured locally but not configured on the deployment platform.

Local setup may use a `.env` file:

```env
VITE_API_BASE_URL=https://api.example.com
VITE_PUBLIC_MAP_KEY=public-key
```

However, this file is usually not pushed to GitHub for security reasons. Therefore, the same variables must be added manually in Netlify or Vercel.

Correct method:

1. Identify the missing variable from the error message or code.
2. Open the deployment platform dashboard.
3. Go to Environment Variables.
4. Add the variable name exactly as used in code.
5. Add the correct production value.
6. Save changes.
7. Redeploy the application.

Flow:

```text
Production app fails
      |
      v
Check missing variable
      |
      v
Add variable in platform dashboard
      |
      v
Redeploy app
      |
      v
Test production again
```

Example usage:

```js
const apiUrl = import.meta.env.VITE_API_BASE_URL;
```

Important points:

- Variable names must match exactly.
- Vite frontend variables must usually start with `VITE_`.
- Create React App variables must start with `REACT_APP_`.
- After changing environment variables, redeployment is required.
- Secret backend keys should be stored on the backend, not in frontend code.

This fixes missing configuration issues in production.

---

## 15. You need to deploy a full-stack application where the frontend is on Netlify and the backend is on a cloud platform (e.g., AWS/Render). Apply the correct method to integrate both and ensure proper communication.

To integrate a Netlify frontend with a backend hosted on AWS, Render, or another cloud platform, both applications must be deployed and configured to communicate using the production backend URL.

Deployment architecture:

```text
User Browser
     |
     v
Netlify Frontend
     |
     v
Cloud Backend API
     |
     v
Database
```

Steps:

1. Deploy backend on cloud platform.
2. Verify backend APIs using browser or Postman.
3. Copy the production backend base URL.

Example:

```text
https://fsd-backend.onrender.com/api
```

4. Add this URL to Netlify environment variables.

```text
VITE_API_BASE_URL=https://fsd-backend.onrender.com/api
```

5. Use the variable in frontend API calls.

```js
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

fetch(`${API_BASE_URL}/orders`);
```

6. Configure CORS in backend to allow Netlify domain.

```java
registry.addMapping("/api/**")
        .allowedOrigins("https://my-app.netlify.app")
        .allowedMethods("GET", "POST", "PUT", "DELETE")
        .allowedHeaders("*");
```

7. Redeploy backend if CORS changed.
8. Redeploy frontend after environment variable changes.
9. Test all important flows.

Final checks:

- Frontend uses cloud backend URL, not `localhost`.
- Backend allows Netlify origin.
- HTTPS is used.
- Request and response JSON formats match.
- Errors are handled properly.

This ensures proper communication between separately deployed frontend and backend.

---

## 16. During deployment on Vercel, the build fails due to dependency issues. Demonstrate how you would troubleshoot and resolve the problem.

Dependency issues during Vercel deployment usually happen when packages are missing, versions conflict, lock files are outdated, or the build command differs from local setup.

Troubleshooting flow:

```text
Build fails on Vercel
      |
      v
Read build logs
      |
      v
Identify missing/conflicting dependency
      |
      v
Fix package.json or lock file
      |
      v
Test build locally
      |
      v
Push fix and redeploy
```

Steps:

1. Open Vercel deployment logs.
2. Find the exact error message.

Examples:

```text
Module not found
Cannot find package
Dependency conflict
npm ERR!
```

3. Check `package.json` to ensure the dependency is listed.

```bash
npm install package-name
```

4. Run local build.

```bash
npm run build
```

5. If lock file is outdated, regenerate it.

```bash
npm install
```

6. Commit updated files.

```bash
git add package.json package-lock.json
git commit -m "Fix dependency issue"
git push
```

7. Redeploy on Vercel.

Common solutions:

- Add missing dependency.
- Fix import path.
- Use compatible dependency versions.
- Ensure correct Node.js version.
- Check build command.
- Delete unused imports.

The main rule is to reproduce the build locally, fix the dependency issue, and then redeploy.

---

## 17. Your frontend is successfully deployed, but API requests are blocked due to CORS errors. Apply appropriate backend configuration to resolve this issue.

CORS errors occur when the browser blocks a frontend request because the backend has not allowed the frontend's origin. If the frontend is deployed successfully but API requests fail with CORS errors, the backend must be configured to allow the deployed frontend domain.

Example error:

```text
Access to fetch at 'https://api.example.com'
from origin 'https://my-app.vercel.app'
has been blocked by CORS policy
```

Spring Boot global CORS configuration:

```java
@Configuration
public class CorsConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                        .allowedOrigins("https://my-app.vercel.app")
                        .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                        .allowedHeaders("*");
            }
        };
    }
}
```

Controller-level option:

```java
@CrossOrigin(origins = "https://my-app.vercel.app")
@RestController
public class ProductController {
}
```

CORS flow:

```text
Browser sends request from frontend domain
        |
        v
Backend checks allowed origins
        |
   +----+----+
   |         |
Allowed   Not allowed
   |         |
Request   Browser blocks request
works
```

Important points:

- Use the exact frontend domain.
- Include required HTTP methods.
- Include `OPTIONS` for preflight requests.
- Redeploy backend after changing CORS.
- Avoid allowing all origins in production unless there is a strong reason.

---

## 18. You have updated the backend API endpoint, but the frontend still calls the old URL. Show how you will update environment variables and redeploy the application.

If the backend API endpoint changes, the frontend must be updated to use the new backend URL. If the frontend uses environment variables, the fix is usually done in the deployment dashboard.

Old backend URL:

```text
https://old-api.example.com/api
```

New backend URL:

```text
https://new-api.example.com/api
```

Frontend environment variable:

```text
VITE_API_BASE_URL=https://new-api.example.com/api
```

Steps:

1. Identify where the frontend gets the API URL.
2. Check if it uses an environment variable.
3. Open Netlify or Vercel dashboard.
4. Update the API URL variable.
5. Save changes.
6. Redeploy the frontend.
7. Test production API calls.

Code:

```js
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

fetch(`${API_BASE_URL}/products`);
```

Update flow:

```text
Backend URL changes
      |
      v
Update env variable in hosting platform
      |
      v
Redeploy frontend
      |
      v
Frontend build uses new URL
      |
      v
API calls work again
```

If the URL is hardcoded in code, replace it with an environment variable to avoid the same issue in future deployments.

---

## 19. A deployed application exposes sensitive API keys in the frontend code. Apply secure practices to fix this issue and prevent future occurrences.

If sensitive API keys are exposed in frontend code, anyone can view them through browser developer tools or bundled JavaScript files. This is a serious security issue.

Bad practice:

```js
const SECRET_KEY = "sk_live_123456";
```

Why this is unsafe:

- Frontend code is visible to users.
- Keys can be copied and misused.
- Attackers may make unauthorized API calls.
- Billing or data security may be affected.

Correct solution:

```text
Frontend
   |
   v
Your Backend API
   |
   v
Third-party API using secret key
```

The secret key should be stored on the backend as an environment variable.

Backend environment variable:

```env
PAYMENT_SECRET_KEY=sk_live_123456
```

Backend usage:

```java
@Value("${PAYMENT_SECRET_KEY}")
private String paymentSecretKey;
```

Fix steps:

1. Remove the key from frontend code.
2. Move secret API calls to backend.
3. Store the key in backend environment variables.
4. Add `.env` to `.gitignore`.
5. Rotate the exposed key from the provider dashboard.
6. Redeploy frontend and backend.
7. Review Git history if the key was committed.

Prevention:

- Never commit secrets to GitHub.
- Use environment variables.
- Keep private keys only on backend.
- Use code reviews before deployment.
- Use restricted keys where possible.

Frontend environment variables are not a safe place for secrets if the value is used in browser code.

---

## 20. A team is preparing to release their application to production. What checklist will you follow for final testing? How will you ensure frontend-backend integration is working correctly?

Before releasing an application to production, the team should follow a final testing checklist. This ensures that the application is functional, integrated, secure, and ready for users.

Final testing checklist:

```text
Build testing
      |
      v
Functional testing
      |
      v
Integration testing
      |
      v
Error handling testing
      |
      v
Performance testing
      |
      v
Production release
```

Checklist:

- Application builds successfully.
- Environment variables are configured.
- Frontend uses production backend URL.
- Backend is deployed and running.
- CORS allows deployed frontend domain.
- Login and logout work if present.
- CRUD operations work.
- Forms validate data.
- API errors are handled properly.
- Page refresh works on deployed routes.
- Browser console has no major errors.
- Network tab shows correct API URLs.
- Performance is acceptable.
- No secrets are exposed in frontend code.

Frontend-backend integration testing:

1. Open the deployed frontend URL.
2. Perform real user actions such as login, create, update, delete, and fetch data.
3. Check browser Network tab.
4. Verify requests go to production backend, not `localhost`.
5. Verify response status codes and JSON data.
6. Check backend logs if any API fails.

Integration flow:

```text
User action on frontend
        |
        v
API request to backend
        |
        v
Backend processes request
        |
        v
Database updates or returns data
        |
        v
Frontend displays result
```

This checklist reduces the chances of production failures.

---

## 21. A project requires separate configurations for development and production environments. How will you use environment variables to manage this setup? How will you ensure correct environment selection during deployment?

Separate configurations are needed because development and production environments use different API URLs, database connections, and keys.

Example:

```text
Development backend:
http://localhost:8080/api

Production backend:
https://api.example.com/api
```

Development `.env`:

```env
VITE_API_BASE_URL=http://localhost:8080/api
```

Production environment variable in Vercel/Netlify:

```env
VITE_API_BASE_URL=https://api.example.com/api
```

Frontend usage:

```js
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;
```

Environment selection flow:

```text
Local development
      |
      v
Uses local .env file

Production deployment
      |
      v
Uses hosting platform environment variables
```

How to ensure correct environment:

- Use different `.env` files for local development.
- Do not hardcode URLs in code.
- Configure production variables in Netlify/Vercel dashboard.
- Check build logs if variables are missing.
- Verify the actual API URL in browser Network tab.
- Use clear variable names.
- Redeploy after changing environment variables.

Backend can also use profiles:

```properties
spring.profiles.active=prod
```

Mental model:

```text
Code stays same.
Configuration changes by environment.
```

This makes deployment safer and avoids accidentally calling local APIs from production.

---

## 22. Evaluate different performance optimization techniques and justify which are most effective for large-scale applications.

Performance optimization improves speed, scalability, and user experience. In large-scale applications, optimization must be applied at frontend, backend, database, and deployment levels.

Frontend techniques:

- Minify JavaScript and CSS.
- Use code splitting.
- Lazy load routes and images.
- Compress images.
- Remove unused dependencies.
- Cache static assets.

Backend techniques:

- Optimize API logic.
- Use caching for repeated data.
- Compress responses.
- Avoid unnecessary processing.
- Use pagination for large responses.

Database techniques:

- Add indexes on frequently searched columns.
- Avoid fetching unnecessary data.
- Use pagination.
- Optimize queries.

Deployment techniques:

- Use CDN for static assets.
- Deploy closer to users.
- Use HTTPS and HTTP/2 where supported.
- Monitor performance metrics.

Performance mental model:

```text
Slow app?
   |
   +-- Large frontend bundle?
   +-- Slow API?
   +-- Slow database query?
   +-- Too much network latency?
   +-- Repeated unnecessary requests?
```

Most effective techniques for large-scale applications:

- CDN hosting for frontend assets because it improves global delivery.
- Code splitting because users download only needed code.
- Database indexing because slow queries affect many users.
- Caching because repeated requests can be served faster.
- Pagination because large data responses slow down frontend and backend.
- Monitoring because performance must be measured continuously.

Justification: Large-scale applications handle many users and large data. Therefore, reducing network size, avoiding repeated computation, and improving database response time are the most effective optimizations.

---

## 23. Design a deployment workflow for a full-stack application ensuring secure API communication and optimal performance.

A secure and optimized full-stack deployment workflow should cover frontend deployment, backend deployment, environment configuration, CORS, HTTPS, testing, and monitoring.

Workflow:

```text
Source code in GitHub
        |
        v
Frontend deployed on Netlify/Vercel
        |
        v
Backend deployed on cloud platform
        |
        v
Environment variables configured
        |
        v
HTTPS and CORS configured
        |
        v
Final testing completed
        |
        v
Production release
```

Frontend:

- Build command: `npm run build`.
- Output folder: `dist` or `build`.
- Configure API base URL using environment variables.
- Enable route redirects for single-page apps if required.

Backend:

- Deploy API to cloud server.
- Configure database variables.
- Store secrets in environment variables.
- Enable HTTPS.
- Configure CORS for deployed frontend domain.

Secure API communication:

```text
Frontend -> HTTPS -> Backend API -> Database
```

Security practices:

- Do not expose secret keys in frontend.
- Use HTTPS for production.
- Restrict CORS origins.
- Use authentication for protected APIs.
- Keep production credentials separate.

Performance practices:

- Compress assets.
- Use CDN.
- Optimize API queries.
- Use caching where suitable.
- Monitor response time and errors.

Final testing:

- Test all major user flows.
- Check browser console and Network tab.
- Verify production API URL.
- Test error handling.
- Confirm deployment logs are clean.

This workflow ensures secure communication and stable performance after deployment.

---

## 24. Evaluate the impact of improper environment variable configuration on application security and functionality.

Improper environment variable configuration can break application functionality and create security risks. Environment variables often control API URLs, database credentials, third-party keys, and production settings.

Functional impact:

- Frontend may call the wrong backend URL.
- Production app may call `localhost`.
- API requests may fail.
- Build may fail if required variables are missing.
- Login, payment, or data fetching may stop working.
- App may connect to the wrong database.

Security impact:

- Secret keys may be exposed in frontend code.
- Production credentials may be committed to GitHub.
- Development keys may be used in production.
- Incorrect CORS settings may allow unwanted origins.
- Wrong backend URL may send data to an unsafe server.

Example:

```text
VITE_API_BASE_URL=http://localhost:8080/api
```

If this is used in production, users' browsers will try to call their own local machines instead of the real backend.

Impact flow:

```text
Wrong environment variable
        |
        v
Wrong API/config used in production
        |
        v
Feature failure or security exposure
        |
        v
Poor user experience and risk
```

Prevention:

- Keep separate development and production variables.
- Validate environment variables before deployment.
- Do not commit `.env` files with secrets.
- Configure variables in hosting platform dashboard.
- Redeploy after changes.
- Check actual API URLs in browser Network tab.
- Keep private secrets only on backend.

Environment variables are small configuration values, but incorrect values can cause major production failures.

---

## 25. Analyze the differences between Netlify and Vercel for deploying full-stack applications. Which would you recommend and why?

Netlify and Vercel are modern deployment platforms commonly used for frontend applications and serverless functions. Both support Git-based deployment, environment variables, custom domains, and automatic builds.

Comparison:

| Point | Netlify | Vercel |
|---|---|---|
| Main focus | Static sites and frontend apps | Frontend apps, especially Next.js |
| Git deployment | Supported | Supported |
| Environment variables | Supported | Supported |
| Serverless functions | Supported | Supported |
| Preview deployments | Supported | Supported |
| CDN hosting | Supported | Supported |
| Best fit | React, Vue, static sites, JAMstack | Next.js, React, full-stack frontend frameworks |
| Routing support | Uses redirects configuration | Strong framework-aware routing |

Netlify advantages:

- Simple for static and React apps.
- Easy form handling and redirects.
- Good for Vite and Create React App deployments.
- Straightforward UI.

Vercel advantages:

- Excellent for Next.js applications.
- Strong preview deployment workflow.
- Good serverless and edge function support.
- Simple integration with GitHub.

Recommendation:

For a normal React frontend with a separate Spring Boot backend, either Netlify or Vercel is suitable. I would recommend Vercel if the project uses Next.js or needs framework-aware routing and preview deployments. I would recommend Netlify if the project is a simple React/Vite static frontend and the team wants straightforward deployment.

Decision mental model:

```text
Next.js project?
      |
     Yes -> Vercel
      |
     No, simple React static app?
      |
     Netlify or Vercel both work
```

For this FSD context, the best choice depends on project type. Both platforms can integrate with a Spring Boot backend through environment variables and CORS configuration.

---

## 26. Evaluate the impact of improper environment variable configuration on application security and functionality.

Improper environment variable configuration affects both functionality and security because deployment settings control how the application connects to external systems.

Functional problems:

- Missing API URL causes fetch requests to fail.
- Wrong API URL sends requests to an incorrect server.
- Missing build-time variable causes blank pages or runtime errors.
- Incorrect database URL can connect backend to the wrong database.
- Incorrect mode can run production with development settings.

Security problems:

- Exposed API keys can be stolen.
- Production secrets may be accidentally pushed to GitHub.
- Public frontend variables may reveal sensitive service details.
- Wrong CORS origin may allow unauthorized websites to access APIs.
- Debug settings may expose internal error information.

Example:

```js
const API_URL = "https://dev-api.example.com";
```

If this is hardcoded and deployed to production, production users may use the development backend.

Better approach:

```js
const API_URL = import.meta.env.VITE_API_BASE_URL;
```

Impact diagram:

```text
Bad env configuration
        |
        +-- API failure
        +-- Wrong database
        +-- Exposed secrets
        +-- CORS issues
        +-- Broken deployment
```

Best practices:

- Use environment variables for all deployment-specific values.
- Keep secrets on backend only.
- Use `.gitignore` for local `.env` files.
- Configure production variables in hosting dashboard.
- Verify variables before deployment.
- Redeploy after changing variables.
- Use different values for dev, test, and production.

Thus, proper environment variable configuration is essential for safe and functional production deployment.

---

## 27. Propose a strategy to ensure seamless integration between frontend and backend in a production environment.

Seamless frontend-backend integration means that the deployed frontend can reliably communicate with the deployed backend without URL errors, CORS errors, data mismatch, or authentication problems.

Strategy:

```text
Define API contract
      |
      v
Deploy backend
      |
      v
Configure frontend API URL
      |
      v
Configure CORS
      |
      v
Test complete user flows
      |
      v
Monitor production
```

Steps:

1. Define API endpoints clearly.

Example:

```text
GET /api/products
POST /api/orders
```

2. Use environment variables for backend URL.

```env
VITE_API_BASE_URL=https://api.example.com/api
```

3. Use a centralized API service in frontend.

```js
const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;
```

4. Configure backend CORS.

```java
.allowedOrigins("https://frontend.example.com")
```

5. Use consistent JSON request and response formats.

6. Test using browser Network tab and backend logs.

7. Handle errors in frontend.

8. Monitor API performance after deployment.

Integration mental model:

```text
Frontend and backend should agree on:
  URL
  HTTP method
  Request body
  Response format
  Error format
  Authentication method
```

This strategy ensures that frontend and backend work together correctly in production.

---

## 28. A startup is building a full-stack application with a React frontend and Node.js backend. They are considering Netlify and Vercel for deployment. Analyze the suitability of both platforms for this project.

For a startup with a React frontend and Node.js backend, both Netlify and Vercel can be suitable, especially if the frontend is static and the backend is deployed separately or through serverless functions.

Architecture option:

```text
React frontend -> Netlify/Vercel
Node.js backend -> Render/AWS/Railway/Vercel functions
Database -> Cloud database
```

Netlify suitability:

- Good for React static frontend.
- Supports automatic Git deployment.
- Supports environment variables.
- Supports serverless functions.
- Easy redirects and build configuration.

Vercel suitability:

- Excellent for React and Next.js.
- Strong preview deployments.
- Supports serverless functions.
- Good developer experience.
- Better choice if the startup may move to Next.js.

Comparison:

| Requirement | Netlify | Vercel |
|---|---|---|
| React frontend | Good | Good |
| Git auto-deploy | Yes | Yes |
| Environment variables | Yes | Yes |
| Serverless backend | Yes | Yes |
| Next.js support | Supported | Excellent |
| Preview deployments | Yes | Excellent |

Recommendation:

- If the project is a simple React frontend with Node.js backend deployed separately, either Netlify or Vercel works well.
- If the team uses Next.js or wants strong preview deployments, Vercel is a better choice.
- If the team wants simple static hosting and easy redirects, Netlify is also a strong option.

Startup decision mental model:

```text
Need fastest simple React static deployment?
        |
        v
Netlify or Vercel

Using Next.js or serverless-first workflow?
        |
        v
Vercel
```

The final choice should depend on the framework, team familiarity, backend hosting plan, and expected scaling needs.

---

## 29. An application deployed on Vercel experiences a sudden spike in traffic during a marketing campaign. Evaluate how the system handles scaling under high load.

When a Vercel-deployed frontend experiences a sudden traffic spike, static frontend assets are usually served through Vercel's global CDN. This helps handle high traffic efficiently for HTML, CSS, JavaScript, and image assets.

Frontend scaling:

```text
Many users
   |
   v
Vercel CDN
   |
   v
Static assets served from edge locations
```

Vercel can handle static frontend scaling well because:

- Static assets are cached globally.
- CDN reduces load on origin servers.
- Users receive assets from nearby locations.
- Deployments are optimized for frontend delivery.

However, full application scaling also depends on backend and database.

Possible bottlenecks:

```text
Frontend CDN may scale well
        |
        v
Backend API may become slow
        |
        v
Database may become overloaded
```

High-load concerns:

- Backend API response time may increase.
- Database queries may slow down.
- Rate limits may be reached.
- Serverless functions may have cold starts.
- Third-party APIs may throttle requests.

Optimization strategy:

- Cache static assets.
- Cache repeated API responses.
- Use database indexing.
- Use pagination.
- Avoid unnecessary API calls.
- Monitor backend CPU, memory, and response times.
- Use autoscaling backend hosting if available.
- Add rate limiting for sensitive endpoints.

Evaluation:

Vercel handles frontend traffic spikes well for static or CDN-served content. But if every user action calls a backend API, the backend and database must also be prepared to scale. Therefore, high-load readiness requires both frontend CDN scaling and backend performance planning.

---

## 30. A backend API is updated, but the frontend still depends on the older version, causing failures. Analyze the challenges caused by API version mismatch. Propose a versioning and deployment strategy to handle such scenarios.

API version mismatch occurs when the backend changes its endpoint, request format, or response format, but the frontend still expects the old behavior. This can break production features.

Example:

Old response:

```json
{
  "name": "Amit"
}
```

New response:

```json
{
  "fullName": "Amit Sharma"
}
```

If the frontend still uses `user.name`, it may show `undefined`.

Challenges:

- Frontend may call removed endpoints.
- JSON field names may change.
- Required request fields may change.
- UI may show undefined data.
- Users may face failed forms or broken pages.
- Debugging becomes harder in production.

Versioning strategy:

Use API versioning in URLs:

```text
/api/v1/users
/api/v2/users
```

Keep old version temporarily:

```text
Frontend old version -> /api/v1
Frontend new version -> /api/v2
```

Deployment strategy:

```text
Step 1: Add new API version
Step 2: Keep old API working
Step 3: Update frontend to new version
Step 4: Deploy frontend
Step 5: Monitor usage
Step 6: Remove old API after migration
```

Good practices:

- Do not break existing API contracts suddenly.
- Use versioned endpoints.
- Communicate backend changes to frontend team.
- Maintain API documentation.
- Test frontend and backend together before production.
- Use staging environment before release.
- Add backward compatibility where possible.

Mental model:

```text
Backend changes should not surprise frontend.
Version old and new APIs during transition.
```

This strategy prevents production failures caused by API version mismatch.

