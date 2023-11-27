# 2. Azure Load Testing Service
Welcome to this technical tutorial on Azure load testing. In this chapter, we will focus on Azure Load test Service. You will learn how to set up an Azure container instance and an Azure load testing instance. These instances will be used to run load tests against an Azure container apps instance that contains 2 containers.

Initially, we will start with a single virtual user for the load testing. As the load increases, you will observe how the Azure container apps automatically scale out to accommodate the traffic load. This step-by-step tutorial will provide you with the knowledge and skills required to effectively load test your Azure container apps. Let’s begin!


## 2.1 Deployment


### 2.1.1 Create Azure Container Apps

In this step, we will create a Container Apps instance that hosts a pre-made web template for testing purposes. This will allow us to easily set up a test environment and begin experimenting with load testing. Follow the instructions below to create your Container Apps instance.

1) In the Azure Portal, type **Container Apps** in the search field.
2) From the drop down menu choose **Container Apps**. 

![Screenshot](/images/alt1.JPG)

3) Click on **Create** button.

![Screenshot](/images/alt2.JPG)

4) Select your subscription from the drop down menu.
5) Select your existing resource group called **rg-companyname-yourname**.
6)Provide a name for the container apps instance, call it: **ca-yourcompanyname-yourname**.
7) select **Sweden Central** as your region.

8) Press **Next: Container>**.

![Screenshot](/images/alt3.JPG)

9) Press **Next: Bindings >**.

![Screenshot](/images/alt4.JPG)

10) Press **Next: Tags >**.

![Screenshot](/images/alt5.JPG)

11) Press **Next: Review + Create >**.

![Screenshot](/images/alt6.JPG)

12) Press **Create**.

![Screenshot](/images/alt7.JPG)

The deployment is in progress, it may take 1-3 minutes.

![Screenshot](/images/alt8.JPG)

13) Once the deployment is finished, click on the **Home >** breadcrumb on the top left hand side of the portal.

![Screenshot](/images/alt9.JPG)

14) Click on **Resource groups**.

![Screenshot](/images/alt10.JPG)

15) Click on your resource group called **rg-yourcompanyname-yourname**.

![Screenshot](/images/alt11.JPG)

16) Within the resource group, you should be able to locate your Container Apps instance. Let’s take a moment to verify that the pre-made web template is up and running, by clicking on your **Container Apps instance**

![Screenshot](/images/alt12.JPG)

17) In the overview page of Container Apps, copy the URL and click on the **Application URL**.

![Screenshot](/images/alt13.JPG)

You should now see a page similar to the one illustrated below. If you can see it, congratulations! You have successfully created your first Container Apps instance and made it accessible from the internet. We now have an Azure Container Apps instance that we can run load tests against. Follow the instructions below to verify the status of your Container Apps instance.

![Screenshot](/images/alt14.JPG)

### 2.1.2 Create Azure Load Testing Service Instance

18) In the Azure Portal, type **Azure Load Testing** in the search field.

19) From the drop down menu choose **Azure Load Testing**

![Screenshot](/images/alt15.JPG)

20) Click on the button called ***Create Azure load testing resource**

![Screenshot](/images/alt16.JPG)

21) Select your **Subscription** from the drop down menu
22) Select your existing resource group called **rg-yourcompanyname-yourname**.
23) Provide a name for your Azure Load Test service instance, call it **alt-yourcompanyname-yourname**.
24) Select **Sweden Central** as your region for deployment.
25) Click on **Review + create**. 

![Screenshot](/images/alt17.JPG)

26) Click **Create**.

![Screenshot](/images/alt18.JPG)

27) Once the deployment is finished, click on the **Home >** breadcrumb on the top left hand side of the portal.

![Screenshot](/images/alt19.JPG)

28) Click on **Resource groups**.

![Screenshot](/images/alt20.JPG)

29) Click on your resource group called **rg-yourcompanyname-yourname**.

![Screenshot](/images/alt21.JPG)

30) Within the resource group, you should be able to locate your **Azure Load Testing resource**. 

![Screenshot](/images/alt22.JPG)

31) Click on the **Create Button**. to create your load test environment.

![Screenshot](/images/alt23.JPG)

32) Provide a test name for your test environment, follow the following convention: **basic-test-ca-yourcompanyname-yourname**
33) Provide a test description for your test environment for example **Testing load against my app in Azure Container apps**
34) thick the box **Run test after creation**

35) Provide the **URL** for the container app instance, as this is the URL we will be generated load for.

36) Specify **one virtual user** for testing purposes.

37) Specify **1 minute** for test duration.

38) Specify **0 minutes** for Ramp-up time.

39) Press **Review + create**.

![Screenshot](/images/alt24.JPG)

40) Press **Create**.

![Screenshot](/images/alt25.JPG)

The load test is initializing, and will start momentarily.

![Screenshot](/images/alt26.JPG)

When the load test begins, you will be able to view graphs that display the number of virtual users, response times, and the number of requests per second. It is important to carefully analyze these graphs to understand the test results.

![Screenshot](/images/alt27.JPG)

41) Let’s assess the impact of the load test we conducted on our Azure container app. To do this, we can click on the resource called **ca-yourcompanyname-yourname** within your resource group.

![Screenshot](/images/alt29.JPG)

42) Click on **Scale and replicas**.

![Screenshot](/images/alt30.JPG)

We can observe that the number of replicas in the Azure container apps has scaled up to 10 instances to accommodate the load generated by the Azure load test.

![Screenshot](/images/alt31.JPG)

### 2.1.3 Create Scenario Based Load Test

#### 2.1.3.1 Scenario

As a tester, you are responsible for running a load test on a company's internet facing landing page web application, which is deployed on Azure Container Apps. The application owner expects the web application to provide fast and consistent responses to the visitors, with an average latency of no more than 800 milliseconds. The application owner also wants to keep the cost of running the web application as low as possible, and only use one container instance on Azure Container Apps if possible. You will use Azure Load Testing Service to generate realistic and scalable load on the web application, based on the requirements and measure its latency.

#### 2.1.3.1 Deployment




