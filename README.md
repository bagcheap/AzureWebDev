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

