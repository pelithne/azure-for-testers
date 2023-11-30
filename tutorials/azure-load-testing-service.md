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

As a tester, you are responsible for running a load test on a company's internet facing landing page web application, which is deployed on Azure Container Apps. The application owner expects the web application to provide fast and consistent responses to the visitors, with an average latency of no more than 800 milliseconds. The application owner also wants to keep the cost of running the web application as low as possible, and only use one container instance on Azure Container Apps if possible. You will use Azure Load Testing Service to generate realistic and scalable load on the web application, based on the requirements we will use Azure Load testing Service to create the testing criteria.

#### 2.1.3.1 Preparation

To ensure that only one container is used in Azure container apps, as desired by the application owner, we have to set the maximum scaling limit of azure container apps to one container.

43) Click on the **Azure container app resource**.

![Screenshot](/images/alt/Slide1.JPG)


44) Click on **Scale and replicas**
45) Select the "http-scaler

![Screenshot](/images/alt/Slide2.JPG)


46) Set max-replicas to 1
47) Click **create** in the bottom left. This will create a new revision with the new scaling limitation.

![Screenshot](/images/alt/Slide3.JPG)




48) Go back to the resource group, and select the **Azure Load Testing resource**, to start building the load test.

![Screenshot](/images/alt/Slide6.JPG)


49) Select **tests** from the left hand side menu
50) Click **create** and select URL-based test

![Screenshot](/images/alt/Slide7.JPG)

51) Give the test a name
52) Write a brief description of the test
53) Select **Run the test after creation**
54) Select **Enable advanced settings**
55) Click **Next**

![Screenshot](/images/alt/Slide8.JPG)

56) In the test plan section, click **Add request**

![Screenshot](/images/alt/Slide9.JPG)

57) Give the request a name
58) Enter the URL of your Container App
59) Select "Add"

![Screenshot](/images/alt/Slide10.JPG)

60) Click **next**

![Screenshot](/images/alt/Slide11.JPG)


61) No change is needed in **Parameters** so just click **Next**

![Screenshot](/images/alt/Slide12.JPG)


62) Set **Engine Instances** to **3**.
63) Set **Concurrent users per engine** to **250**.
64) Click **Next**.


![Screenshot](/images/alt/Slide13.JPG)


65) Set the **Test Criteria** as in the picture below
66) Set **Error Percentage** to 20%. This will stop the test when error percentage goes over that threshold.
67) Press **Next**.

![Screenshot](/images/alt/Slide14.JPG)

68) Press **Next**.

![Screenshot](/images/alt/Slide15.JPG)

69) Press **Create**.

![Screenshot](/images/alt/Slide16.JPG)

The test is initializing it may take up to 3 minutes before the test environment is ready and can start producing load.

![Screenshot](/images/alt/Slide17.JPG)

Once the test has finished initializing you should now see load metrics.

![Screenshot](/images/alt/Slide18.JPG)

Note that the load test failed based on the test criteria Analyze the data and identify the root cause behind it.

![Screenshot](/images/alt/Slide19.JPG)

70) We now need to scale up our azure container instance, click on the breadcrumb on your top left hand side menu to get to the resource group. **rg-yourcompanyname-yourname**.

![Screenshot](/images/alt/Slide20.JPG)

71) Click on the **Azure container app resource**

![Screenshot](/images/alt/Slide21.JPG)

72) Click on **Revisions**.
73) Click on **Inactive revisions**

![Screenshot](/images/alt/Slide22.JPG)

74) Click **Activate**.
75) Press **Save**.
76) Click on the breadcrumbs called **rg-yourcompanyname-yourname**.

![Screenshot](/images/alt/Slide23.JPG)

77) Click on the **Azure Load Testing Service resource**.

![Screenshot](/images/alt/Slide24.JPG)

78) Click on your **test**.

![Screenshot](/images/alt/Slide25.JPG)

79) Click  **Run**, to rerun the test with a scaled up Azure Container app instance.

![Screenshot](/images/alt/Slide26.JPG)

80) Provide a test description for the test.

81) Press **Run**.

![Screenshot](/images/alt/Slide27.JPG)

82) Press the link **TestRun_xxxxxx**.

![Screenshot](/images/alt/Slide28.JPG)

The test is initializing, will take approx 1-3 minutes.

![Screenshot](/images/alt/Slide29.JPG)

Load metrics should now be visible.

![Screenshot](/images/alt/Slide30.JPG)


Analyze the load metrics and understand why the test successfully passed.

![Screenshot](/images/alt/Slide31.JPG)

83) Mark the two tests that you have conducted and compare the results.
84) Click on **Compare**.

![Screenshot](/images/alt/Slide32.JPG)


85) Once you have finished comparing click on the **breadcrumb**.

![Screenshot](/images/alt/Slide33.JPG)


86) Export the configuration as a JMX file, this will now enable you to save your test as a code and you can now re-run the load test with the same configuration. Analyze the jmx file.

![Screenshot](/images/alt/Slide34.JPG)

