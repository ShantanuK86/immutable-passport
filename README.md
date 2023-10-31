# Immutable Passport Integration Guide
### Immutable X: Introduction

- **Platform Overview:** Immutable X provides Layer 2 solutions on Ethereum, the most secure and decentralized Layer 1 blockchain.
  
- **Developer Tools:** It offers APIs and developer tools, simplifying the development of fast, scalable, and secure applications, specifically tailored for NFTs and blockchain games.

- **Key Features:**
  - Free minting of game assets.
  - Fast and affordable in-game transactions.
  - Self-custodial wallets ensuring true ownership of in-game items.
  - Global orderbook for open market item exchange.

### Benefits of Immutable X Over Traditional Wallets

- **Scalability:**
  - Unlimited scalability, enabling applications to handle a growing user base and high transaction volumes.

- **User Experience:**
  - Instant transaction confirmation for an enhanced user experience.

- **Cost-Effectiveness:**
  - Cheaper (sometimes free) transactions using 'rollups' that batch multiple transactions into a single, cost-effective transaction on Layer 1.

- **Security:**
  - Maintains Ethereum-level security by batching and settling Layer 2 transactions on Layer 1.

- **Network Effects:**
  - Leverages the network effects of Ethereum, allowing easy interaction with Layer 1 users and applications.

- **Use Cases:**
  - Ideal for NFTs, blockchain games, token and asset trading platforms, and blockchain transaction analysis tools.

### Prerequisites
Before you begin, make sure you have the following:

- Immutable Passport credentials (client ID) from Immutable Developer Hub.
- Latest version of node.
- Basic ReactJS or NextJS knowlegde

### 1.Git cloning a repository of the simple application4
You can create a simple Next.js application or clone a repository of a pre-built application. Ensure that your application is set up and running.
### 2. Registering the Application on Immutable Developer Hub
Before using Passport, you must register your application as an OAuth 2.0 client in the Immutable Developer Hub. First, you'll need to create a project and a testnet environment. Then you can navigate to the Passport config screen and create a passport client for your created environment. 
- **Application Type** - The type of your application. At the moment only the Web Application option is available. A Native option for mobile or desktop applications is coming soon.
- **Client name**	- The name you wish to use to identify your application.
- **Logout URLs** -	The URL that your users will be redirected to upon logging out of your application.
- **Callback URLs** -	The URL that your users will be redirected to once the authentication is complete. This is also where your application process the authentication response.
- **Web Origins URLs** - The URLs that are allowed to request authorisation. This field is available when you select the Native application type.
### 3. Installing and initialising the Passport client
Install the Immutable SDK by Running the following command in your project root directory.
```javascript
npm install -D @imtbl/sdk
```
Initialise the Passport client by:
```javascript
const passportInstance = new passport.Passport({
  baseConfig: new config.ImmutableConfiguration({
    environment: config.Environment.PRODUCTION,
  }),
  clientId: '<YOUR_CLIENT_ID>',
  redirectUri: 'https://example.com',
  logoutRedirectUri: 'https://example.com/logout',
  audience: 'platform_api',
  scope: 'openid offline_access email transact',
});
```
Here,
- **baseConfig**: baseConfig is the shared configuration for Immutable modules, defining the environment and other global settings.
- **clientId**: clientId is the unique identifier for your registered application in the Immutable Developer Hub.
**redirectUri**: redirectUri is the URL where users are redirected after successful authentication, matching a Callback URL in your client settings.
- **logoutRedirectUri**: logoutRedirectUri is the URL for user redirection after logging out, matching a Logout URL in your client settings.
- **audience**: audience is a string specifying the intended audience for the issued token, e.g., 'platform_api' for Immutable protocol APIs.
- **scope**: scope defines the access privileges requested, including custom scopes like 'transact' and standard OpenID Connect (OIDC) scopes such as 'openid', 'offline_access', and 'email'.
### 4. Logging in a User with Passport

To log in a user, use the following code snippet:

```javascript
import { passportProvider, fetchAuth } from "@/lib/immutable";

const fetchAuth = async () => {
  try {
    const accounts = await passportProvider.request({
      method: "eth_requestAccounts",
    });
    console.log("Connected");
    console.log(accounts);
  } catch (error) {
    console.log(error);
  }
};
```
### 5. Logging Out a User

To log out a user, use the following code:

```javascript
import { passportInstance } from "@/lib/immutable";

const handleLogout = () => {
  passportInstance.logout();
};
```
### 6. Display User Information

After a user is authenticated, you can access and display user information, including the ID token and access token.

Use the code snippet below to fetch and display user information:

```javascript
import { passportInstance } from "@/lib/immutable";

// Define a function to fetch and display user information
const displayUserInfo = async () => {
  try {
    // Fetch the user's profile information
    const userProfile = await passportInstance.getUserInfo();

    // Fetch the access token and ID token
    const accessToken = await passportInstance.getAccessToken();
    const idToken = await passportInstance.getIdToken();
    const nickname = await passportInstance.getNickname(); 
  }
};

// Call the function when needed to display user information
displayUserInfo();
```
### 7. Logging Out a User

You can use the logout function on the Passport instance.

```javascript
import { passportInstance } from "@/lib/immutable";

const handleLogout = () => {
  passportInstance.logout();
};
```

### 8.Initiate a transaction using Passport
To start a transaction using Passport, you just need to give it the required information about the transaction data and parameters.



```javascript
import { passportProvider, initiateTransaction } from "@/lib/immutable";

const initiatePassportTransaction = async (transactionData) => {
  try {
    // Use the 'initiateTransaction' function to send the transaction data
    const transactionHash = await initiateTransaction(transactionData);

    // Handle the transaction response, e.g., display the transaction hash
    console.log("Transaction Hash:", transactionHash);
  } catch (error) {
    // Handle errors when initiating the transaction
    errorHandling("Transaction initiation error", error);
  }
};

// Sample transaction data
const transactionData = {
  to: "0xYourRecipientAddress", // Recipient's Ethereum address
  value: "0.1", // Amount to send (in Ether)
  data: "<Transaction data>", // Transaction data (hex encoded)
  gas: 5000, // Gas limit

};

// Call the function to initiate the transaction with the provided data
initiatePassportTransaction(transactionData);
```
### Conclusion
So you hava finally learned how to integrate Immutable X Passport into your application.
