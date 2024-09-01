# ScoutWired's Got Talent - SAML Setup Guide

This guide will help you set up SAML (Security Assertion Markup Language) authentication for the ScoutWired's Got Talent application using Microsoft 365.

## Prerequisites

- A Microsoft 365 account with administrative access
- Your application registered in the Azure Active Directory

## Steps to Set Up SAML Authentication

1. **Register Your Application in Azure AD**

   - Go to the [Azure Portal](https://portal.azure.com/)
   - Navigate to "Azure Active Directory" > "App registrations"
   - Click "New registration"
   - Name your application (e.g., "ScoutWired's Got Talent")
   - Set the redirect URI to `http://localhost:3000/submissions` (or your production URL)
   - Click "Register"

2. **Configure Authentication**

   - In your app's overview page, note down the "Application (client) ID"
   - Go to "Authentication" in the left sidebar
   - Under "Platform configurations", add a Single-page application
   - Set the Redirect URI to `http://localhost:3000/submissions`
   - Under "Implicit grant and hybrid flows", check "Access tokens" and "ID tokens"
   - Save the changes

3. **Set Up API Permissions**

   - Go to "API permissions" in the left sidebar
   - Click "Add a permission"
   - Choose "Microsoft Graph" > "Delegated permissions"
   - Add the "User.Read" permission
   - Click "Add permissions"
   - Grant admin consent for these permissions

4. **Update Configuration in the Application**

   Open `src/lib/samlAuth.js` and update the following:

   ```javascript
   const msalConfig = {
     auth: {
       clientId: "YOUR_CLIENT_ID", // Replace with your Application (client) ID
       authority: "https://login.microsoftonline.com/39016543-378f-4dc5-b626-832db78c9f85", // Replace YOUR_TENANT_ID with your Azure AD Tenant ID
       redirectUri: "http://localhost:3000/submissions",
     },
     // ... rest of the configuration
   };
   ```

5. **Install Required Packages**

   The project should already have the necessary packages installed. If not, run:

   ```
   npm install @azure/msal-browser @azure/msal-react
   ```

6. **Test the Authentication**

   - Start your application
   - Navigate to the "View Submissions" page
   - You should be prompted to log in with your Microsoft 365 account
   - After successful authentication, you should be able to view the submissions

## Troubleshooting

- Ensure that your Azure AD app registration's redirect URI matches exactly with your application's URL
- Check that you've granted the necessary API permissions and admin consent
- Verify that the client ID and authority in the `msalConfig` are correct
- If you encounter CORS issues, ensure your application's URL is added to the allowed origins in the Azure portal

For more detailed information, refer to the [Microsoft Authentication Library (MSAL) documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-overview).
