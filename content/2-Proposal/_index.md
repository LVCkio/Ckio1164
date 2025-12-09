---
title: "Proposal"
date: 2025-09-10
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Digital Transformation for Mini-market on AWS Cloud Platform

## .NET 3-tier E-commerce Solution applying Repository and Unit of Work Pattern

### 1. Executive Summary

This proposal presents an end-to-end solution for "Digital Transformation for Mini-market on AWS Cloud Platform". Traditional mini-markets are currently facing three major challenges: (1) Manual inventory management (using Excel/notebooks) causing revenue loss and resource waste; (2) 100% dependence on offline sales channels, missing the growing e-commerce market and losing competitiveness; and (3) Slow operational processes (such as manual price lookup), resulting in poor customer experience.

The group's solution is to build a comprehensive e-commerce and operations management platform. Regarding software architecture, the group will use .NET 3-tier architecture (ASP.NET Core MVC, EF Core) combined with Repository Pattern and Unit of Work Pattern. Regarding infrastructure architecture, it is designed according to the AWS Well-Architected Framework, running on AWS Elastic Beanstalk (for .NET backend), Amazon RDS for SQL Server (for database), and Amazon S3 (for static assets). The system is performance-optimized using CloudFront and ElastiCache, and secured using WAF, VPC, and NAT Gateway. The deployment process is fully automated using a CI/CD pipeline integrated with GitHub.

Business benefits are immediate, including automated inventory management (reducing loss) and opening a new online revenue channel. Regarding investment, infrastructure costs in the first 12 months are nearly zero by leveraging AWS Free Tier (e.g., RDS Express Edition, EC2 t3.micro). Long-term operating costs (after Free Tier) are also very practical, estimated at around 138.06 USD/month for the entire system. With minimal initial investment and the ability to directly solve problems causing revenue loss, ROI is very high and almost instant.

The project is proposed to be implemented in 11 weeks, divided into 4 main phases: (1) Foundation & Architecture, (2) Core Feature Development, (3) AWS Integration & CI/CD, and (4) Finalization & Deployment. Expected results are measured by specific success metrics: reduce inventory errors by 90%, reduce checkout time by 50%, and achieve 20% revenue from online channels in the first 6 months. This solution not only solves immediate problems but also provides mini-markets with a scalable platform for data-driven decision-making in the future.

### 2. Problem Statement

_Current Problem_  
Small and medium retail businesses, especially the traditional "mini-market" model in Vietnam, are operating based on outdated manual processes. In the context of an increasingly digitized market, failing to apply technology to the work environment has created several problems, directly affecting their survival and growth.

_Key Problems_

- **Manual inventory management leads to inaccuracy in figures and resource waste:** Most mini-markets currently manage thousands of product codes (SKUs) using notebooks or Excel files. Importing/exporting goods and end-of-day inventory checks rely entirely on manual counting and entry. This can lead to data errors because the manual entry process is prone to mistakes, typically product codes and quantities, causing significant discrepancies between "on book" data and "actual" stock. Checking goods manually like this also demands high resources as staff have to spend hours every day counting, reconciling, and correcting reports, instead of focusing on sales or customer service. Finally, financial loss occurs; when data is not entered accurately, store owners cannot control the condition of goods such as expired goods, damaged goods, or theft, leading to a loss of 5-10% of inventory value monthly.
- **Dependence on offline sales, missing the E-commerce market:** Typically, mini-markets in Vietnam mostly depend on walk-in customers (offline). They are also limited by geographical location (only serving the local area) and a familiar customer base. Stores like this stand outside the E-commerce market, missing the young customer base already accustomed to online shopping. They cannot compete on convenience such as 24/7 ordering or door-to-door delivery compared to large convenience store chains like Circle K or 7-Eleven and delivery apps like Grab and Shopee, leading to a risk of losing customers over time.
- **Operational and customer experience issues:** The payment and information lookup process in traditional mini-markets is usually very slow. When customers ask about price, product information, or promotions, staff (especially new staff) have to manually look up in notebooks, leading to wasted time. Making customers wait long for information lookup or checkout creates frustration and unprofessionalism. Staff waste too much time on simple, error-prone tasks (such as misreading prices due to bad handwriting), reducing the number of customers that can be served during peak hours.

### 3. Solution Architecture

The architecture is designed to solve the stated problems by combining .NET 3-tier software architecture with AWS Managed Services. This architecture adheres to the principles of the AWS Well-Architected Framework, ensuring security, high performance, fault tolerance, and cost optimization.

![Mini-market Architecture](/images/2-Proposal/project_architecture_3.1.png)

_AWS Services Used_

- _AWS Elastic Beanstalk_: PaaS (Platform as a Service) selected to deploy the .NET 3-tier application (including WebShop Presentation Layer and Application Services Layer). Beanstalk automates 100% of infrastructure management, including automatically creating Auto Scaling Group (ASG) to ensure scalability and cost savings.

- _Amazon RDS (SQL Server)_: Database Service (Managed Relational Database Service) to host the Persistence Layer. SQL Server was chosen because the group's .NET application was developed and optimized for SQL Server. Using RDS for SQL Server allows migrating the application to AWS without changing code in the Data Layer. RDS will also automate complex tasks such as daily backups, patching, and failover. regarding security, RDS is placed in a Private Subnet, inaccessible directly from the Internet, only allowing the application on Beanstalk to connect. And regarding cost optimization, to optimize costs in the initial phase, we can start with SQL Server Express Edition on RDS, this version is within the AWS Free Tier.

- _Amazon S3_: Object Storage Service. Used to store static assets such as product images, CSS files, and JavaScript. S3 provides extremely low cost and unlimited scalability.

- _Amazon CloudFront_: Content Delivery Network Service - CDN. CloudFront caches static files from S3 at servers (Edge Locations) globally, helping users load pages significantly faster. It helps reduce direct load on Beanstalk servers and helps the .NET application focus on processing logic.

- _Amazon WAF and Route 53_: WAF (Web Application Firewall) and Route 53 (DNS Service). WAF is associated with CloudFront to block common web attacks (such as SQL injection, XSS). Route 53 provides domain names for users.

- _Amazon ElastiCache (Redis)_: In-memory data stores service. Helps maximize load reduction for RDS Database when there are repetitive queries (for example: retrieving the homepage product list). The .NET application will cache these "hot" data on ElastiCache, helping increase response speed. Similar to RDS, ElastiCache is also placed in a Private Subnet to ensure safety.

- _NAT Gateway_: Network Address Translation Service. NAT will provide secure Internet access for services in the Private Subnet (like Elastic Beanstalk). This allows servers to download security patches without being accessed directly from outside.

- _AWS CodePipeline/CodeBuild_: Continuous Integration and Deployment (CI/CD) services. These services are integrated with GitLab to automate the process: (1) CodeBuild builds .NET code, (2) CodePipeline deploys the new version to Elastic Beanstalk.

_Data Flow_

- [1]-[2] Users access the domain name (via Route 53) and are routed to CloudFront. Amazon WAF will filter this request.

- [3] (Static Flow) If it is a static file (image, css), CloudFront retrieves directly from Amazon S3.

- [4]-[6] (Dynamic Flow) If it is a dynamic request, CloudFront forwards via Internet Gateway to Application Load Balancer, then ALB sends the request to Elastic Beanstalk.

- [7]-[8] The .NET application (on Beanstalk) will check ElastiCache first, if not found, will query Amazon RDS.

- [9]-[10] When Elastic Beanstalk needs Internet access (to download patches), it will go through NAT Gateway then out to Internet Gateway.

- [11]-[14] (CI/CD Flow) When Dev pushes code to Github, CodePipeline and CodeBuild will automatically build and deploy the new version to Elastic Beanstalk.

### 4. Technical Implementation

_Implementation Stages_  
The project will be divided into 4 main stages, lasting 11 weeks to ensure progress and quality:

1. _Building Technical Foundation_: Focus on building the technical foundation, including finalizing the data model for main entities, setting up .NET 3-tier solution structure (Domain, Application, Persistence, WebShop), initializing repository on Github, and researching AWS services. (Weeks 1-4)

2. _Building Core Features_: Complete Persistence Layer (Repositories, Unit of Work) and Application Layer (Services) for main tasks such as managing products, users, and orders. Simultaneously, WebShop Layer (Controllers, Views) will be built for login flows, shopping cart, payment, and start writing Unit Tests for Services. (Weeks 5-7)

3. _Integrating AWS Services_: Integrate Amazon S3 for product images, ElastiCache (Redis) for caching. The group will also complete the CI/CD Pipeline to automatically deploy to the Staging environment on Elastic Beanstalk and perform Integration Testing. (Weeks 8-10)

4. _Finalization and Deployment_: Configure security services such as CloudFront, WAF, and Route 53. Then, deploy version 1.0 to Elastic Beanstalk, perform final UAT, and set up basic monitoring via CloudWatch. (Week 11)

_Technical Requirements_

- _Backend_: ASP.NET Core MVC 9.0.
- _ORM_: Entity Framework Core.
- _Database_: MS SQL Server 2022 (Local) and Amazon RDS for SQL Server (Cloud).
- _Frontend_: Bootstrap 5, jQuery, and Bootstrap Icons.
- _Cloud Platform (AWS)_: Elastic Beanstalk, RDS, S3, CloudFront, WAF, Route 53, ElastiCache, VPC, NAT Gateway, CodePipeline, CodeBuild.
- _Source Control_: Git.
- _Tools_: Visual Studio 2022, Docker Desktop.

_Development Methodology_

Apply Agile methodology (Scrum-like) to flexibly adjust according to requirements and ensure progress, adhering to the 4 proposed deployment stages. All work (features, bugs) will be tracked and managed via Kanban board, helping the group easily grasp the progress of each task (e.g., To Do, In Progress, Done). All new code must be reviewed via merge requests on Github before being merged into the main branch, ensuring consistent code quality.

_Testing Strategy_

To ensure quality and stability, the group will perform 3 levels of testing. First is Unit Testing, focusing 100% on the Application Layer (e.g., ProductService, OrderService) by mocking repositories to isolate Business Logic, using standard .NET testing frameworks. The second level is Integration Testing, performed on the Staging environment (on Elastic Beanstalk) to check the interaction between the Application Layer and Persistence Layer (EF Core) with the real Amazon RDS Database. Finally, User Acceptance Testing will be performed on the Production environment for the group to check complete functional flows on the user interface such as "Register, Login, Payment".

_Deployment Plan_

Apply fully automated CI/CD process. The process is automatically triggered whenever Dev pushes code to Github. Github will send a webhook triggering AWS CodePipeline, this service will take the code and command AWS CodeBuild to compile the .NET project, run Unit Tests, and package the application into a .zip file. If CodeBuild succeeds, CodePipeline will take the .zip file and automatically deploy this new version to the Staging environment on Elastic Beanstalk.

### 5. Roadmap & Milestones

The project is planned to be executed in 11 weeks, divided into 4 main stages. This schedule ensures time for development, integration, and thorough testing.

Phase 1 (Weeks 1 - 4): This stage focuses on building the technical foundation, including finalizing data models, setting up .NET 3-tier Solution Architecture, initializing Github Repository, and researching AWS services. The milestone of this stage is Solution Architecture and Repository established, along with AWS environment (VPC, Subnets).

Phase 2 (Weeks 5 - 7): After Phase 1 is complete, the group will build core features, complete Persistence and Application Layers (Product Management, Orders) and basic feature flows on WebShop (Auth, Cart). The milestone is main feature flows (Login, View Product, Cart, Payment) operating stably locally, and Unit Tests for Services.

Phase 3 (Weeks 8 - 10): Dependent on Phase 2, this stage will integrate AWS services like Amazon S3 for product images and ElastiCache (Redis) for caching. The milestone is CI/CD pipeline operational, successfully automatically deploying to Staging environment on Elastic Beanstalk and Integration Testing completed.

Phase 4 (Week 11): This final stage focuses on finalization and deployment, dependent on the stable Staging build from Phase 3. The group will configure security services (CloudFront, WAF, Route 53). The milestone is Version 1.0 successfully deployed to Production environment (Elastic Beanstalk), final User Acceptance Testing completed, and system monitored via CloudWatch.

### 6. Budget Estimation

Costs can be viewed on [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=d9984dcf32da018859c676f29d2d4d255a2933ca)  
Or download [budget estimation file](/attachments/pricing.pdf).

_Infrastructure Costs_

Estimates from AWS Pricing Calculator show the monthly operating cost of this architecture is 138.06 USD, with an upfront cost of 0.00 USD. The group's cost optimization strategy focuses on maximizing AWS Free Tier usage and managed services. The figure of 138.06 USD/month is a realistic cost for long-term operation (after 12 months) of a complete, scalable, and highly secure e-commerce system.

Costs for Elastic Beanstalk are broken down into the resources it manages: Amazon EC2 (1 instance t3a.small) costing 19.15 USD/month and Elastic Load Balancing (1 Application Load Balancer) costing 18.69 USD/month. Amazon VPC service costs 46.02 USD/month, this is the cost for 1 NAT Gateway, a mandatory component for Elastic Beanstalk's Private Subnet security design. Regarding database and cache, Amazon RDS for SQL Server (Express Edition version on db.t3.micro) costs 25.86 USD/month, and Amazon ElastiCache (cache.t4g.micro) is 17.52 USD/month. Security and CI/CD services like WAF (7.20 USD/month), CloudFront (2.43 USD/month), Route 53 (0.90 USD/month), S3 (0.17 USD/month), and CodeBuild (0.12 USD/month) make up the remaining costs.

The most important cost optimization strategy is leveraging AWS Free Tier in the first 12 months. Although the total estimate is 138.06 USD/month, many core services herein (including EC2 t3a.small, RDS db.t3.micro, ElastiCache t4g.micro, S3, and CloudFront) are within the Free tier free for 12 months. Therefore, the actual operating cost in the first year will be significantly lower, mainly consisting of costs for NAT Gateway (46.02 USD) and WAF (7.20 USD).

Regarding ROI calculation, and initial investment is nearly zero (as infrastructure costs are covered within Free Tier and development costs are the group's effort during the internship). Profit is almost immediate, as the solution directly solves revenue loss problems (from manual inventory management) and missed market opportunities (due to offline-only sales) stated in the Problem Statement (Part 1). Therefore, ROI (return on investment) is very high.

### 7. Risk Assessment

Some potential risks lie in three areas: Technical, Business, and Operational. Thus a mitigation plan and contingency plan have been prepared for high-impact risks.

1. Technical: System Overload (Performance Bottleneck):

   - Impact: **High** | Probability: **Medium**
   - This is the risk of the system being slow or crashing when there is high traffic volume. The group's mitigation strategy is to use ElastiCache to offload queries for RDS, configure Auto Scaling (in Elastic Beanstalk) with reasonable triggers (e.g., CPU > 70%), and use CloudFront to cache static files. The contingency plan is to use CloudWatch Alarms for immediate alerts, and if RDS overloads, the group will perform vertical scaling of the RDS instance immediately.

2. Business: Low User Adoption:

   - Impact: **High** | Probability: **Medium**
   - This is the risk that mini-market owners find the solution too complex and do not use it. To mitigate this risk, the group will stick to a simple frontend design (Bootstrap), gather shop owner feedback early from Phase 2, and provide instruction manuals. The contingency plan is if adoption is low after deployment, the group will perform an additional Sprint (Phase 5) to prioritize adjusting features based on gathered feedback.

3. Operational: Data Loss / Breach:

   - Impact: **Critical** | Probability: **Low**
   - The group's mitigation strategy is to configure RDS for automatic daily backups, place RDS and Beanstalk in Private Subnet, use WAF to block attacks, and manage connection strings via Beanstalk environment variables. The contingency plan is if data is lost, the group will perform Point-in-Time Recovery (PITR) immediately from RDS backup.

### 8. Expected Outcomes

The goal of this solution is to directly address the problems stated in the Problem Statement section. Regarding business metrics, the group expects to reduce inventory management errors by 90% (compared to Excel/notebooks), reduce checkout time at the counter by 50%, and achieve at least 20% new revenue from online channels in the first 6 months. Regarding technical metrics, the goal is to maintain 99.9% uptime, ensure average page load time under 2 seconds (thanks to CloudFront and ElastiCache), and stable deployment frequency via CI/CD pipeline.

Benefits are expected to increase over time. In the short-term (0-6 months), mini-market owners will see immediately improved user experience and significantly improved operations (automated inventory management). In the medium-term (6-18 months), value comes from market expansion (reaching online customers) and starting to collect valuable business data. Long-term value and strategic capabilities gained are the ability to make data-driven decisions (e.g., knowing which products sell well) and easy system scalability (adding more new stores) thanks to Solution Architecture on Elastic Beanstalk and RDS.