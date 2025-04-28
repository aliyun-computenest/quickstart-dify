<h1> Rapid deployment of Dify computing nest </h1>

<blockquote>
<p><strong> Disclaimer:</strong> This service is provided by a third party. We try our best to ensure its security, accuracy and reliability, but we cannot guarantee that it is completely free from failure, interruption, error or attack. Therefore, the company hereby declares that it makes no representations, warranties or commitments regarding the content, accuracy, completeness, reliability, suitability and timeliness of the Service and is not liable for any direct or indirect loss or damage arising from your use of the Service; for third-party websites, applications, products and services that you access through the Service, do not assume any responsibility for its content, accuracy, completeness, reliability, applicability and timeliness, and you shall bear the risks and responsibilities of the consequences of use; for any loss or damage arising from your use of this service, including but not limited to direct loss, indirect loss, loss of profits, loss of goodwill, loss of data or other economic losses, even if we have been advised in advance of the possibility of such loss or damage; we reserve the right to amend this statement from time to time, so please check this statement regularly before using the Service. If you have any questions or concerns about this Statement or the Service, please contact us. </p>
</blockquote>

<h2> Overview </h2>

<p>Dify.AI is an LLMOps platform that helps developers build AI applications easier and faster. Its core idea is to define all aspects of an AI application through declarable YAML files, including prompts, contexts, and plug-ins. Dify provides functions such as visual Prompt orchestration, operation, and data set management. These capabilities enable developers to complete the development of AI applications in days, or quickly integrate LLM into existing applications, and continue to operate and improve to create a truly valuable AI application.
This solution provides the best practices for deploying Dify. Dify deployed based on Alibaba Cloud ACK supports high availability, auto scaling, and other capabilities, and supports integration with cloud databases, which can achieve stable and high-performance deployment of Dify and is recommend used in production environments. </p>

<h2> Prerequisites </h2>

<p> To deploy a Dify Community Edition service instance, you need to access and create some Alibaba Cloud resources. Therefore, your account must contain permissions for the following resources.
<strong> Note </strong>: This permission is required only when your account is a RAM account. </p>

<table>
<thead>
<tr>
<th> Permission policy name </th>
<th> Remarks </th>
</tr>
</thead>
<tbody>
<tr>
<td>AliyunECSFullAccess</td>
<td> Permissions to manage ECS </td>
</tr>
<tr>
<td>AliyunVPCFullAccess</td>
<td> Permissions for managing VPC networks </td>
</tr>
<tr>
<td>AliyunROSFullAccess</td>
<td> Manage permissions for Resource Orchestration Services (ROS) </td>
</tr>
<tr>
<td>AliyunCSFullAccess</td>
<td> Manage permissions for Container Service (CS) </td>
</tr>
<tr>
<td>AliyunROSFullAccess</td>
<td> Manage permissions for Resource Orchestration Services (ROS) </td>
</tr>
<tr>
<td>AliyunKvstoreFullAccess</td>
<td> Permissions for managing cloud database Tair (compatible with Redis) </td>
</tr>
<tr>
<td>AliyunRDSFullAccess</td>
<td> Permissions to manage ApsaraDB for RDS </td>
</tr>
<tr>
<td>AliyunGPDBFullAccess</td>
<td> Permissions for managing AnalyticDB for PostgreSQL </td>
</tr>
<tr>
<td>AliyunComputeNestUserFullAccess</td>
<td> Manage user-side permissions for the compute nest service (ComputeNest) </td>
</tr>
</tbody>
</table>

<h2> Billing instructions </h2>

<p> The cost of Dify Community Edition deployment in the computing nest mainly involves:</p>

<ul>
<li> Selected vCPU and Memory Specifications </li>
<li> System disk type and capacity </li>
<li> Internet bandwidth </li>
<li> Specifications of the selected cloud database </li>
</ul>

<h2> Deployment Architecture </h2>

<p><img src="8.png" width="1500" height="700" align="bottom"/></p>

<p>Dify application components mainly include business components and basic components. business components include api/worker, web and sandbox. Basic components include: db, verctor db, redis, nginx, and ssrf_proxy.
In this solution, the basic components can be selected from the community open source redis, postgres, and weaviate, or the cloud database of Aliyun. If you have higher performance, function, and SLA requirements for these basic components, or have operation and management pressure on the basic components, you recommend use the cloud database. </p>

<h2> Deployment process </h2>

<ol>
<li> Access the computing nest Dify Community Edition <a href = "https://computenest.console.aliyun.com/user/cn-hangzhou/serviceInstanceCreate?ServiceId=service-c8afb895dd314f70a020"> Deployment link </a> Fill in the deployment parameters as prompted:
If you want to use the existing ACK cluster, you need to select the ID of the ACK cluster. In order to improve the deployment success rate and stability, you recommend use the new container cluster. If you select the existing ACK, you need to manually configure the service access.
<img src="9.png" alt="image.png" />
If you select New Container Cluster, you must configure the node specification and node password.
<img src="10.png" alt="image.png" />
Configure the Postgres database, which requires relevant configuration.
<img src="11.png" alt="image.png" />
To configure the Reids database, you need to configure it.
<img src="12.png" alt="image.png" />
Configuration vector database, need to be configured
<img src="13.png" alt="image.png" />
Configure network parameters. You can select a new VPC or an existing VPC and select two zones to improve service availability.
<img src="14.png" alt="image.png" />
Configure the service. If you have an available domain name, you can enter your own domain name. Otherwise, the test domain name provided by the computing nest is used by default. The test domain name needs to be bound to the host, otherwise it cannot be accessed.
If you need to enable the TLS configuration, you need to upload the TLS certificate.
we recommend that you enable the high availability configuration to improve the application's disaster tolerance and peak load processing capabilities. </li>
<li> After completing the parameters, you can see the corresponding inquiry details. After confirming the parameters, click <strong> Next: Confirm Order </strong>. Confirm the completion of the order and agree to the service agreement and click <strong> Create Now </strong>
Enter the deployment phase.
<img src="16.png" alt="image.png" /></li>
<li> you can start using the service after the deployment is completed. enter the details of the service instance to view the instructions and Dify access page. if you use the domain name, you need to bind host or configure it after parsing according to the instructions.
<img src="17.png" alt="image.png" /></li>
<li> Register your account.
<img src="6.png" alt="image.png" /></li>
<li> login to create your own dify application
<img src="7.png" alt="image.png" /></li>
</ol>

<h2> Instructions for use </h2>

<p >### Configure Access Entry
If you select an existing ACK cluster for deployment, you need to manually configure the access portal. recommend two methods </p>

<p><strong> Method 1:</strong> Access through load balancing and configure according to the following steps
<img src="18.png" alt="img.png" />
After the configuration is complete, you will see the external IP address (External IP) of the ack-dify service. Enter the external IP address into the address bar of the browser to access the Dify service.
<img src="19.png" alt="img.png" /></p>

<p><strong> Method 2:</strong> Access through the configuration Ingress and configure as follows
First, install Nginx Ingress Controller components in the cluster, then configure the relevant routes, and configure them according to the following steps
<img src="20.png" alt="img.png" />
After the configuration is complete, you will see the Ingress endpoint. After the endpoint is bound to the domain name host or the configuration is resolved, you can access Dify through the domain name.
<img src="21.png" alt="img.png" /></p>

<p >### Configure auto scaling for the Dify service
If you want to achieve elastic expansion and contraction,
You can refer to this document to <a href = "https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/auto-scaling-of-nodes?spm=a2c4g.11186623.help-menu-85222.d_2_12_1_0.9ae546c6P5Pf9i"> configure </a> the elastic scaling capacity of a node.
The elastic scaling of the load can turn on the "high availability" switch of the deployment page </p>
