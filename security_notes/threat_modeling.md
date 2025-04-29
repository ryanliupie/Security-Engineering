## What is Threat Modeling? 

Threat modeling is the process of identifying threats against a system, application, or network. 

Believe it or not, it's something that humans naturally do everyday. For example, turning left at the lights. You have to aks yourself: 
- Are there pedestrians crossing? 
- Is there incoming traffic ahead? 
- Is the light still green/turning yellow/turning red? 
- Is it safe to turn? 
- Are you turning fast enough to not get hit? 

<hr>

###  Threat Modeling Framework

This framework helps us organize threat modeling which consists of the following:

- <b>Assess Scope:</b> What are working on? It could be a large system, or small system. We have to understand the assets, value of them etc..

- <b>Identify what can go wrong:</b> What are the threats to the system? How much money would a company lose if a threat exploited "x"? This is where you brainstorm everything that can go wrong. 

- <b>Identify countermeasures:</b> Once these threats are identifed, what are you going to do about them? This may involve approaching approaches to risk management such as accepting the risk, transfer or mitigate it; if this system participates is high risk activites, it may be time to avoid them. 

- <b>Assessing work:</b> Did you do a good job? What went well/wrong? What can you do to improve next time? It is important to assess your solutions because if this happens again, you know what to do and how to make them more efficient and stronger. 

<hr>

###  Implementation with SDLC

SDLC stands for "Software Development Lifecycle" which explain the steps how software is made. There are different types of ways to create software such as waterfall, agile, iterative etc.. but Swe will just cover the steps and how security is involved. 

- <b>Planning:</b> This involves gathering information and tools we will need. For example, defining scope, setting goals, SDEs, cost estimation and allocation, customer input, internal/external experts, managers overseeing project, etc.. 

- <b>Requirements & Analysis:</b> This is similar to step one, but this dig a bit deeper into the granular details where every single person understands. This can include, the hardware, OS, containers, programming, security, and understanding what the customer exactly wants. 

- <b>Design:</b> The software developers analyze the requirements and come up with the best solution to create the software. They try to integrate the software into the existing infrastructure the easiest way possible. Once they come up with a solution, they present it all stakeholders to make sure everything is aligned because small mistakes can change the trajectory of every componenet. 

    - 🚨*Threat Modeling*🚨- Before writing any code, it is important to understand all the threats to mitigate and anticipate those first. This is apart of the `shift-left` practice which emphasizes integrating security practices and considerations early on in the SDLC. 

- <b>Implementation:</b> Once the design is confirmed, the software developers would break tasks into smaller chunks to achieve the final result. This is longest stage as the developers need to account for all the requirements and follow pre-defined coding guidlines. 

    - 🚨*Code Review/Static Code Analysis*🚨 - This is the analysis of source code without executing it to find bugs, vulnerabilites, or code quality issues. We can check for SQL injection risks, XSS risks, hardcoded secrets (passwords, keys), unused variables, insecure API packages, buffer overflows (in C/C++), etc... 

- <b>Testing:</b> Once the software is built, it is deployed in the testing environment. The testing team ensures that the entire application works according to the customer requirements. Once tested, the quality analysis (QA) team would look for any bugs or defects and communicate them with the developer. The development team would fix then the QA would re-test it until everything is fixed and ready for the next stage. 

    - 🚨*Pentests/Dynamic Code Analysis*🚨 - Performing a simulated cyberattack on the application allows for missed exploitable vulnerabilites, where there can be black box, grey box, and white box tests. Moreover, dynamic code analysis is the analysis of a program while it's running to find vulnerabilites that only appear during execution such as input validation flaws, insecure session handling, authentication/authorization issues etc...  

- <b>Deployment:</b> Once the testing is completed, the software is ready for deployment for customers to use. Usually when new users sign up, they are guided with some type of simple training to get started. 

    - 🚨*CICD Automated Security Checks*🚨 - Within the CICD pipline right before and during code deployment, some security checks are performed. This includes the use of SAST tools (SonarQuebe, Semgrep), DAST tools (OWASP ZAP), and SCA tools --> Software Composition Analysis (Synk, OWASP Dependency-Check). 

    - Detect leaked secrets with tools such as Gitleaks, TruffleHog, or GitHub Secret Scanning. 

    - If deployement is on Docker/Kubernetes, you can use tools such as Aqua, Anchore, or Clair to scan container images for OS/package vulnerablites, and enforce policies such as no root user in containers. 

    - Scanning (IaC) with tools such as Checkov, Terrascan, or AWS Config to make sure there are no misconfigurations. 

    - Implement "fail deploy" with security polices/gates if certain criteria is not met such as compliance violation, secrets found, or a certain number of vulnerabilites and its severity exceed a certain amount. 

    - Signed Artifacts: The process of digitally signing code, binaries, containers, and other forms of digital products to ensure its integrity and authentcity. You can use tools such as Cosign or Sigstore to prevent tampering. 

- <b>Maintenance:</b> In this phase, this is where teams fix bugs, resolve customer complaints, and manage change management of software. Also, teams monitor system performance, security issues, and user experience to figure out ways to improve. 

    - 🚨*Disclose/Bug Bounty*🚨 - If people such as security researchers, customers, or internal engineers find vulnerabilites, they can provide responsible ways of reporting it to the company. Some companies will pay people if they find something where platforms HackerOne help manage these programs. 
    



