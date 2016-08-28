# Demo - Azure Container Service

This demo shows how deploy Azure Container Service, and how to run a workload in ACS.

## Pre-Requisites
This section lists the pre-requisites required for this demonstration.
- Azure subscription
- Capability to setup an SSH tunnel and generate a SSH public/private key pair. This demo script assumes the use of Windows with PuTTY and PuTTYgen (see <http://www.putty.org/>).

## Setup
*Estimated time: 5 minutes + approximately 10 minutes wait time.*

### Create an SSH public/private key pair
1. Start PuTTYgen.

2. Click the **Generate** button to create an SSH public/private key pair, and move the mouse over the blank area as instructed.

   <img src="./media/puttygen2.png" />

3. Once done, enter a keyphrase (twice).

   <img src="./media/puttygen3.png" />

4. Click the **Save public key** button and save the public key as a .txtx file.

5. Click the **Save private key** button and save the private key as a .ppk file.

### Create Azure Container Service test cluster
1. Sign-in to the Azure portal (<http://portal.azure.com>).

2. Click the plus sign to add a new resource and type *Azure Container Service*. Press enter or select **Azure Container Service** from the dropdown.

3. Select **Azure Container Service (test cluster with DC/OS)**.

   <img src="./media/acs1.png" />

4. Click the **Create** button.

5. Enter the requested values:
    1. **Username** (1)
    2. **SSH public key** (2). Copy this from the textfile you created in step #4.
    3. **DNS prefix** for the cluster (3).
    4. **Resource group** (4)

   <img src="./media/acs3.png" />   

6. Click the **OK** button on the *Basics* pane.

7. Once validation has passed, click the **OK** button on the *Summary* pane.

8. Click the **Purchase** button on the *Buy* pane, and wait for the cluster to deploy.

9. One the cluster is deployed, follow the instructions in [Connect to an Azure Container Service cluster](https://azure.microsoft.com/en-gb/documentation/articles/container-service-connect/) to setup an SSH tunnel.

10. Open <http://localhost> (or <http://localhost:8000> if you used an alternate port like 8000 in this case) to verify that you have access to DC/OS on the cluster.

    <img src="./media/dcos0.png" /> 

## Demo Steps
*Estimated time: 10 minutes*

1. Sign-in to the Azure portal (<http://portal.azure.com>).

2. Click the plus sign to add a new resource and type *Azure Container Service*. Press enter or select **Azure Container Service** from the dropdown.

3. Explain that there are several pre-canned ACS configurations, from test clusters to production clusters with high availability.

4. Select the generic *Azure Container Service* and explain that this lets you configure ACS fully.

   <img src="./media/acs2.png" />

5. Click the **Create** button.

6. Enter the requested values:
    1. **Username** (1)
    2. **SSH public key** (2). Copy this from the textfile you created in step #4 during setup. Explain that you created this with PuTTYgen (demonstrate if necessary).
    3. **Resource group** (3)

   <img src="./media/acs6.png" />

7. Click the **OK** button on the *Basics* pane.

8. In the *Framework configuration* show the dropdown values and explain that you can choose between DC/OS and Swarm for container orchestration.

9. Keep or set **DC/OS** as selected value, and click the **OK** button.

   <img src="./media/acs7.png" />

10. On the *Azure Container Service settings* tab explain the different values.
    1. **Agent count**. The explanation on the field itself is a little cryptic. Explain that an ACS cluster consist of Agent nodes which run the container applications, and Master nodes which orchectrate how containers are placed on the agents. When you have a single master, you get 1 agent node by default, plus the number of nodes you specify in this field. If you have multiple Master nodes (typically 3 or 5), you get 2 agent nodes by default, plus the number of nodes you specify in this field.
    2. **Agent virtual machine size**. Explain that Agents are placed in a Virtual Machine Scale Set, and that all Agent nodes have the same size.
    3. **Master count**. Explain that you have 3 choices: 1 for test clusters, 3 for high available production clusters, or 5 for large production clusters with up to 100 Agent nodes.
    4. **DNS prefix** for the cluster.

    <img src="./media/acs8.png" />

11. Click the **OK** button.

12. Once validation has passed, click the **OK** button on the *Summary* pane.

13. Explain that if you click the **Purchase** button on the *Buy* pane, that the cluster will be deployed, but that this takes a few minutes.

14. Do not create the cluster, but instead navigate to the main page and click the tile of the cluster you created during demo setup.

15. Explain the different items in the resource group and relate them to the architecture in the presentation.

    <img src="./media/acs4.png" />

16. Select the *Public IP Address* item of the mesos master. Explain that you have setup an SSH tunnel to this address on port 2200. (If you have enough time, you can walk through setting up the SSH tunnel.)

    <img src="./media/acs11.png" />

17. Select the *Public IP Address* item of the mesos agents.

    <img src="./media/acs9.png" />

18. Copy the DNS name, and explain that this is the public access to the applications running on your ACS cluster.

    <img src="./media/acs10.png" />

19. Paste the DSN name in a browser and show that there is no response.

20. In a new tab, open <http://localhost> (or <http://localhost:8000> if you used an alternate port like 8000 in this case) as done in step 10 of the demo setup to show the DC/OS Dashboard.

21. Explain that there are 3 nodes connected to the test cluster (1), that currently there are no tasks running, and that this means CPU (3) and Memory (4) allocation is 0.

    <img src="./media/dcos1.png" />

22. Open a new tab to <http://localhost/marathon> (or <http://localhost:8000/marathon>) to show the Marathon dashboard.

23. Click the **Create Application** button.

    <img src="./media/dcos3.png" />

24. Enter an ID, for example `nginx`, and set the memory requirement to 32 MB.

    <img src="./media/dcos4.png" />

25. Click the *Docker Container* tab. Provide the image name `nginx`, and set the *Network* to **Bridged**. Explain that this points to a docker image of the nginx reverse proxy server that is available in the cluster, and that network traffic is routed to it.

    <img src="./media/dcos5.png" />

26. Click the *Ports & Service Discovery* tab. Enter `80` in both the *Container Port* and *Name* fields. Explain that this exposes port 80 in the container to the host.

    <img src="./media/dcos6.png" />

27. Switch to JSON Mode. Add `"hostPort": 80,` after the `containerPort` setting. Explain that this ties the application to port 80 on the host where it is being hosted.

    <img src="./media/dcos13.png" />

28. Switch out of JSON Mode and Click the *Optional* tab. Enter `slave_public` in the *Accepted Resource Roles* field. Explain that this ensures the application is deployed on a node accessible from the web.

    <img src="./media/dcos14.png" />

29. Explain that you can create much more elaborate application configurations using Marathon, and click the **Create Application** button to start the deployment.

30. Show that the application first gets the status *Deploying*, followed by *Running*.

    <img src="./media/dcos7.png" />

31. Refresh the browser (tab) from step 18 and show that nginx is displayed (although not yet configured).

    <img src="./media/dcos2.png" />

32. Go back to the DC/OS dashboard and show that there is now a task running, and that this results in CPU and Memory allocation.

    <img src="./media/dcos11.png" />

33. Select *Nodes* to show on which node the cluster is running and the CPU allocation.

    <img src="./media/dcos12.png" />

34. Switch to the Marathon dashboard and click on the running application.

35. Click de **Scale Application** button to show that you could add more instances of the application.

    <img src="./media/dcos15.png" />

## Clean Up

1. Close PuTTY.
2. Delete the resource group you created.