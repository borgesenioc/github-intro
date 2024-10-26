# Running a Chrome Bot from a Cloud Server

## Overview
This project, **Running Chrome Bot from Server**, automates browser tasks using Puppeteer and Chrome. It leverages **Render** as the server host, **GitHub** for version control and deployment, and **Retool** as a control interface. This setup enables running scheduled or on-demand browser sessions, which can be triggered from Retool and managed via GitHub.

## Components

- **GitHub**: Used for code version control and deployment automation. Each push to the main branch automatically deploys updates to Render.
- **Render**: Hosts the Puppeteer service, providing the server environment needed to run Chrome instances for browser automation.
- **Retool**: Acts as a front-end control panel for triggering automation tasks and managing task parameters in real-time.

## Setup

### 1. Render Setup
   - Create a new **Render Web Service** and link it to your GitHub repository.
   - Configure **Environment Variables** in Render:
     - `NORDVPN_USERNAME`: Your NordVPN account username.
     - `NORDVPN_PASSWORD`: Your NordVPN account password.
   - Set up a **Background Worker** if using cron jobs or other scheduled tasks.

### 2. GitHub Repository
   - Add your Puppeteer automation code and push it to the repository.
   - Include a `start` script in `package.json` to launch Puppeteer with the correct configurations:
     ```json
     "scripts": {
       "start": "node index.js"
     }
     ```
   - **GitHub Actions**: Optional - Set up workflows to run tests or lint code before deployment.

### 3. Retool Integration
   - Create a Retool app to serve as a control dashboard.
   - Configure API requests in Retool to interact with your Render service endpoints, allowing you to:
     - Start automation tasks on-demand.
     - Adjust task parameters (e.g., intervals or specific URLs).
     - Monitor task results (optional: save logs to a database for display in Retool).

### 4. Puppeteer Configuration with NordVPN Proxy
   - In your Puppeteer script, set up the NordVPN proxy server for a NYC IP address. Example:
     ```javascript
     const puppeteer = require('puppeteer');

     (async () => {
       const browser = await puppeteer.launch({
         headless: true,
         args: [
           `--proxy-server=socks5://${process.env.NORDVPN_USERNAME}:${process.env.NORDVPN_PASSWORD}@us1234.nordvpn.com:1080`
         ],
       });
       const page = await browser.newPage();
       await page.goto('https://www.linkedin.com');
       await browser.close();
     })();
     ```

## Diagram

```plaintext
             +----------------+               +--------------+              +----------------+
             |                |   Code Push   |              |  API Calls   |                |
             |    GitHub      +-------------->+    Render    +------------->+     Retool     |
             |                |               | (Server)     |              | (Control Panel)|
             +----------------+               +--------------+              +----------------+
                     ^                             |
                     |       Auto Deploy           |
                     +-----------------------------+
