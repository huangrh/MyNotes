# https://medium.com/@prhmma/streamlit-login-with-azure-ad-authentication-66ebd1691858
1. go to Azure App registration and + New Registration
2. Create a docker image and run it on the cloud
3. push the docker image up in the Azure Containder Registry (ACR) and connnect it to a Web App
4. 
5. 
# https://learn.microsoft.com/en-us/answers/questions/1470782/how-to-deploy-a-streamlit-application-on-azure-app
```
To deploy a Streamlit application on Azure App Service, follow these steps:

Create an Azure App Service with B1 SKU or higher, as the free version does not support Streamlit.
Choose Python v3.10 or above for Streamlit in the App Service.
Choose Linux as the operating system for the App Service.
Make sure your code folder has a requirements.txt file with all the dependencies.
Create a bash script run.sh, and write the following command in it:
python -m streamlit run ui/app.py --server.port 8000 --server.address 0.0.0.0

# Replace ui/app.py with your application name.
- Use port 8000 because Azure App Service by default exposes only 8000 and 443 ports.  
- Open Visual Studio Code and install the Azure Extension Pack.  
Log in to Visual Studio Code with your Azure account.  
Use the Azure App Service: Deploy to Web App command in Visual Studio Code and select your App Service name.  
Wait for deployment to be finished.  
Go to the Azure portal and update the Startup Command configuration for the App Service and set the value to run.sh. You can find this configuration inside App Service > Settings > Configurations > General settings.
Wait for some seconds and visit the application URL. Congratulations! You have successfully deployed your Streamlit application to the Azure App Service.
```

# Azure App Service in 15 MINUTES | Web App Tutorial

https://www.youtube.com/watch?v=5jOvVY1G62U

# Deploy on Azure  
- https://learn.microsoft.com/en-us/answers/questions/1470782/how-to-deploy-a-streamlit-application-on-azure-app  
- https://benalexkeen.com/deploying-streamlit-applications-with-azure-app-services/
