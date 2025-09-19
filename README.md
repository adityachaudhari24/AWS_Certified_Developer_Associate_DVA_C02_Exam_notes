# AWS-Certified-Developer---Associate-DVA-C02-Exam-notes
AWS Certified Developer - Associate (DVA-C02) Exam notes


## Table of Contents
1. <a href="#General-Notes">General-Notes</a>
2. <a href="#Prompt-Engineering">Prompt Engineering</a>
3. <a href="#Amazon-Q">Amazon Q</a>
4. <a href="#AI-and-ML">AI and ML</a>
5. <a href="#aws-managed-ai-services">AWS managed AI services</a>
6. <a href="#Amazon-Sagemaker">Amazon SageMaker</a>
7. <a href="#AI-Challenges-and-Responsibilities">AI Challenges and Responsibilities</a>




## General Notes

<details>
<summary>🎯Q. Difference availability vs scalability  </summary>

- 🔹 High Availability (HA)
Definition: Ensuring your application/system is accessible and operational without downtime, even if failures occur.
AWS mechanisms:
Multi-AZ deployments (e.g., RDS Multi-AZ, S3 cross-region replication).
Load balancers across multiple Availability Zones.
Auto-recovery features (e.g., EC2 Auto Recovery).
`Exam tip`: If the question mentions minimizing downtime, fault tolerance, or disaster recovery, the answer is about high availability.
`Example`: Deploying your application across multiple AZs with ELB so if one AZ fails, traffic automatically routes to healthy instances.

- 🔹 Scalability
Definition: The ability of your application/system to handle increased load by adding more resources (scale out) or making resources bigger (scale up).
AWS mechanisms:
Auto Scaling Groups (EC2, ECS, Lambda concurrency).
DynamoDB auto-scaling (read/write capacity units).
Serverless architectures (Lambda scales automatically).
`Exam tip`: If the question mentions handling traffic spikes, growing workloads, or unpredictable demand, the answer is about scalability.
`Example`: Configuring an EC2 Auto Scaling Group to add more instances when CPU > 70%.

`High availability` = uptime, fault tolerance.
`Scalability` = ability to grow with demand.
</details>