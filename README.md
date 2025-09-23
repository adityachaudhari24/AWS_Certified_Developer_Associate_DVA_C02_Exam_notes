# AWS-Certified-Developer-Associate-DVA-C02-Exam-notes
AWS Certified Developer - Associate (DVA-C02) Exam notes


## Table of Contents
1. <a href="#ELB & ASG">ELB ASG</a>
2. <a href="#Prompt-Engineering">Prompt Engineering</a>
3. <a href="#Amazon-Q">Amazon Q</a>




## ELB & ASG

<details>
<summary>🎯Q. Difference availability vs scalability  </summary>

- 🔹 High Availability (HA)
- `Definition`: Ensuring your application/system is accessible and operational without downtime, even if failures occur.
- `AWS mechanisms`:
   - Multi-AZ deployments (e.g., RDS Multi-AZ, S3 cross-region replication).
   - Load balancers across multiple Availability Zones.
   - Auto-recovery features (e.g., EC2 Auto Recovery).
   - `Exam tip`: If the question mentions minimizing downtime, fault tolerance, or disaster recovery, the answer is about high availability.
   - `Example`: Deploying your application across multiple AZs with ELB so if one AZ fails, traffic automatically routes to healthy instances.

- 🔹 Scalability
- `Definition`: The ability of your application/system to handle increased load by adding more resources (scale out) or making resources bigger (scale up).
- `AWS mechanisms`:
  - Auto Scaling Groups (EC2, ECS, Lambda concurrency).
  - DynamoDB auto-scaling (read/write capacity units).
  - Serverless architectures (Lambda scales automatically).
  - `Exam tip`: If the question mentions handling traffic spikes, growing workloads, or unpredictable demand, the answer is about scalability.
  - `Example`: Configuring an EC2 Auto Scaling Group to add more instances when CPU > 70%.

- `High availability` = 💡 uptime, fault tolerance.
- `Scalability` = 💡 ability to grow with demand.
</details>

<details>
<summary>🎯Q. what is load balancing  </summary>

- `load balancers are the servers which distribute incoming application traffic` across multiple targets, such as EC2 instances, in multiple Availability Zones.
- what all things we can do with load balancer
  - `spread load across multiple downstream servers` to ensure no single server becomes a bottleneck.
  - `expose a single point of access(DNS) to the application` while hiding the complexity of the backend infrastructure.
  - `handle faolure of the downstream instances/servers` by rerouting traffic to healthy instances.
  - `do regular health checks` on the backend instances to ensure traffic is only sent to healthy instances.
  - `provide the SSL termination (HTTPS) for your website ` - offloading the SSL decryption/encryption from the backend servers.
  - `enforce sticky sessions` to ensure that a user's session is consistently routed to the same backend instance.
  - `high availability` by distributing traffic across multiple Availability Zones.
  - `separate public traffic from the private traffic` - for example, using an internet-facing load balancer for public traffic and an internal load balancer for private traffic within a VPC.

- `Types of Load Balancers in AWS`:
  - ALB (Application Load Balancer): Layer 7, for HTTP/HTTPS, supports advanced routing (path/host-based).
  - NLB (Network Load Balancer): Layer 4, for TCP/UDP, high performance and low latency.
  - CLB (Classic Load Balancer): Basic Layer 4/7, legacy use.
  - GLB (Gateway Load Balancer): Layer 3, for deploying/scaling third-party appliances (e.g., firewalls).
- `Exam tip`: If the question mentions distributing traffic, handling failures, or improving application availability, it's likely about load balancing.
</details>


<details>
<summary>🎯Q. what is auto scaling group  </summary>

- The goal of the ASG is to `maintain application availability and allow you to automatically add or remove instances according to conditions you define`.
- scale out (add instances) during demand spikes to maintain performance and `scale in (remove instances)` during low demand to reduce costs.
- ASG also ensure minimum and maximum number of instances are running on the specific time.
- scale in and out is possible based on
  - `Dynamic scaling`: Adjusts the number of instances based on real-time metrics (e.g., CPU utilization, network traffic). (cloudwatch alarms)
    - `target tracking scaling`: Automatically adjusts the number of instances to maintain a specific metric (e.g., keep average CPU at 50%).
    - `Simple / step scaling`: Adds or removes instances based on predefined thresholds (e.g., add 2 instances if CPU > 70% for 5 minutes).
  - `Predictive scaling`: Uses machine learning to forecast future traffic and adjusts capacity in advance.
  - `Scheduled scaling`: Anticipate scaling based on known usage patterns (e.g., increase instances every weekday at 9 AM).
</details>

<details>
<summary>🎯Q. health checks for the instances are done at ALB or ASG level ?  </summary>

- ✅ Health checks can happen at two levels:
  - Target Group / ALB level health checks
    - Defined in the Target Group (HTTP/HTTPS/TCP checks).
    - ALB routes traffic only to healthy targets.
    - If an instance fails ALB health check → ELB stops sending traffic there.
    - But ASG does not terminate the instance based on ALB health check alone (unless you configure it).
    
- ASG health checks
    - By default, ASG uses EC2 status checks (system + instance status).
    - You can also configure ASG to use ELB health checks.
    - If an instance fails, ASG terminates it and launches a replacement.
  
- 🔑 Key Difference 
    - ALB health check → Controls traffic routing.
    - ASG health check → Controls instance lifecycle (terminate & replace).
- Exam Tip:
    - 🎯If a question says “ensure unhealthy instances are replaced automatically” → the answer is ASG health check using ELB health check type.🎯
    - 🎯If a question says “ensure users are not routed to unhealthy instances” → the answer is Target Group health check (ALB level).🎯
</details>

<details>
<summary>🎯Q. when to choose ALB and when to choose NLB  </summary>

- ⭐`Application Load Balancer (ALB)`⭐
- Layer 7 (Application Layer) load balancing. 
- Best when you need:
    - HTTP/HTTPS traffic handling.
    - Host-based or path-based routing (e.g., /api → one service, /images → another).
    - Support for WebSockets.
    - Integration with AWS WAF for security.
    - Advanced features like content-based routing, redirects, and fixed responses.
    - `Target types`: EC2, ECS, EKS, Lambda (serverless integration!).
- `Use case examples`:
    - Microservices with multiple APIs.
    - Routing to different services based on the request path.
    - Serving websites or REST APIs.

- ⭐`Network Load Balancer (NLB)`⭐
- Layer 4 (Transport Layer) load balancing. 
- Best when you need:
    - Very high performance and ultra-low latency.
    - Handling millions of requests per second.
    - TCP, UDP, and TLS traffic.
    - `Static IP support (can attach an Elastic IP)`. means it does not provide the DNS name like ALB.
    - Preservation of client source IP.
    - Target types: EC2, ECS, EKS, `IP addresses`, `ALBs (chaining)`.
- Use case examples:
    - Financial applications requiring fixed IP and low latency.
    - Gaming or VoIP apps (UDP traffic).
    - Load balancing across on-prem + cloud (hybrid).
</details>

⭐⭐⭐ one liner notes ⭐⭐⭐
- Application Load Balancers do not utilize geographical data to route traffic; instead, they can route based on factors like URL Path and Hostname, ensuring efficient traffic management according to specified criteria.
- you 🎯cannot attach an elastic IP to an Application Load Balancer (ALB)🎯; elastic IPs are typically associated with EC2 instances or Network Load Balancers (NLBs).
- ⭐cross zone load balancing⭐ -  distributes incoming traffic evenly across all registered instances in all enabled Availability Zones, enhancing fault tolerance and resource utilization.
- Server Name Indication (SNI) is a feature that allows multiple SSL certificates to be hosted on a single load balancer, enabling secure connections for different domains without needing separate IP addresses.
- `cooldown period` - This is configured at the Auto Scaling Group level to prevent rapid scaling actions, allowing the system to stabilize before another scaling event occurs.

<details>
Emojis used
⭐ - For important points
🔥 - For hot/important exam topics
💡 - For key concepts/tips
⚠️ - For warnings/common mistake
🎯 - For exam targets/focus areas/ question 
🚀 - For advanced topics .
🚫 - For indicating something that cannot be used or a concerning point
</details>