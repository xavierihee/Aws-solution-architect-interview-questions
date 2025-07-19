### EC2 Instance
<details><summary>1. What is an EC2 instance?</summary>
  <code>An EC2 instance is a virtual server in the Amazon Elastic Compute Cloud (EC2) service. It provides scalable computing capacity in the AWS cloud, allowing users to run applications and services.</code>
</details>

<details><summary>2. Can you explain the difference between an instance and an AMI?</summary>
  <code>An instance is a running virtual server in EC2, while an AMI (Amazon Machine Image) is a pre-configured virtual machine template that serves as a blueprint for launching instances. You use an AMI to create, launch, and clone instances.</code>
</details>

<details><summary>3. How do you launch an EC2 instance?</summary>
  <code>You can launch an EC2 instance through the AWS Management Console, AWS CLI (Command Line Interface), or SDKs using the "RunInstances" command.</code>
</details>

<details><summary>4. What is the significance of an instance type?</summary>
  <code>An instance type defines the hardware of the host computer used for your instance. Each type offers different combinations of CPU, memory, storage, and networking capacity, impacting performance and pricing.</code>
</details>

<details><summary>5. What is the purpose of user data in EC2 instances?</summary>
  <code>User data allows you to run scripts or provide configuration information when launching an instance. It's helpful for tasks like installing software, setting configurations, or running startup scripts.</code>
</details>

<details><summary>6. How can you stop and start an EC2 instance?</summary>
  <code>You can stop or start an EC2 instance using the AWS Management Console, AWS CLI, or SDKs.</code>
</details>

<details><summary>7. What is the difference between stopping and terminating an EC2 instance?</summary>
  <code>Stopping turns off the instance but keeps it in your infrastructure. Terminating deletes the instance and associated resources permanently.</code>
</details>

<details><summary>8. How do you resize an EC2 instance?</summary>
  <code>Stop the instance, change its instance type in the AWS Console, then start it again.</code>
</details>

<details><summary>9. Can you attach an IAM role to an existing EC2 instance?</summary>
  <code>Yes, you can stop the instance, modify its settings, and attach the desired IAM role.</code>
</details>

<details><summary>10. Explain the concept of an Elastic IP address in EC2.</summary>
  <code>An Elastic IP address is a static, public IPv4 address that remains assigned to your AWS account. It ensures your instance retains a consistent IP even if restarted.</code>
</details>

---

### Security Groups

<details><summary>11. What is a security group in EC2?</summary>
  <code>A security group acts as a virtual firewall. It defines inbound and outbound traffic rules for the instance.</code>
</details>

<details><summary>12. How is a security group different from a Network Access Control List (NACL)?</summary>
  <code>Security groups are stateful and apply at the instance level. NACLs are stateless and apply at the subnet level.</code>
</details>

<details><summary>13. Can you associate multiple security groups with a single EC2 instance?</summary>
  <code>Yes, and all rules from associated groups are aggregated.</code>
</details>

<details><summary>14. What are inbound and outbound rules in a security group?</summary>
  <code>Inbound rules regulate incoming traffic; outbound rules regulate outgoing traffic. Each rule specifies protocol, port range, and source/destination.</code>
</details>

<details><summary>15. How does security group evaluation work?</summary>
  <code>Traffic must explicitly match an allow rule to be permitted; all other traffic is denied by default.</code>
</details>

---

### EBS Volumes

<details><summary>16. What is an EBS volume?</summary>
<code>An EBS (Elastic Block Store) volume is a block-level storage device that you can attach to an EC2 instance. It provides persistent storage that remains even if the instance is stopped or terminated.</code>
</details>

<details><summary>17. What is the difference between EBS-backed and instance-store backed instances?</summary>
<code>EBS-backed instances store the root file system on an EBS volume. Instance-store backed instances use ephemeral storage physically attached to the host computer and are not persistent.</code>
</details>

<details><summary>18. How can you increase the size of an EBS volume?</summary>
<code>Increase the size by creating a snapshot → creating a larger volume from the snapshot → attaching it to the instance.</code>
</details>

<details><summary>19. Can you attach multiple EBS volumes to a single EC2 instance?</summary>
<code>Yes. You can attach multiple EBS volumes to a single EC2 instance using unique device names.</code>
</details>

<details><summary>20. gp2 vs. io1 SSDs?</summary>
<code>gp2 offers balanced performance for general workloads. io1 provides consistent, high-performance IOPS for I/O-intensive applications.</code>
</details>

---

### Data Lifecycle Manager (DLM)

<details><summary>21. What is AWS Data Lifecycle Manager?</summary>
<code>AWS DLM automates creation, retention, and deletion of EBS snapshots to manage backup lifecycles.</code>
</details>

<details><summary>22. How do you create a lifecycle policy?</summary>
<code>Use the DLM console or API to define rules for snapshot frequency and retention duration.</code>
</details>

<details><summary>23. What are retention policies in DLM?</summary>
<code>Retention policies define how long snapshots are retained, either by count or time duration.</code>
</details>

---

### Snapshots

<details><summary>24. What is an EBS snapshot?</summary>
<code>An EBS snapshot is a point-in-time copy of a volume for backup or volume restoration purposes.</code>
</details>

<details><summary>25. How do you create a snapshot?</summary>
<code>Use the AWS Console, CLI, or SDKs to select a volume and initiate snapshot creation.</code>
</details>

<details><summary>26. Can you snapshot a root volume on a running instance?</summary>
<code>Yes, but stopping the instance is recommended for data consistency.</code>
</details>

<details><summary>27. Snapshot vs. AMI?</summary>
<code>A snapshot captures volume data. An AMI is a bootable image that includes one or more snapshots.</code>
</details>

---

### Load Balancers

<details><summary>28. What is an Elastic Load Balancer (ELB)?</summary>
<code>ELB automatically distributes traffic across multiple targets like EC2, containers, or IPs.</code>
</details>

<details><summary>29. Types of load balancers?</summary>
<code>ALB (application layer), NLB (transport layer), Classic (legacy layer 4/7 support).</code>
</details>

<details><summary>30. ALB vs. NLB?</summary>
<code>ALB routes traffic based on content; ideal for web apps. NLB is optimized for high-throughput, low-latency transport-level use cases.</code>
</details>

<details><summary>31. What is a Target Group?</summary>
<code>A target group routes traffic to registered targets based on health checks and balancing algorithms.</code>
</details>

---

### Auto Scaling Group

<details><summary>32. What is Auto Scaling in AWS?</summary>
<code>Auto Scaling automatically adjusts EC2 instance count or size based on demand conditions.</code>
</details>

<details><summary>33. How to set up an Auto Scaling group?</summary>
<code>Define a launch configuration or template → create Auto Scaling group referencing it.</code>
</details>

<details><summary>34. What is a Launch Configuration?</summary>
<code>A Launch Configuration is a template with parameters like AMI, instance type, key pair, and security groups.</code>
</details>

---

### IAM Roles for EC2

<details><summary>35. What is an IAM role?</summary>
<code>An IAM role is an identity with permissions policies, used to delegate access to AWS resources without using credentials.</code>
</details>

<details><summary>36. How to associate an IAM role with EC2?</summary>
<code>Attach the role at launch, or stop the instance → modify settings → assign role.</code>
</details>

<details><summary>37. Benefits of IAM roles with EC2?</summary>
<code>Enhances security and simplifies access management without hardcoding credentials.</code>
</details>

---

### Elastic Beanstalk

<details><summary>38. What is AWS Elastic Beanstalk?</summary>
<code>A managed service for deploying applications across multiple languages and frameworks with automated provisioning and scaling.</code>
</details>

<details><summary>39. Elastic Beanstalk vs EC2?</summary>
<code>Beanstalk abstracts infrastructure, handling deployment automatically. EC2 requires manual setup and configuration.</code>
</details>

<details><summary>40. Supported languages/platforms?</summary>
<code>Java, .NET, Node.js, Python, Ruby, PHP, Go, Docker.</code>
</details>

---

### Placement Groups

<details><summary>41. What is a placement group?</summary>
<code>A logical group of EC2 instances within an Availability Zone to influence placement for performance needs.</code>
</details>

<details><summary>42. Types of placement groups?</summary>
<code>Cluster, Spread, and Partition placement groups — each suited for different performance and fault tolerance needs.</code>
</details>

<details><summary>43. Cluster vs. Spread placement groups?</summary>
<code>Cluster = low latency/high throughput. Spread = maximize fault tolerance by spreading instances across hardware.</code>
</details>

<details><summary>44. Can you move an instance into a placement group?</summary>
<code>No. You must launch the instance into the group, or create an AMI and relaunch it in the desired group.</code>
</details>

### Systems Manager Run Command

<details><summary>45. What is AWS Systems Manager Run Command?</summary>
<code>AWS Systems Manager Run Command lets you remotely and securely manage EC2 instances or on-premises machines at scale without direct access.</code>
</details>

<details><summary>46. How do you execute a command on multiple instances?</summary>
<code>Create a Systems Manager document, select target instances, and specify the command — all via Console, CLI, or API.</code>
</details>

<details><summary>47. Benefits over SSH or RDP?</summary>
<code>Provides centralized command execution, secure access without open ports, and detailed audit logging.</code>
</details>

<details><summary>48. What are SSM Documents?</summary>
<code>SSM Documents are JSON/YAML files that define actions and parameters for Run Command execution on instances.</code>
</details>

<details><summary>49. How do you schedule commands?</summary>
<code>Use Systems Manager State Manager to define and enforce desired states across your instances.</code>
</details>

<details><summary>50. Run Command vs. Automation?</summary>
<code>Run Command = manual command execution. Automation = reusable workflows triggered by schedules or events.</code>
</details>

---

### Systems Manager Parameter Store

<details><summary>51. What is Parameter Store?</summary>
<code>A secure, hierarchical storage service for configuration and secrets management like API keys and DB passwords.</code>
</details>

<details><summary>52. Types of parameters?</summary>
<code>SecureString (encrypted via KMS) and String (plain text).</code>
</details>

<details><summary>53. Retrieve parameters from EC2?</summary>
<code>Use the SSM Agent with CLI: <code>aws ssm get-parameter</code>.</code>
</details>

<details><summary>54. Benefits over env vars or config files?</summary>
<code>Centralized, secure, version-controlled, and encrypted storage with IAM-based access control.</code>
</details>

<details><summary>55. SecureString vs. String?</summary>
<code>SecureString uses KMS encryption for sensitive data; String stores plain text values.</code>
</details>

---

### Systems Manager Session Manager

<details><summary>56. What is Session Manager?</summary>
<code>A secure shell access tool for EC2 instances via browser or CLI, eliminating the need for SSH or RDP.</code>
</details>

<details><summary>57. How does it ensure security?</summary>
<code>Uses IAM for access control and logs session activity for auditing via CloudWatch and CloudTrail.</code>
</details>

<details><summary>58. Can it connect to on-prem or other clouds?</summary>
<code>Yes, if the target machines have the SSM Agent installed and are registered with Systems Manager.</code>
</details>

<details><summary>59. Advantages over traditional remote access?</summary>
<code>No open ports or public IPs needed; granular IAM controls and full audit trail.</code>
</details>

<details><summary>60. How to configure Session Manager?</summary>
<code>Ensure SSM Agent is installed and running; assign IAM role with necessary Systems Manager permissions.</code>
</details>
