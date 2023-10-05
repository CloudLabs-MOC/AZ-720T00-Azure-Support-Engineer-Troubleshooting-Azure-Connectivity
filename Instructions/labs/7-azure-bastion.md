# Module 07: Troubleshoot routing, traffic control and load balancing issues

## Lab Scenario

In this lab, you are responsible for supporting Azure infrastructure. One of the teams you support is facing difficulties connecting to their virtual machine (VM) using the Azure Bastion service. Azure Bastion is a service that allows secure and seamless RDP and SSH connectivity to Azure virtual machines directly from the Azure portal.

The team is unable to establish a connection to the VM through Azure Bastion, and your task is to diagnose and troubleshoot the issue. You will investigate and resolve problems with Azure Bastion, ensuring that it can be used to access Azure virtual machines successfully.

## Lab Objectives

In this lab you will perform:

+ Task 1: Check that the issue still exists
+ Task 2: Check that Bastion has been deployed
+ Task 3: Check if AzureBastionSubnet is using a Network Security Group correctly
+ Task 4: Check if there's a private DNS zone
+ Task 5: Run the Connection Troubleshoot tool to check for issues
+ Task 6: Resolve the bastion connection issue
+ Task 7: Test the issue is resolved

## Estimated timing: 30 minutes

You've been contacted by one of the teams you support. The team is having problems connecting to their VM using the Azure Bastion service.

In this lab, you'll see how to troubleshoot the Azure Bastion Service.

### Task 1: Check that the issue still exists

1.  If you are not logged in already, click on Azure portal shortcut that is available on the desktop and log in with below Azure credentials or skip to **step 2**.
    * Azure Username/Email: <inject key="AzureAdUserEmail"></inject> 
    * Azure Password: <inject key="AzureAdUserPassword"></inject>
    
1. From the top left portal menu, select **Virtual machines**, then select **labvm1-<inject key="DeploymentID" enableCopy="false"/>**.

1. On the Overview pane, select **Connect**, then select **Bastion**.

1. In **Username**, enter **<inject key="VM Admin Username" enableCopy="true"/>**

1. In **Password**, enter 
   ```
    Password.1!!
     ```

1. Select **Connect**.

1. Please ensure to enable the pop-up settings of the browser.

   ![Screenshot showing selecting option 15.](../media/popup.png)

> **Note:** It takes time for the new tab to open, and when it does there's no response.

### Task 2: Check that Bastion has been deployed

1. In the Azure portal, in the search box, type **Bastions**.

   ![A screenshot showing the Bastion service being searched for and then selected.](../media/mod7-2.png)

1. From the results, under **Services**, click **Bastions**.

1. You should see the Bastion service listed.

   ![A screenshot showing the list of created Azure Bastions.](../media/mod7-1.png)

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Navigate to the Lab Validation Page, from the upper right corner in the lab guide section.
    > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 3: Check if AzureBastionSubnet is using a Network Security Group correctly

1. In the Azure portal, in the search box, type **Bastions**.

1. From the results, under **Services**, click **Bastions**.

1. Select the **lab07-rg-<inject key="DeploymentID" enableCopy="false"/>-vnet-bastion**.

   ![A screenshot showing the Virtual network/subnet link on the Bastion pane.](../media/mod7-3a.png)

1. In the top right, click the **Virtual network/subnet** link.

   ![A screenshot of the subnets menu highlighted on the virtual network pane, with the AzureBastionSubnet selected.](../media/mod7-4a.png)

1. Under **Settings**, click **Subnets**, and then click **bastion-nsg**.

If Azure Bastion has a **Network security group** associated with the subnet, you need to check that it has all the inbound and outbound rules created. See the official Microsoft documentation, https://learn.microsoft.com/azure/bastion/bastion-nsg. Are there any rules missing?

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Navigate to the Lab Validation Page, from the upper right corner in the lab guide section.
> - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 4: Check if there's a private DNS zone

1. In the Azure portal, in the search box, type **private dns**.

   ![A screenshot of the Azure portal, searching for private D N S, and selecting Private D N S zones.](../media/mod7-5.png)

1. From the results, under **Services**, click **Private DNS zones** and you shouldn't see any private DNS zones.

   ![A screenshot showing that no private D N S zones are being used.](../media/mod7-no-private-zones.png)

1. If there are any zones listed, check that they don't end in **azure.com** or **core.windows.net**.

   ![Screenshot showing a private D N S zone that will cause connection problems for Azure Bastion.](../media/mod7-7.png)

### Task 5: Run the Connection Troubleshoot tool to check for issues

1. In the Azure portal, in the search box, type **Bastions**.

1. From the results, under **Services**, click **Bastions**.

1. Select the Bastion you are troubleshooting.

1. Under **Monitoring**, click **Connection Troubleshoot**.

1. Under **Resource group**, select **lab07-rg-<inject key="DeploymentID" enableCopy="false"/>**

1. Under **Virtual machine**, select the only existing Virtual Machine.

1. Select your **Preferred IP Version**.

1. In the **Destination port**, we use **RDP**, enter **3389** for this lab.

1. Click **Check**.

    ![A screenshot of the Azure Bastion connection troubleshooting wizard.](../media/mod7-8.png)

1. The connection troubleshooter will show that the VM is reachable, even though it isn't.

    ![A screenshot of the connection troubleshoot results, showing the stats as reachable.](../media/mod7-9a.png)


### Task 6: Resolve the bastion connection issue

After reviewing the bastion-nsg network security group you notice that there is a missing rule.

1. In the Azure portal, in the search box, type **Network security groups**.
1. Select **bastion-nsg** of Resource Group **lab07-rg-<inject key="DeploymentID" enableCopy="false"/>**.
1. Select **Inbound security rules**.
1. Select **+ Add**.
1. In the fly out, enter the following details and leave the remaining at default.

    - Source: **Service Tag**
    - Source service tag: **Internet**
    - Service: **HTTPS**
    - Name: **AllowInternetInbound**

    ![Screenshot of the Add inbound security rule fly out.](../media/mod7-10.png)

1. Select **Add**.

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Navigate to the Lab Validation Page, from the upper right corner in the lab guide section.
    > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### Task 7: Test the issue is resolved

1. Select the top left portal menu, select **Virtual machines**, then select **labvm1-<inject key="DeploymentID" enableCopy="false">**.

1. On the Overview pane, select **Connect**, then select **Bastion**.

1. In **Username**, enter <inject key="VM Admin Username" enableCopy="true"/>
   

1. In **Password**, enter 
    ```
    Password.1!!
    ```

1. Select **Connect**.

    ![Screenshot showing a browser tab connected to a VM with Bastion.](../media/mod7-final.png)

    You should see a new tab open and connect to your VM.

## Review

In this lab, you have performed:

- Diagnosing Azure Bastion connectivity issues.
- Verifying Azure Bastion deployment.
- Investigating and fixed Network Security Group (NSG) rules.
- Successfully resolved the issue and established a VM connection through Azure Bastion.

## You have Successfully Completed this Lab
