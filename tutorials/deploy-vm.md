# 1. Deploy Virtual Machine in Azure

In this section, you will learn how to set up a Ubuntu Linux Virtual Machine with an Nginx webserver, which will be publicly accessible from the internet. This will allow you to host your own website or web application on Azure. By the end of this section, you will be able to:

* Use Azure Portal.
* Create and configure a Ubuntu Linux Virtual Machine on Azure.
* Install and run Nginx webserver on your virtual machine.
* Configure a domain name for your web server and access it from the Internet.


## 1.1 Prerequisites

### 1.1.1 Azure Portal

To make sure you are correctly setup with a working subscription, make sure you can log in to the Azure portal. Go to <https://portal.azure.com> and provide your credentials. Once logged in, feel free to browse around a little bit to get to know the surroundings!


## 1.2 Deployment

### 1.2.1 Create Virtual Machine
In this section, you will learn how to create a virtual machine running Ubuntu Linux.

1) Navigate to the Azure portal at https://portal.azure.com and click on the menu.

2) Click on **Create a resource**.

![Screenshot](/images/Slide2.PNG)

3) From the popular marketplace products list click **Ubuntu Server 22.04 LTS**.

![Screenshot](/images/Slide3.PNG)

4) Before we can do a deployment we must first create a resource group click on **Create new**.

5) Name the resource group and follow the naming convention: **rg-YOUR COMPANY NAME-YOUR FIRST NAME** Click **OK**.

![Screenshot](/images/Slide4.PNG)

6) Type the name of the virtual machine,use the following naming convention: **vm-YOUR COMPANY NAME-YOUR FIRST NAME**

7) From the drop down menu choose **Sweden Central**, this is the region were we will deploy the virtual machine.

8) In the availability options, choose **No Infrastructure Redundancy Required** from the drop down menu.

9) Scroll down to the bottom of the page.

![Screenshot](/images/Slide5.PNG)

10) As a mean to authenticate to the Virtual machine we will use Password, check **Password**.

11) Lets create a user name for the virtual machine which we will use later to login with. Use username **azureuser** with lowercase.

12) Provide a strong **password** for the azureuser which you can login with later.

13) Confirm the **password**.

14) Click **Next: Disks > button** to proceed with the deployment configuration.

![Screenshot](/images/Slide6.PNG)

15) Click **Next: Networking > button** to proceed with the deployment configuration.


![Screenshot](/images/Slide7.PNG)

16) On the selected inbound ports, click on the **drop down menu**.

17) From the drop down menu choose **HTTP (80)**. This option will allow you to access your web server from the internet.

18) Click **Next: Management >** button.

![Screenshot](/images/Slide8.PNG)

19) Click **Next: Monitoring >** button.

![Screenshot](/images/Slide9.PNG)

20) Click **Next: Advanced >** button.

![Screenshot](/images/Slide10.PNG)

21) Click **Next: Tags >** button.

![Screenshot](/images/Slide11.PNG)

22) Click **Next: Review + Create** button.

![Screenshot](/images/Slide12.PNG)

23) Click **Create** button.
![Screenshot](/images/Slide13.PNG)

This will initiate the actual deployment of the Virtual Machine, the deployment process usually takes 1-3 minutes.

![Screenshot](/images/Slide14.PNG)

24) Your Virtual Machine should now be successfully deployed to Azure. Click on **Home >**.

![Screenshot](/images/Slide15.PNG)

### 1.2.2 Install Nginx Webserver.

25) Click on **Resource groups**.

![Screenshot](/images/Slide16.PNG)

26) Identify your newly created resource group from the list and click on **your resource group** called **rg-YOUR COMPANY NAME-YOUR FIRST NAME**. 

![Screenshot](/images/Slide17.PNG)

27) Within your resource group you will find all the resources that you have created for your Virtual Machine, such as IP address, hard disk, network card etc.... Lets login to the Virtual Machine. Click on the Virtual Machine named: **vm-YOUR COMPANY NAME-YOUR FIRST NAME** icon from the list.

![Screenshot](/images/Slide18.PNG)

28) Scroll down to the bottom of the page.

![Screenshot](/images/Slide19.PNG)

29) Click on **Serial Console**, this will allow us to login to the Virtual Machine in order for us to conduct operations.

![Screenshot](/images/Slide20.PNG)

You will now be prompted with a login, supply the username **azureuser** and hit **Enter** on the keyboard for the second prompt you will need to provide a password. Provide the **password** you created earlier for the Virtual Machine and hit **Enter**.

![Screenshot](/images/Slide21.PNG)

Once you are logged into the Virtual Machine, we can now install the Nginx Webserver, supply the following commands in the serial console.

````bash
sudo apt-get update -y
apt get install nginx -y
````

Congratulations you have now successfully installed Nginx Webserver on Ubuntu Linux.



### 1.2.3 Configure domain name for your Virtual Machine.

30) We now need to configure a domain name in Azure, so its easier for us to access our newly created webserver from the internet On the bread crumb menu choose your **Resource group**

![Screenshot](/images/Slide22.PNG)

31) In your resource group, click on the **public IP resource**

![Screenshot](/images/Slide23.PNG)

32) On your left hand side menu, click on **Configuration**.

![Screenshot](/images/Slide24.PNG)

33) Supply your subdomain name follow the following naming convention **Your Company Name-Your First Name**.

34) **Press Save**.

![Screenshot](/images/Slide25.PNG)

### 1.2.4 Access the Web Server from the browser.

35) Open your web browser and type in the following URL:http://**Your Company Name-Your First Name**.swedencentral.cloudapp.azure.com

![Screenshot](/images/Slide26.PNG)


Congratulations you made it through.