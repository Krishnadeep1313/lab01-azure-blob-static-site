# ☁️ Lab 01: Hosting Your First Static Website in Azure

🎥 WATCH ME COMPLETE THIS LAB HERE:

[https://www.loom.com/share/a4d0456889c942699e634698f69776ef](https://www.loom.com/share/fe98ab1cce7b4f989a8f35076f2d05b2)




## 📋 Table of Contents
- [Objective](#-objective)
- [Architecture Diagram](#-architecture-diagram)
- [Prerequisites](#-prerequisites)
- [Lab Variables](#-lab-variables)
- [Step-by-Step Instructions](#-step-by-step-instructions)
  - [Phase 1: Create the Resource Group](#phase-1-create-the-resource-group)
  - [Phase 2: Create the Storage Account](#phase-2-create-the-storage-account)
  - [Phase 3: Enable Static Website Hosting](#phase-3-enable-static-website-hosting)
  - [Phase 4: Create Your Website Files](#phase-4-create-your-website-files)
  - [Phase 5: Upload Content](#phase-5-upload-content)
  - [Phase 6: Validation & Testing](#phase-6-validation--testing)
- [Live Website](#-live-website)
- [Clean Up Resources](#-clean-up-resources)


---

## 🎯 Objective
In this lab, we will deploy our first public-facing resource in Azure. Instead of building and maintaining a complex virtual machine to host a simple website, we will use Azure Blob Storage. This introduces us to the concept of **PaaS (Platform as a Service)** and **Serverless hosting**, where you focus purely on the content, and Microsoft handles the underlying infrastructure.

## 🏗️ Architecture Diagram

```mermaid
graph LR
    User([Internet User]) -->|HTTP GET Request| URL[Primary Endpoint URL]
    URL --> SA[(Azure Storage Account)]
    
    subgraph Storage Configuration
        SA --> WebCont[$web Container]
        WebCont --> Route{Traffic Routing}
        Route -->|Root or Valid Path| Index[index.html]
        Route -->|Invalid Path| Error[404.html]
    end
    
    style User fill:#f9f,stroke:#333,stroke-width:2px
    style SA fill:#0072C6,stroke:#fff,stroke-width:2px,color:#fff
    style WebCont fill:#28a745,stroke:#fff,stroke-width:2px,color:#fff
```

## ✅ Prerequisites
- [x] Active Azure Subscription (Free Tier is fully supported).
- [x] A basic text editor (Notepad, TextEdit, or VS Code).

## ⚙️ Lab Variables (Naming Convention)
Use these naming conventions to ensure consistency throughout the deployment:
*   **Resource Group:** `rg-lab01-[yourname]`
*   **Location:** `East US`
*   **Storage Account Name:** `stlab01[yourname]` 

> **Note:** Storage account names must be globally unique across all of Azure, use only lowercase letters and numbers, and contain no special characters.

---

## 🚀 Step-by-Step Instructions

### Phase 1: Create the Resource Group
1. Log in to the [Azure Portal](https://portal.azure.com).
2. Search for **Resource Groups** in the top search bar.
3. Click **+ Create**.
4. Under the **Basics** tab:
    * **Subscription:** Select your subscription.
    * **Resource Group:** Enter `rg-lab01-[yourname]`.
    * **Region:** Select `(US) East US`.
5. Click **Review + create** -> **Create**.

### Phase 2: Create the Storage Account
1. Search for **Storage accounts** in the top search bar.
2. Click **+ Create**.
3. Under the **Basics** tab:
    * **Resource Group:** Select `rg-lab01-[yourname]`.
    * **Storage account name:** `stlab01[yourname]` (e.g., `stlab01jhante`).
    * **Region:** `(US) East US`.
    * **Performance:** `Standard`.
    * **Redundancy:** `Locally-redundant storage (LRS)`. *(This is the most cost-effective option for labs).*
4. Click **Review + create**.
5. Wait for validation to pass, then click **Create**.
6. When deployment finishes (approx. 30 seconds), click **Go to resource**.

### Phase 3: Enable Static Website Hosting
1. On the left-hand menu of your Storage Account, scroll down to the **Data management** section.
2. Click on **Static website**.
3. Toggle the switch from "Disabled" to **Enabled**.
4. **Index document name:** Enter `index.html`.
5. **Error document path:** Enter `404.html`.
6. Click **Save**.

> **CRITICAL:** Once saved, a **Primary endpoint URL** will appear (e.g., `https://stlab01krish.z13.web.core.windows.net/`). Copy this URL and save it to your notepad. This is our website's public address.

### Phase 4: Create Your Website Files
We will create two distinct HTML files: one for the main page, and one to handle bad routing requests.

#### 1. Create the Main Page (`index.html`)
Open the text editor, paste the following HTML code, and save the file to your desktop as `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>System Online | Infrastructure</title>
    <style>
        body { background-color: #0d1117; color: #58a6ff; font-family: 'Fira Code', 'Courier New', Courier, monospace; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; overflow: hidden; }
        .terminal { text-align: left; padding: 2.5rem; border: 1px solid #30363d; border-radius: 8px; background-color: #161b22; box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5); width: 80%; max-width: 600px; }
        h1 { font-size: 2rem; margin-bottom: 20px; text-shadow: 0 0 8px rgba(88, 166, 255, 0.4); }
        p { font-size: 1.1rem; color: #8b949e; line-height: 1.5; }
        .highlight { color: #7ee787; }
        .cursor { animation: blink 1s step-end infinite; }
        @keyframes blink { 50% { opacity: 0; } }
    </style>
</head>
<body>
    <div class="terminal">
        <h1>> init_azure_deployment.sh<span class="cursor">_</span></h1>
        <p>Status: <span class="highlight">[ OK ]</span> Handshake established.</p>
        <p>Connection secured. Welcome to the infrastructure zone. Nothing but clean code, optimal routing, and low latency here.</p>
        <p>Awaiting further instructions...</p>
    </div>
</body>
</html>
```

#### 2. Create the Error Page (`404.html`)
Open a new file in your text editor, paste the following HTML code, and save it to your desktop as `404.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>404 - Packet Dropped</title>
    <style>
        body { background-color: #0d1117; color: #ff7b72; font-family: 'Fira Code', 'Courier New', Courier, monospace; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
        .container { text-align: center; padding: 3rem; border: 1px solid #ff7b72; border-radius: 8px; background-color: #161b22; box-shadow: 0 10px 30px rgba(255, 123, 114, 0.15); max-width: 500px; }
        h1 { font-size: 4rem; margin: 0 0 10px 0; color: #ff7b72; }
        p { font-size: 1.2rem; color: #c9d1d9; margin-bottom: 20px; }
        .cmd { color: #8b949e; font-size: 0.9rem; background: #0d1117; padding: 10px; border-radius: 4px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>404</h1>
        <p>> ERR_DESTINATION_UNREACHABLE</p>
        <p>Looks like you hit a wall. The requested resource doesn't exist on this node.</p>
        <div class="cmd">Contact the IT guy. (Or try clearing your DNS cache, who knows) ¯\_(ツ)_/¯</div>
    </div>
</body>
</html>
```

### Phase 5: Upload Content
1. Go back to the Storage Account in the Azure Portal.
2. On the left menu, click on **Containers** (under Data storage).
3. You will see a new container named `$web`. This was created automatically when you enabled the static website feature. Click to open it. 4. Click **Upload** at the top. 5. Browse the computer and select **both** your `index.html` and `404.html` files. 6. Click **Upload**. 7. *Verification:* Ensure both files sit directly in the root of the `$web` container.

### Phase 6: Validation & Testing
1. Open a new browser tab or window.
2. Paste the **Primary endpoint URL** you copied in Phase 3 and hit Enter. You should see our custom message.
3. Next, test our custom error routing by intentionally typing a bad path at the end of your URL (e.g., `https://stlab01krish.z13.web.core.windows.net/this`). 
4. Hit Enter. You should immediately see your custom `404 - Packet Dropped` page instead of a default Azure XML error.

---

## 🌐 Live Website

👉 [*(Check out My Website!)*](https://stlab01krish.z13.web.core.windows.net/)

👉 [*(Test the Custom 404 Error Page!)*](https://stlab01krish.z13.web.core.windows.net/lol)

---

## 🧹 Clean Up Resources
*Important: Always clean up unused resources to prevent billing leakage and keep your cloud environment tidy.*

1. Navigate to **Resource Groups** in the Azure Portal.
2. Click on `rg-lab01-[yourname]`.
3. Click **Delete resource group** at the top.
4. Type the exact name of the resource group to confirm the safety check.
5. Click **Delete**.
