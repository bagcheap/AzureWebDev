# AzureWebDev
Website hoting using frontdoor
To create the tutorial for your web application with Azure Front Door, Azure Blob Storage, and an API backend, here’s a list of broad steps we will need to follow:

### 1. **Setting Up Azure Storage for Front-End Static Content**
   - **Goal:** Host the static files (HTML, CSS, JavaScript) for the front end in Azure Blob Storage.
   
### 2. **Configuring Azure Front Door for CDN and Load Balancing**
   - **Goal:** Use Azure Front Door to route and balance traffic, as well as serve the static content globally.

### 3. **Building the Backend API for Data (Stock Prices)**
   - **Goal:** Create a backend API that will fetch stock prices (e.g., from Yahoo Finance) and serve it to the front end.
   
### 4. **Connecting Front End to Backend API**
   - **Goal:** Integrate the front-end static site with the backend API to display dynamic data like stock prices.
   
### 5. **Deploying and Testing the Web Application**
   - **Goal:** Ensure the front-end and API are working together, hosted properly, and test with real data.
   
### 6. **Setting Up Security (Optional)**
   - **Goal:** Secure the API and storage using Azure security services (e.g., CORS, Azure AD, or API Keys).

---

Now, let's drill down into each step. Do you want to proceed with step-by-step details, or would you like to focus on a specific area first?

Here’s a detailed breakdown of **Step 1: Setting Up Azure Storage for Front-End Static Content**:

### **Step 1: Setting Up Azure Storage for Front-End Static Content**

#### **1.1. Create an Azure Storage Account**
   - **1.1.1.** Log in to the [Azure Portal](https://portal.azure.com).
   - **1.1.2.** Navigate to "Storage accounts" and click "Create."
   - **1.1.3.** Choose the following:
     - **Subscription:** Select your subscription.
     - **Resource Group:** Create a new or select an existing one.
     - **Storage account name:** Choose a globally unique name (e.g., `mywebappstorage`).
     - **Region:** Choose the region closest to your users.
     - **Performance:** Standard or Premium, based on your needs (Standard is common).
     - **Redundancy:** Choose the redundancy (LRS, GRS, etc.).
   - **1.1.4.** Click "Review + Create" and then "Create."

#### **1.2. Create a Blob Container for Static Files**
   - **1.2.1.** Once the storage account is created, go to the "Overview" of your storage account.
   - **1.2.2.** Under "Data storage," click on "Containers."
   - **1.2.3.** Click "+ Container" to create a new blob container.
     - **Name:** Set a name for the container (e.g., `frontend`).
     - **Public access level:** Choose "Blob" to allow anonymous public read access to the files (or set this to "Private" if you plan to secure it later).
   - **1.2.4.** Click "Create."

#### **1.3. Upload Front-End Files to the Blob Container**
   - **1.3.1.** Once the container is created, navigate to it.
   - **1.3.2.** Click "Upload" and select the front-end files (HTML, CSS, JavaScript, images, etc.).
   - **1.3.3.** Review and click "Upload" to store the files in the container.
   
#### **1.4. Configure Static Website Hosting**
   - **1.4.1.** In your storage account, go to "Static website" (under "Data management").
   - **1.4.2.** Enable "Static website" hosting.
   - **1.4.3.** Set the following:
     - **Index document name:** Specify your main HTML file (e.g., `index.html`).
     - **Error document path:** Specify the path to an error page (e.g., `404.html`).
   - **1.4.4.** Click "Save." Azure will provide a "Primary endpoint" URL where your static content is now accessible.
   
#### **1.5. Test the Static Site**
   - **1.5.1.** Copy the "Primary endpoint" URL.
   - **1.5.2.** Paste the URL in your browser to ensure that the static website is accessible and displays your front-end content.

---

Do you want to proceed with the sub-steps for Step 2 (Configuring Azure Front Door) next, or do you want to dive into any specific details in Step 1?

Here’s the breakdown of **Step 2: Configuring Azure Front Door for CDN and Load Balancing**:

### **Step 2: Configuring Azure Front Door for CDN and Load Balancing**

#### **2.1. Create an Azure Front Door**
   - **2.1.1.** Log in to the [Azure Portal](https://portal.azure.com).
   - **2.1.2.** Navigate to "Front Door and CDN Profiles" and click "+ Create."
   - **2.1.3.** Choose the following:
     - **Subscription:** Select your subscription.
     - **Resource Group:** Use the same resource group as your storage account or create a new one.
     - **Name:** Give your Front Door a name (e.g., `mywebappfrontdoor`).
     - **Tier:** Select "Standard" for basic use or "Premium" for advanced security and routing.
   - **2.1.4.** Click "Review + Create" and then "Create."

#### **2.2. Configure Frontend Host (Domain)**
   - **2.2.1.** Once the Front Door instance is created, go to the "Frontend/domains" section.
   - **2.2.2.** Click "+ Add a frontend host."
   - **2.2.3.** Specify your custom domain or use the default one generated by Azure (e.g., `mywebappfrontdoor.azurefd.net`).
   - **2.2.4.** If using a custom domain, follow the instructions to validate ownership through DNS.
   - **2.2.5.** Configure HTTPS by enabling "Use a custom certificate" if you have one, or select Azure-managed certificates for automatic HTTPS provisioning.
   - **2.2.6.** Save the frontend host settings.

#### **2.3. Configure Backend Pool**
   - **2.3.1.** Navigate to the "Backend pools" section.
   - **2.3.2.** Click "+ Add a backend pool."
   - **2.3.3.** Set up the following for your backend pool:
     - **Name:** Provide a name for the backend pool (e.g., `mywebappbackendpool`).
     - **Backend type:** Select "Storage" for Azure Blob Storage.
     - **Backend hostname:** Enter the URL of your blob storage static website (from Step 1.4.4, e.g., `mywebappstorage.z6.web.core.windows.net`).
   - **2.3.4.** Configure health probes if needed (this is optional for static sites).
   - **2.3.5.** Click "Add" to save the backend pool configuration.

#### **2.4. Configure Routing Rules**
   - **2.4.1.** Navigate to the "Routing rules" section.
   - **2.4.2.** Click "+ Add a routing rule."
   - **2.4.3.** Set the following configurations:
     - **Name:** Provide a name for the routing rule (e.g., `mywebapproutingrule`).
     - **Frontend hosts:** Select the frontend host configured earlier.
     - **Backend pool:** Choose the backend pool created earlier (which points to the blob storage).
     - **Accepted protocols:** Select both HTTP and HTTPS (prefer HTTPS for security).
   - **2.4.4.** Save the routing rule.

#### **2.5. Verify Azure Front Door Configuration**
   - **2.5.1.** After saving all configurations, go to the "Overview" section of Azure Front Door.
   - **2.5.2.** Copy the Front Door endpoint URL (e.g., `mywebappfrontdoor.azurefd.net`).
   - **2.5.3.** Open the URL in your browser and verify that your static website (hosted on blob storage) is being served through Azure Front Door.

---

Would you like to continue with **Step 3: Building the Backend API** next, or do you want to focus on more details of Step 2 before moving forward?

Here’s the detailed breakdown of **Step 3: Building the Backend API for Data (Stock Prices)**:

### **Step 3: Building the Backend API for Data (Stock Prices)**

#### **3.1. Choose a Backend Framework for the API**
   - **3.1.1.** Decide on a backend framework for your API (e.g., Python with Flask, Node.js with Express, or any language of choice).
   - **3.1.2.** Set up your development environment:
     - For **Python/Flask**, ensure you have Python installed and set up a virtual environment.
     - For **Node.js/Express**, ensure you have Node.js installed and initialize a new project with `npm init`.

#### **3.2. Fetch Stock Price Data**
   - **3.2.1.** Choose a data source for stock prices:
     - **Yahoo Finance API:** This is a popular option, but you can use other APIs like Alpha Vantage or IEX Cloud.
   - **3.2.2.** Integrate the API to fetch stock prices:
     - For Yahoo Finance, use the `yfinance` package for Python or other available Node.js modules.
   
   **Example (Python/Flask with `yfinance`):**
   ```python
   import yfinance as yf
   from flask import Flask, jsonify

   app = Flask(__name__)

   @app.route('/api/stock/<ticker>', methods=['GET'])
   def get_stock_price(ticker):
       stock = yf.Ticker(ticker)
       data = stock.history(period="1d")
       price = data['Close'][0]  # Get the closing price for the last day
       return jsonify({"ticker": ticker, "price": price})

   if __name__ == '__main__':
       app.run(debug=True)
   ```
   - This API will allow you to fetch stock prices by providing a ticker symbol (e.g., `AAPL` for Apple, `GOOGL` for Google).

#### **3.3. Test the Backend API Locally**
   - **3.3.1.** Run your backend API locally to test it.
     - For Flask: Run `python app.py`.
     - For Express/Node.js: Run `node app.js` (or whichever file you created).
   - **3.3.2.** Test the API by visiting `http://localhost:5000/api/stock/AAPL` in your browser or use tools like **Postman** to ensure it's fetching stock prices correctly.

#### **3.4. Deploy the API to Azure**
   - **3.4.1.** In Azure, navigate to "App Services" and click "+ Create."
   - **3.4.2.** Fill in the following details:
     - **Resource Group:** Use the same resource group as before.
     - **Name:** Give your API a name (e.g., `stockpriceapi`).
     - **Publish:** Select "Code" if deploying a standard web app.
     - **Runtime stack:** Choose the appropriate stack (Python, Node.js, etc.).
     - **Region:** Choose a region close to your users.
   - **3.4.3.** Click "Review + Create" and then "Create."
   
#### **3.5. Push Your Code to Azure App Service**
   - **3.5.1.** In your local environment, initialize a git repository if not already done:
     ```bash
     git init
     git add .
     git commit -m "Initial commit"
     ```
   - **3.5.2.** Push the code to Azure using the Azure CLI:
     - Install the Azure CLI if needed.
     ```bash
     az login
     az webapp up --name <app-name> --resource-group <resource-group> --runtime <runtime-stack>
     ```
   - This will deploy your API to Azure.

#### **3.6. Configure the API Endpoint for Production**
   - **3.6.1.** After deployment, go to "App Services" and find your app.
   - **3.6.2.** Copy the **API URL** (e.g., `https://stockpriceapi.azurewebsites.net/api/stock/AAPL`).
   - **3.6.3.** Test the API in production to ensure it works just like it did locally.

---

This completes the backend setup. Would you like to continue with **Step 4: Connecting the Front End to the Backend API** or focus more on any sub-step in Step 3?

Here’s the detailed breakdown of **Step 4: Connecting the Front End to the Backend API**:

---

### **Step 4: Connecting the Front End to the Backend API**

#### **4.1. Update Front-End Code to Fetch Data from the API**

- **4.1.1.** Open your front-end project files (HTML, CSS, JavaScript).

- **4.1.2.** Add JavaScript code to fetch data from the backend API.

  - Use the `fetch` API or a library like Axios to make HTTP requests.

  - **Example using JavaScript `fetch`:**

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Stock Price Viewer</title>
        <script>
            async function getStockPrice() {
                const ticker = document.getElementById('tickerInput').value;
                try {
                    const response = await fetch(`https://stockpriceapi.azurewebsites.net/api/stock/${ticker}`);
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    const data = await response.json();
                    document.getElementById('priceDisplay').innerText = `The current price of ${data.ticker} is $${data.price}`;
                } catch (error) {
                    console.error('Error fetching stock price:', error);
                    document.getElementById('priceDisplay').innerText = 'Error fetching stock price.';
                }
            }
        </script>
    </head>
    <body>
        <h1>Stock Price Viewer</h1>
        <input type="text" id="tickerInput" placeholder="Enter stock ticker (e.g., AAPL)" />
        <button onclick="getStockPrice()">Get Stock Price</button>
        <p id="priceDisplay"></p>
    </body>
    </html>
    ```

  - **Notes:**

    - Replace `https://stockpriceapi.azurewebsites.net` with your actual backend API URL if different.

    - Ensure that the API endpoint matches the route defined in your backend.

#### **4.2. Handle Cross-Origin Resource Sharing (CORS)**

- **4.2.1.** Since your front end and backend are on different domains, you need to enable CORS on your backend API to allow the front end to make requests.

- **4.2.2.** **Enable CORS in Your Backend:**

  - **For Python Flask:**

    - **Install Flask-CORS:**

      ```bash
      pip install flask-cors
      ```

    - **Update your `app.py`:**

      ```python
      from flask import Flask, jsonify
      from flask_cors import CORS

      app = Flask(__name__)
      CORS(app)  # This will enable CORS for all routes

      @app.route('/api/stock/<ticker>', methods=['GET'])
      def get_stock_price(ticker):
          # Your existing code
      ```

  - **For Node.js Express:**

    - **Install CORS middleware:**

      ```bash
      npm install cors
      ```

    - **Update your `app.js`:**

      ```javascript
      const express = require('express');
      const cors = require('cors');
      const app = express();

      app.use(cors()); // Enable CORS for all routes

      app.get('/api/stock/:ticker', (req, res) => {
          // Your existing code
      });
      ```

- **4.2.3.** **Restrict CORS to Your Front-End Domain (Optional but Recommended):**

  - Instead of enabling CORS for all origins, specify your front-end domain for better security.

  - **For Flask:**

    ```python
    CORS(app, resources={r"/api/*": {"origins": "https://yourfrontdoor.azurefd.net"}})
    ```

  - **For Express:**

    ```javascript
    app.use(cors({
        origin: 'https://yourfrontdoor.azurefd.net'
    }));
    ```

  - Replace `https://yourfrontdoor.azurefd.net` with your actual Azure Front Door URL.

#### **4.3. Update API URLs in Front-End Code**

- **4.3.1.** Ensure that all API calls in your front-end code point to the correct backend API URL.

  - Replace any placeholder URLs with your actual backend API endpoint.

- **4.3.2.** If you plan to have different environments (development, staging, production), consider using configuration files or environment variables to manage API endpoints.

#### **4.4. Test the Integrated Application Locally**

- **4.4.1.** **Run Backend Locally with CORS Enabled:**

  - Start your backend API locally to test the integration.

- **4.4.2.** **Serve Front End Locally:**

  - You can use a simple HTTP server to serve your static files locally.

    - For Python 3:

      ```bash
      python -m http.server 8000
      ```

    - For Node.js (using `http-server` package):

      ```bash
      npm install -g http-server
      http-server -p 8000
      ```

- **4.4.3.** **Test the Application:**

  - Open `http://localhost:8000` in your browser.

  - Enter a stock ticker and ensure that the price is fetched and displayed correctly.

  - Check the browser console for any errors and resolve them.

#### **4.5. Upload Updated Front-End Files to Azure Blob Storage**

- **4.5.1.** **Navigate to Your Blob Container:**

  - In the Azure Portal, go to your storage account.

  - Under "Data storage," click on "Containers" and select your container (e.g., `frontend`).

- **4.5.2.** **Upload Updated Files:**

  - Click on "Upload."

  - Select your updated front-end files (HTML, CSS, JavaScript).

  - Choose "Overwrite if files already exist" to replace the old files.

  - Click "Upload."

- **4.5.3.** **Set Correct Content Types (Optional but Recommended):**

  - Ensure that each file has the correct content type to avoid issues.

    - For example, `.html` files should have `text/html`, `.css` files should have `text/css`, and `.js` files should have `application/javascript`.

  - You can set the content type when uploading or adjust it in the file properties within the Azure Portal.

#### **4.6. Test the Application via Azure Front Door**

- **4.6.1.** **Access Your Application:**

  - Visit your Azure Front Door URL (e.g., `https://yourfrontdoor.azurefd.net`).

- **4.6.2.** **Verify Front-End Functionality:**

  - Interact with your application as an end-user would.

  - Enter stock ticker symbols and ensure that the stock prices are displayed.

- **4.6.3.** **Troubleshoot Any Issues:**

  - If data isn't loading:

    - Check the browser's developer console for errors.

    - Ensure that CORS is correctly configured.

    - Verify that the API endpoint URLs are correct.

  - If styling is off:

    - Ensure that all CSS and image files are correctly uploaded and accessible.

---

This completes **Step 4: Connecting the Front End to the Backend API**.

Would you like to proceed with **Step 5: Deploying and Testing the Web Application**, or do you have any questions or need further details on any of the steps so far?

Here’s the detailed breakdown of **Step 5: Deploying and Testing the Web Application**:

---

### **Step 5: Deploying and Testing the Web Application**

#### **5.1. Finalize Front-End and Back-End Integration**

- **5.1.1.** Ensure all updates to both the front-end and back-end code are completed.
  - Verify that the front-end (HTML, CSS, JavaScript) correctly fetches data from the API.
  - Confirm that your API endpoint in the backend is responding correctly to requests for stock prices.

- **5.1.2.** Run both the front-end and back-end applications locally to test the integration one last time:
  - Visit the local version of your front-end and check if it successfully communicates with your locally running backend API.

#### **5.2. Upload the Final Front-End Files to Azure Blob Storage**

- **5.2.1.** Go to your **Blob Storage** account in the [Azure Portal](https://portal.azure.com).
  
- **5.2.2.** Navigate to the "Containers" section, choose your container (e.g., `frontend`), and upload the final version of your static files:
  - Overwrite the existing files if necessary to ensure all updates are in place.

- **5.2.3.** Verify that the content types (e.g., `.html` as `text/html`, `.css` as `text/css`, etc.) are set correctly for each file.

#### **5.3. Redeploy or Update the API on Azure App Service**

- **5.3.1.** If any changes were made to the backend API during testing, redeploy the backend to Azure App Service:
  - **Push code updates** to the Azure App Service using the Azure CLI or Git.
  - Ensure that the API URL has remained consistent and is working in production.

- **5.3.2.** After deployment, test the backend API in production by calling it directly in your browser (e.g., `https://yourbackendapi.azurewebsites.net/api/stock/AAPL`) to confirm it’s working as expected.

#### **5.4. Test the Entire Web Application via Azure Front Door**

- **5.4.1.** Open the **Azure Front Door** URL (e.g., `https://yourfrontdoor.azurefd.net`) in a browser.

- **5.4.2.** Test the web application:
  - Enter a stock ticker symbol (e.g., `AAPL`, `GOOGL`).
  - Confirm that the stock price is fetched and displayed in the front end.
  - Interact with different elements on the page to ensure everything is functioning smoothly.

#### **5.5. Test on Different Devices and Browsers**

- **5.5.1.** Open the web application on different devices (laptop, tablet, mobile) and browsers (Chrome, Safari, Firefox, Edge).
  - Ensure that the layout, responsiveness, and functionality are consistent across all platforms.

- **5.5.2.** Test for any **CORS errors** in the browser's developer console.
  - If there are any issues with fetching data, ensure that CORS is properly configured on your API for the production environment.

#### **5.6. Implement SSL/TLS (HTTPS) for Security**

- **5.6.1.** Azure Front Door provides **HTTPS** by default when using an Azure-generated domain (e.g., `https://yourfrontdoor.azurefd.net`).

- **5.6.2.** If you’re using a **custom domain**, ensure you have SSL/TLS certificates installed:
  - Either use an Azure-managed certificate (which is simpler) or upload your custom certificate.

- **5.6.3.** After implementing HTTPS, test the application by accessing it via `https://yourcustomdomain.com` or `https://yourfrontdoor.azurefd.net` to ensure SSL is functioning.

#### **5.7. Check Application Performance**

- **5.7.1.** Use browser developer tools (under the **Network** tab) to evaluate how quickly your front-end static files and API data are loading.

- **5.7.2.** Test the performance of your web application by simulating high traffic or slow network conditions to ensure your app scales well:
  - Azure Front Door should handle load balancing and routing to optimize performance.

#### **5.8. Monitor Your Application Using Azure Tools**

- **5.8.1.** In Azure, go to **App Insights** (or a similar monitoring tool) for your backend API to:
  - Monitor API response times, error rates, and overall performance.
  - Set up alerts for any potential issues (e.g., API downtime).

- **5.8.2.** Set up monitoring for your **Azure Front Door** to track traffic patterns, latency, and health.

#### **5.9. Address Any Issues and Make Final Adjustments**

- **5.9.1.** If any issues arise during testing, revisit the appropriate step:
  - Re-upload files to Blob Storage if needed.
  - Redeploy the API if any backend issues occur.

- **5.9.2.** Fine-tune the application performance, styling, and error handling to ensure a seamless user experience.

---

This completes **Step 5: Deploying and Testing the Web Application**.

Would you like to move forward with **Step 6: Setting Up Security**, or do you need additional details for any of the previous steps?

Here’s the detailed breakdown of **Step 6: Setting Up Security**:

---

### **Step 6: Setting Up Security (Optional but Recommended)**

#### **6.1. Secure the Front-End (Blob Storage) with Shared Access Signatures (SAS)**
   - **6.1.1.** If your static content should not be publicly accessible, you can secure your Blob Storage using **Shared Access Signatures (SAS)** to control access.
     - **Go to your Storage Account** in Azure Portal.
     - Navigate to **Shared access signature** under "Security + networking."
     - Configure the following:
       - **Allowed services:** Choose "Blob."
       - **Allowed resource types:** Container and Object.
       - **Allowed permissions:** Select appropriate permissions (e.g., "Read" for read-only access).
       - **Start and expiry time:** Set the valid time period for the SAS.
     - Click "Generate SAS token and URL."
   - **6.1.2.** Use the generated SAS URL when accessing the front-end files, ensuring they are accessible only within the defined time window and with the specified permissions.

#### **6.2. Enable HTTPS on Azure Front Door**
   - **6.2.1.** Azure Front Door automatically enables HTTPS for traffic when using an Azure-provided domain. However, if you use a **custom domain**, follow these steps to secure the connection:
     - Go to **Front Door** in Azure Portal.
     - In the **Frontend/domains** section, select your custom domain.
     - Enable **HTTPS** and choose whether to use **Azure-managed certificates** (recommended for simplicity) or upload your own certificates.
   - **6.2.2.** Azure Front Door will automatically enforce SSL/TLS encryption on all incoming traffic to ensure secure communication.

#### **6.3. Secure the Backend API**
   - **6.3.1.** If your API handles sensitive or private data, secure the API by limiting access and requiring authentication.
     - **Option 1:** Use **API keys** to authenticate requests. Only authorized clients with valid API keys can access your API.
       - Generate and validate API keys in your backend logic.
     - **Option 2:** Use **Azure Active Directory (Azure AD)** to secure API endpoints.
       - Set up **Azure AD** to authenticate users and allow only authorized requests to your API.
   - **6.3.2.** Implement rate limiting to prevent abuse of the API.
     - Set up restrictions on the number of requests a user or IP address can make within a certain period.

#### **6.4. Enable CORS for Front-End and API**
   - **6.4.1.** **Cross-Origin Resource Sharing (CORS)** is essential for security when your front-end and back-end are hosted on different domains.
     - Ensure you have restricted CORS settings to allow only the necessary origins (e.g., your Azure Front Door domain) in your backend API to prevent malicious websites from making unauthorized requests.

   - **6.4.2.** If your front-end files are stored in Azure Blob Storage, you can configure CORS for the Blob service:
     - Go to **Storage Account** → **CORS** settings.
     - Set allowed origins (e.g., `https://yourfrontdoor.azurefd.net`), methods (e.g., GET), and allowed headers.

#### **6.5. Set Up Firewalls and Network Restrictions (Optional)**
   - **6.5.1.** Use **Azure Storage Firewalls** to limit who can access your Blob Storage containers.
     - In the **Storage Account**, go to **Firewalls and virtual networks**.
     - Allow access only from specific IP addresses or virtual networks, preventing unauthorized access.

   - **6.5.2.** Similarly, for the **App Service** hosting your API, restrict network access to only the IPs or services that require access:
     - In your **App Service**, go to **Networking** → **Access restrictions** and specify the allowed IP ranges.

#### **6.6. Enable Logging and Monitoring for Security**
   - **6.6.1.** Set up **Azure Monitor** and **Application Insights** to track and log API requests, errors, and potential security breaches.
     - Monitor API traffic and usage patterns for unusual activity.
     - Set up alerts for potential threats like DDoS attacks, unusually high traffic, or unauthorized access attempts.

   - **6.6.2.** Enable **Azure Storage Analytics** to log read, write, and delete operations on your Blob Storage. This helps track any suspicious activity.

#### **6.7. Configure Access Control with Azure RBAC**
   - **6.7.1.** Use **Azure Role-Based Access Control (RBAC)** to manage permissions for accessing and managing your Azure resources (e.g., Blob Storage, App Service).
     - In the Azure Portal, go to **Access control (IAM)** for the relevant resource.
     - Assign roles to users or applications based on the principle of **least privilege** (e.g., granting only the required access).
   - **6.7.2.** Limit access to sensitive areas (e.g., blob storage containers or API configurations) to ensure only authorized individuals can modify or view the resources.

#### **6.8. Encrypt Data in Transit and at Rest**
   - **6.8.1.** Ensure that all communication between your front-end and back-end is encrypted using HTTPS.
   - **6.8.2.** Enable **encryption at rest** for your Blob Storage and App Service:
     - By default, Azure Blob Storage and Azure SQL databases are encrypted using **Azure Storage Service Encryption (SSE)**. However, verify that it's enabled for your specific resources.

#### **6.9. Enable DDoS Protection (Optional but Recommended)**
   - **6.9.1.** To protect your web application from Distributed Denial of Service (DDoS) attacks, enable **Azure DDoS Protection**:
     - Go to **Azure DDoS Protection** settings in your Azure portal and configure it to protect the necessary resources.
   - **6.9.2.** Azure Front Door also provides built-in DDoS protection by distributing traffic across multiple regions, mitigating attacks before they reach your back-end services.

#### **6.10. Test Your Security Setup**
   - **6.10.1.** Perform a thorough test of your security configuration:
     - Test API access and verify that unauthorized requests are denied.
     - Use browser developer tools to ensure CORS is working correctly and no unintentional resources are accessible.
     - Test secure HTTPS access across your front-end and back-end services.
   
   - **6.10.2.** Consider performing a **security audit** or using third-party tools to scan for vulnerabilities in your web application.

---

This completes **Step 6: Setting Up Security**.

Let me know if you'd like to explore more specific details on any of these security aspects or need help implementing a particular step.

Here's a more detailed and example-driven version of **Step 6: Setting Up Security**, using realistic values and configurations to clarify each concept:

---

### **Step 6: Setting Up Security (with Examples)**

---

#### **6.1. Secure the Front-End (Blob Storage) with Shared Access Signatures (SAS)**

**Scenario**: You want to restrict access to the static files hosted in your Azure Blob Storage so that only authorized users or services can access them.

- **6.1.1.** Go to your Storage Account in the [Azure Portal](https://portal.azure.com).
- **6.1.2.** Navigate to **Shared access signature** under "Security + networking."
- **6.1.3.** Configure the following:

    - **Allowed services:** `Blob`
    - **Allowed resource types:** `Container` and `Object`
    - **Allowed permissions:** `Read`
    - **Start and expiry time:** Set the access duration for 24 hours (start now, and expire in 24 hours).
  
    Example values:
    - Start time: `2024-09-20T00:00Z`
    - Expiry time: `2024-09-21T00:00Z`
  
    - **Allowed IP addresses:** Enter specific IP addresses that can access this blob (e.g., `123.45.67.89`).
    - **Protocol:** Choose `HTTPS only` to ensure secure access.

- **6.1.4.** Click **Generate SAS token and URL**.
- **6.1.5.** Use the generated **Blob SAS URL** when accessing the front-end files, which now have time-limited, IP-restricted access.

---

#### **6.2. Enable HTTPS on Azure Front Door**

**Scenario**: You want all traffic to your application to be encrypted with HTTPS to protect user data.

- **6.2.1.** In the Azure Portal, go to your **Azure Front Door** instance.
- **6.2.2.** In the **Frontend/domains** section, select your domain (e.g., `mywebappfrontdoor.azurefd.net`).
- **6.2.3.** Scroll to the **Custom domain HTTPS** section.
  - If using a custom domain (e.g., `www.mysecureapp.com`), ensure that **HTTPS** is enabled.
  
    - **SSL/TLS type:** Choose **Azure-managed certificate**.
    - Click **Save**.

- **6.2.4.** After setting this up, your domain (e.g., `https://www.mysecureapp.com`) will now be served over HTTPS, providing encryption for all user data.

---

#### **6.3. Secure the Backend API with API Keys**

**Scenario**: You want to protect your backend API so that only clients with an authorized API key can access stock price data.

- **6.3.1.** Add API key authentication to your backend.

  - **For Flask (Python):**
  
    - Install the `Flask` API Key extension:
  
      ```bash
      pip install flask
      ```

    - Update your `app.py` to include an API key check:
  
      ```python
      from flask import Flask, request, jsonify

      app = Flask(__name__)

      # Define an API key for authorized access
      API_KEY = 'mysecretapikey123'

      @app.route('/api/stock/<ticker>', methods=['GET'])
      def get_stock_price(ticker):
          # Get the API key from the request headers
          api_key = request.headers.get('X-API-KEY')

          # Check if the provided API key is correct
          if api_key != API_KEY:
              return jsonify({'error': 'Unauthorized'}), 401

          # Process stock price fetching here
          stock_price = 150  # Simulate stock price retrieval
          return jsonify({'ticker': ticker, 'price': stock_price})
      
      if __name__ == '__main__':
          app.run(debug=True)
      ```

  - **6.3.2.** Now, only clients that send the correct API key in the `X-API-KEY` header can access your API.
    
    Example API request:

    ```bash
    curl -H "X-API-KEY: mysecretapikey123" https://mywebappapi.azurewebsites.net/api/stock/AAPL
    ```

---

#### **6.4. Enable CORS for the Front-End and API**

**Scenario**: Your API is hosted on `https://mywebappapi.azurewebsites.net`, and your front end is on `https://mywebappfrontdoor.azurefd.net`. You need to allow CORS so the front-end can make requests to the API.

- **6.4.1.** Add CORS configuration to your backend API.

  - **For Flask (Python):**
  
    - Install the `Flask-CORS` package:
  
      ```bash
      pip install flask-cors
      ```

    - Update your `app.py`:
  
      ```python
      from flask_cors import CORS

      app = Flask(__name__)
      CORS(app, resources={r"/api/*": {"origins": "https://mywebappfrontdoor.azurefd.net"}})
      ```

  - **For Node.js (Express):**

    - Install the `cors` package:

      ```bash
      npm install cors
      ```

    - Update your `app.js`:

      ```javascript
      const cors = require('cors');
      const express = require('express');
      const app = express();

      // Allow only requests from the front-end domain
      app.use(cors({
          origin: 'https://mywebappfrontdoor.azurefd.net'
      }));
      ```

- **6.4.2.** Now, your API only accepts requests from `https://mywebappfrontdoor.azurefd.net`.

---

#### **6.5. Set Up Azure Storage Firewall and Network Restrictions**

**Scenario**: You want to restrict access to your Blob Storage so that only requests from Azure Front Door can access the static files.

- **6.5.1.** In your Storage Account, go to **Firewalls and virtual networks** under "Security + networking."

- **6.5.2.** Configure the following:
  - **Allow access from**: Selected networks.
  - **Virtual networks**: Add your Azure Virtual Network (if applicable).
  - **Firewall rules**: Add specific IP ranges (e.g., the IPs of your trusted networks).

- **6.5.3.** Save the settings to ensure that only the allowed networks can access your Blob Storage.

---

#### **6.6. Enable Logging and Monitoring for Security**

**Scenario**: You want to log all access and operations performed on your API and Blob Storage to detect any suspicious activity.

- **6.6.1.** In the Azure Portal, go to **Application Insights** for your API.
  - Set up monitoring for **API performance** and **requests** to track the usage of your backend.
  
    Example setup:
    - Monitor failed requests (e.g., unauthorized access attempts).
    - Track slow API response times.

- **6.6.2.** For **Blob Storage**, enable logging:
  - Go to your Storage Account in Azure Portal.
  - Under **Monitoring**, click on **Diagnostics settings**.
  - Enable logging for **Read, Write, Delete** operations.
  - Set up an output to store logs in another storage account or send logs to Azure Monitor.

---

#### **6.7. Configure Access Control with Azure RBAC**

**Scenario**: You want to ensure only certain users or roles can manage or view your Azure resources.

- **6.7.1.** In the Azure Portal, go to **Access control (IAM)** for your storage account or API.
- **6.7.2.** Click on **Add role assignment** and assign the appropriate role:
  - **Example roles**:
    - **Storage Blob Data Reader**: Allows a user to read blobs.
    - **Storage Blob Data Contributor**: Allows a user to read and write blobs.
  - **User/Group**: Add the relevant user or service principal.

    Example: Assign `Storage Blob Data Reader` to `app@mycompany.com` to allow them to read from Blob Storage but not modify anything.

---

#### **6.8. Encrypt Data in Transit and at Rest**

**Scenario**: You want to ensure all data between users and your web app is encrypted and that data stored in Azure Blob Storage is encrypted at rest.

- **6.8.1.** For **data in transit**, ensure HTTPS is enabled for both the front-end (via Azure Front Door) and backend (via Azure App Service).

- **6.8.2.** For **data at rest**, Azure Storage encrypts data by default using **Azure Storage Service Encryption (SSE)**.
  - To verify, go to your **Storage Account** in Azure.
  - Under **Encryption**, ensure that **Azure-managed keys** are enabled for data encryption.

---

#### **6.9. Enable DDoS Protection**

**Scenario**: You want to protect your app from Distributed Denial of Service (DDoS) attacks.

- **6.9.1.** Azure Front Door provides basic DDoS protection, but you can enable **Azure DDoS Protection Standard** for enhanced protection.

- **6.9.2.** Go to **Azure DDoS Protection** in the portal, create a DDoS protection plan, and apply it to your resources (e.g., App Service and Storage Account).

---

#### **6.

10. Test Your Security Setup**

**Scenario**: You want to ensure that all security configurations are functioning as expected.

- **6.10.1.** Use tools like **Postman** or **curl** to test API access with and without API keys.
  - Example test:
    ```bash
    curl -H "X-API-KEY: wrongapikey" https://mywebappapi.azurewebsites.net/api/stock/AAPL
    ```
    The response should be `401 Unauthorized`.

- **6.10.2.** Use browser developer tools (F12) to ensure **CORS** is enabled and no unauthorized domains can access the API.

---

This version includes practical, hands-on examples that you can follow along. Let me know if you need further clarification on any step!
