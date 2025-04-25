# ASP.NET MVC Non-Core Application

This repository contains an **ASP.NET MVC** (non-core) application, configured to serve static files (such as images, CSS, and JavaScript) correctly when hosted in **IIS**.

## Project Structure

E:\ASP.NET\test <br>
├── bin  <br>
├── Controllers  <br>
├── Models  <br>
├── obj  <br>
├── Properties  <br>
├── Views <br>
├── wwwroot <br>
├── appsettings.Development.json  <br>
├── appsettings.json <br>
├── Program.cs  <br>
├── test.csproj  <br>
└── web.config

vbnet
Copy

### **`web.config` File in ASP.NET MVC (Non-Core)**

The **`web.config`** file is a key configuration file used in **ASP.NET MVC (non-core)** applications to manage settings related to the web server and application behavior. It is an XML file placed in the root of the application, and it configures various settings such as:

- **Static Content Handling**: Specifies how static files like images, CSS, and JavaScript are served, including defining MIME types for different file extensions.
- **Authentication and Authorization**: Manages user authentication and access control for different resources in the app.
- **Compilation and HTTP Runtime**: Configures how the application code is compiled and how HTTP requests are processed.
- **Custom Handlers**: Defines how custom file types or request paths are handled by the application.

This file is automatically read by IIS (Internet Information Services) to configure the application when hosted.

### **IIS Static File Configuration**

To serve static files (such as images, CSS, JavaScript) in an **ASP.NET MVC non-core** application hosted on IIS, you need to ensure proper configuration in the **`web.config`** file:

#### 1. **Enable Static Content**:
The `<staticContent>` section in `web.config` defines MIME types for various file extensions, allowing IIS to correctly identify and serve static files.

Example:
```xml
<staticContent>
  <mimeMap fileExtension=".jpg" mimeType="image/jpeg" />
  <mimeMap fileExtension=".css" mimeType="text/css" />
  <mimeMap fileExtension=".js" mimeType="application/javascript" />
</staticContent>
```
#### 2. Set Permissions:
Ensure that IIS has read permissions on the static files located in the wwwroot directory. The App Pool Identity (the IIS user) should have the necessary permissions to access the files and serve them to the client.

#### 3. Application Pool Configuration:
The application pool in IIS must be set to use the .NET Framework 4.x version for compatibility with the ASP.NET MVC (non-core) application. This ensures that the application runs correctly and IIS processes requests using the appropriate runtime version.

Steps to Set Up and Resolve 403 Forbidden Error

Step 1: Set Permissions for Static Files
- Navigate to the wwwroot folder where your static files (images, CSS, JS) are stored.

- Right-click on the folder and choose Properties.

- Go to the Security tab.

- Click Edit to modify permissions, then select the IIS App Pool Identity (e.g., IIS AppPool\YourAppPoolName) or IUSR user.

- Grant Read permissions to the selected user.

- Click Apply and OK.

Step 2: Ensure Correct MIME Types in web.config
- In the web.config, ensure that static content is properly configured to be served by IIS. For example, add MIME types for file extensions like .jpg, .css, and .js:

```xml
<staticContent>
  <mimeMap fileExtension=".jpg" mimeType="image/jpeg" />
  <mimeMap fileExtension=".jpeg" mimeType="image/jpeg" />
  <mimeMap fileExtension=".png" mimeType="image/png" />
  <mimeMap fileExtension=".css" mimeType="text/css" />
  <mimeMap fileExtension=".js" mimeType="application/javascript" />
  <mimeMap fileExtension=".ico" mimeType="image/x-icon" />
</staticContent>
```

#### Step 3: Verify Application Pool Settings in IIS
- Open IIS Manager.

- Navigate to your application and select the Application Pool it is using.

- Right-click on the application pool and select Edit Application Pool.

- Ensure that the .NET Framework version is set to .NET Framework 4.x (or the version your application is using).

- Ensure the Managed Pipeline Mode is set to Integrated.

- Click OK to apply the changes.


#### Step 4: Check IIS Handler Mappings
- In IIS Manager, select your site.

- In the Features View, double-click on Handler Mappings.

- Ensure that the correct handlers are mapped for ASP.NET and static files.

- Ensure that the StaticFile handler is present, which handles requests for static files like .css, .jpg, .js, etc.


#### Step 5: Configure Web.config for Static Files
- Ensure that your web.config is set up correctly for static content, as shown below:

``` xml
<system.webServer>
  <!-- Static Content Handling -->
  <staticContent>
    <mimeMap fileExtension=".jpg" mimeType="image/jpeg" />
    <mimeMap fileExtension=".jpeg" mimeType="image/jpeg" />
    <mimeMap fileExtension=".png" mimeType="image/png" />
    <mimeMap fileExtension=".css" mimeType="text/css" />
    <mimeMap fileExtension=".js" mimeType="application/javascript" />
    <mimeMap fileExtension=".ico" mimeType="image/x-icon" />
  </staticContent>

  <!-- Handlers to handle ASP.NET requests -->
  <handlers>
    <add name="ASP.NET" path="*.aspx" verb="*" type="System.Web.HttpForbiddenHandler" resourceType="Unspecified" />
  </handlers>
</system.webServer>
```

#### Step 6: Test and Verify
- Once the above steps are completed, restart your IIS server.

- Try accessing the application again and check if the 403 Forbidden error is resolved.

- If the issue persists, double-check the permissions and ensure the application pool identity has access to all required directories and files.