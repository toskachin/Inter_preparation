
## PayPay 5 senses
* Believes in our PRODUCT & TEAM
  * Enjoy growing together, believing in the product,our teams and that we will be the no.1 fintech company in the world.
* SPEED is our bet on the market
  * Contribute to our users and the company by achieving results at an amazing speed
* Ego is not welcome,Communication is necessary
  * Value communications and respect diversity of our colleagues.Work together and tackle tasks as one.
* Be Sincere To be Professional
  * Be sincere and work as a professional without making any compromise.Create new opportunities and values. Go through till the end.
* Work for LIFE or Work for Rice
  * Take action, looking for essential objectives and values.Try to take risks again and again and never stop challenging.



## Question from hunter

* Main scrop of SRE role
  * we help Marin maintence our product platform also inclouding test env, it provides platform statility for  Marin
  * We build and deliver two generation CI/CD help internal teams deploy code to different envs and save 50%, automation deployment process
  * We system several monitor system to monitor platform, backend OS system, also product data. it helps find issue proactively, it also reduce people work to track system, it also share useful data like PMs, salses team 
  * We maintence AWS platform and hybrid cloud platform and automation deployment and monitor system. 
  





* Can you describe your experience with microservice architectures running on
Kubernetes and AWS?
- we have one labs env K8s platform we use for testing our product on K8s, we deploy pods, installed nodes, build network and all essential compents 
- for AWS we have one product call Tracker on AWS, we help developer team build the cloud platform and help them deploy code to cloud platform 


* What challenges have you faced in maintaining high availability in production
environments?
- We build the system with hig availablitity, the challenges is to 
- set up monitor system to track which metris and 
- set up SLAs
- investigate buisness impact 

* How do you go about analyzing system bottlenecks?
- general we use linux commands and monitor system to find bottlenecks
- for linux we also use perf tool to find the bootleneck


* Can you share an example where your insights led to performance
improvements?
- from Network side, improve the netwok latency
- evaluate system usage and provide enough rescourses for the system 


* What tools and methods have you used for monitoring system performance and
generating alerts? How did these improve observability and visibility within the
system?
- we use grafanna to improve the visibility 
- we also use filebeat  to collect logs and show on Kibana for observability 
- 


* Explain a situation where you had to pre-emptively verify failure scenarios
-  we have internal application github, it's all company product code, it's important, I've asked to find disaster backup plan
-  I implement VM backup snapshot 
-  we use github offical backup tool backup data to other location.
-  we performce backup demo to test backup data is correct  

* What steps did you take to reduce Mean Time To Recovery (MTTR)?
- once the incident happens, we will do postmoderm to summary the incident and how to reduce the reponse time.
- we create document to share more knowledge cross difference teams 
- 


* Discuss your experience in developing solutions aimed at ensuring high
availability, scalability, and resilience of systems.

- we disgn and build AWS platform provide high availablitity and scalability 
- we set up CDN for multipule regions also set up datacenter as backup
- we build system under Loadbalance system it provides 99.9% of availability and easily add/remove ec2 servers behind loadbalance.


* How do you establish and measure Service Level Agreements (SLAs) for
uptime? Can you give an example of how you've integrated these with telemetry
and alerting platforms?
- We setup on Montroing system and oncal page system, it will page difference teams with SLAs
- Nagios system and grafanna will send email to the address of opsgenie or pageduty 

* Can you describe a project where you successfully implemented industry best
practices for system development, configuration management, and
deployment?

- For GoCD system, once we onboarded the tool for developers and have meeting with developer and QA teams to share how to use it. 
- Also explians deatiled system like how to find consol. 
- Create WIKI document for commend issues 
- Custom GoCD pipelines to fit business demand

* Which programming languages are you most proficient in for software
development (Python, Java, Go, etc.)? How have these skills helped you in an
SRE role?
-  two pointes 
   -  use python to create automation tools to grather data for monitoring, also automate daily work 
   -  use python to interagte three parties tools like CI/CD system

* What is your experience with different databases, such as RDS, NoSQL, and
distributed TiDB? How have you leveraged these technologies for optimal
performance?
- differece of RDS and NoSQL, one is relational database, and NoSQL only has key-value, like redies, it improve the perforamance 


* Can you share an example of a technical challenge you took up and drove to
conclusion within your team? How did you ensure seamless communication
and knowledge sharing among team members?
- one is that the issue of our Tracker application, we use Flume and Flume it gots issue, it stuck at data sync, it's P1 incident. 
- I quickly find the root cause and share temp solution for developers team 
- communication with developer team to find backfill solution for customer
- Create document/WIKI and  write incident post mortem to summry the incident and how to avoid it happens next time. 