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
We will activate Microsoft Defender for Containers now, because it takes some time for the initial activation and configuration to complete. By enabling it early, we can ensure that our containers are protected as soon as possible and avoid any potential security gaps. To enable Microsoft Defender for Containers, we will follow these steps:

### 1.2.2 Install Nginx Webserver.

### 1.2.3 Configure domain name for your Virtual Machine.

### 1.2.4 Access the Web Server from the browser.

1) Navigate to the Azure portal at https://portal.azure.com 

2) Type in **Microsoft Defender for Cloud** in the search field.

3) From the drop down menu click on  **Microsoft Defender for Cloud**.

![Screenshot](/images/mdc-step1.png)

3.1) If you are prompted to upgrade press the Upgrade button.

![Screenshot](/images/defender-upgrade.png)

4) In the Microsoft Defender for Cloud overview page, Click **Environment settings** in the left hand side menu under the **Management** section.

5) Expand the **Tenant Root Group** (if you don't see a tenant group, just skip this step).

6) On the far right hand side of the subscription click on the **three dots** to open the context menu.

7) From the context menu click on **Edit settings**.

![Screenshot](/images/mdc-step2.png)

8) On the Containers plan toggle the bar to **On**

9) Ensure the Monitoring coverage states **Full**

10) Press **Save**

![Screenshot](/images/mdc-step3.png)

## 1.2 Initial Setup

### 1.2.1 Environment variables
For convenience, create a few environment variables to store values that are frequently reused during the workshop.

````bash
RESOURCE_GROUP=security-workshop
CLUSTERNAME=k8s
LOCATION=northeurope

````

Whenever you create environment variables, it is a good practice to display the content. As an example:

````
echo $RESOURCE_GROUP
````

which should give the following output

````
security-workshop
````

## 1.2.2 Create Resource Group

In all the commands in this section, we will use the name space name ```` $RESOURCE_GROUP````. If you choose a different name, just make sure to modify the commands accordingly

All resources in Azure exists in a *Resource Group*. The resource group is a "placeholder" for all the resources you create. 

All the resources you create in this workshop will use the same Resource Group. Use the commnd below to create the resource group.

````bash
az group create -n  $RESOURCE_GROUP -l $LOCATION
````


## 1.2.3 Azure Kubernetes Service (AKS)

AKS is the hosted Kubernetes service on Azure.

Kubernetes provides a distributed platform for containerized applications. You build and deploy your own applications into a Kubernetes cluster, and let the cluster manage the availability and connectivity. You will learn how to:

* Create an AKS Kubernetes Cluster
* Connect/validate access and permissions towards the AKS Cluster


### 1.2.4 Create AKS Cluster

Create a really small AKS cluster using ````az aks create```` command:

```bash
az aks create --resource-group  $RESOURCE_GROUP --name  $CLUSTERNAME --node-count 1 --node-vm-size Standard_D2s_v4 --no-ssh-key  --network-plugin azure --network-policy azure
```

#### NOTE: the ````network-plugin```` and ````--network-policy```` settings are needed for a later exercise

#### NOTE 2: You may get an obscured message about docker_bridge_cidr. If so, simply disregard it.


The creation time for the cluster should be around 4-5 minutes.

### 1.2.5. Get access to the AKS Cluster

In order to use `kubectl` you need to connect to the Kubernetes cluster, using the following command:

```bash
az aks get-credentials --resource-group  $RESOURCE_GROUP --name  $CLUSTERNAME
```

To verify that your cluster is up and running you can try a kubectl command, like ````kubectl get nodes```` which  will show you the nodes (virtual machines) that are active in your cluster. If you followed the instructions, you should see just one node.

````bash
kubectl get nodes
````


### 1.2.6 Secure cluster access with Microsoft Entra ID (Azure Active Directory)

Azure Kubernetes Service (AKS) supports Azure Active Directory (AAD) integration, which allows you to control access to your cluster resources using Azure role-based access control (RBAC). 


In this section, you will learn how to:

- Update an existing AKS cluster to support AAD integration enabled.
- Create an AAD  group and assign it the Azure Kubernetes Service Cluster Admin Role.
- Create User in AAD


### 1.2.7 Integrate AKS with Microsoft Entra ID

Update the existing AKS cluster to support Microsoft Entra ID integration, and configure a cluster admin group, and disable local admin accounts in AKS. This will prevent anyone from using the **--admin** switch to get the cluster credentials.

````bash
az aks update -g  $RESOURCE_GROUP -n  $CLUSTERNAME --enable-azure-rbac --enable-aad --disable-local-accounts
````
Remove the kubeconfig file from your local filesystem. This is done to remove the access key that allows you to communicate with the Kubernetes API. Normally this would be done by rotating the certificates in AKS by an administrator, but this work-around saves us a time. 

````bash
rm -fr ~/.kube
````
Download the AKS credentials.

```bash
az aks get-credentials --resource-group  $RESOURCE_GROUP --name  $CLUSTERNAME
```
Use the following command to check the status of your cluster nodes. 

````bash
kubectl get nodes
````

This will trigger a sign-in procedure as describe below. The sign in will fail, because you still have not created a user with permissions to connect to AKS. Creating that user is what comes next.

**Sign in with your Microsoft Entra ID credentials and get Azure RBAC permissions to use the Kubernetes API.** This is needed because your cluster has Microsoft Entra ID integration and Azure RBAC enabled.

Example output:
````bash
contoso@DESKTOP-6FPE1AE:~$ kubectl get nodes
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XXXXXXXX to authenticate.
Error from server (Forbidden): nodes is forbidden: User "demo@XXXXXXXX.onmicrosoft.com" cannot list resource "nodes" in API group "" at the cluster scope: User does not have access to the resource in Azure. Update role assignment to allow access.
````


As mentioned, this error occurs because you have signed in with Azure AD and you do not have the appropriate role assignment in Azure RBAC to access any Kubernetes API objectS. To fix this, you need to create a user with the appropiate role assignment to the Azure Kubernetes Service cluster.

Lets access the cluster with admin permissions, but first we need to:

Create the security group in Azure AD for **Cluster Admin**

````bash
az ad group create --display-name AKSAdminGroup --mail-nickname AKSAdminGroup
````

Then grant the **AKSAdminGroup** group users the permissions to connect to and manage all aspects of the AKS cluster. For this you need the reource ID of your cluster and the object ID of your management group. For convenience you can put it in environment variables:

````bash
AKS_RESOURCE_ID=$(az aks show -g  $RESOURCE_GROUP -n  $CLUSTERNAME --query 'id' --output tsv)
ADMIN_GROUP_ID=$(az ad group show --group AKSAdminGroup --query 'id' --output tsv)
````


````bash
az role assignment create --assignee $ADMIN_GROUP_ID --role "Azure Kubernetes Service RBAC Cluster Admin" --scope $AKS_RESOURCE_ID
 ````

To create the user account in the next step, you need to know the domain name of your tenant. Here is how you can get it.

````bash
DOMAIN=$(az ad signed-in-user show --query 'userPrincipalName' -o tsv | sed 's/.*@/@/')
````

Create the Admin user called John Doe.

````bash
az ad user create --display-name 'John Doe'  --user-principal-name john$DOMAIN --password Something_secure123
````

Assign the admin user to admin group for the AKS cluster.

First get the object id of the user as we will need this number to assign the user to the admin group. For convenience, you can put it in an environment variable

````bash
ADMIN_USER_ID=$(az ad user show --id john$DOMAIN --query 'id' --output tsv)
````

Assign the user to the admin security group.

````bash
az ad group member add --group AKSAdminGroup --member-id $ADMIN_USER_ID
````

### 1.2.8 Validate the access to the cluster.

First, delete the kube context once again (because it gets recreated when you run ````az aks get-credentials````)

````bash
rm -fr ~/.kube
````

Then obtain the AKS credentials again. 

````bash
az aks get-credentials -g  $RESOURCE_GROUP -n $CLUSTERNAME
````

Note down the login username which we have conviently stored in environment variable.

````bash
echo john$DOMAIN
````

Now, try to list all nodes on the cluster. 


````
kubectl get nodes
````

This will again trigger a login procedure after which you should be able to interact with the Kubernetes API with "johns" RBAC permissions. 

login with the user ***John Doe*** by selecting *Use Another Account* in the browser window. Use the email address for your newly created user, which will be what you ````echoed```` above and should look something like: ````john@somemail.onmicrosoft.com````. 

The password to use is the one you used when creating the user (which, if you didn't change it, was ````Something_secure123````).


Upon returning to the bash shell, you should see output similar to this:

````bash
alibengtsson@DESKTOP-6FPE1AE:~$ kubectl get nodes
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XXXXXXXX to authenticate.
NAME                                STATUS   ROLES   AGE   VERSION
aks-nodepool1-18760117-vmss000000   Ready    agent   53m   v1.26.6
alibengtsson@DESKTOP-6FPE1AE:~$
````
Create a namespace called test-ns in the AKS cluster. 

````bash
kubectl create ns test-ns
````

Verify the creation of the namespace.

````bash
kubectl get namespaces
````

Create an Nginx Pod and deploy it to namespace test-ns.

````bash
kubectl run test-pod --image=nginx --namespace test-ns
````

Verify the creation of the Nginx pod.

````bash
kubectl get pods -n test-ns
````
As a Cluster admin you have full access to the AKS cluster.