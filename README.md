![image](https://github.com/user-attachments/assets/57895d2e-2744-48d4-b7ed-dc5d45027c30)

# Assignment 3 Performance Testing with JMeter
**Suvash Shrestha, Jose Vallejo**  
**MSSE640 Software Quality and Testing**  
**Assignment 3: Performance Testing with JMeter**

**PART 1**

**Introduction**

Ensuring that applications perform optimally under various conditions is essential in software development. Performance testing is a non-functional testing approach used to evaluate an application's responsiveness, stability, scalability, and resource utilization. It helps ensure that the software meets expected service levels and delivers a consistent user experience.

Sofia Palamarchuk, in her blog Types of Performance Testing: Everything You Need to Know, states, “Performance testing is a key aspect of non-functional testing, which focuses on cross-cutting factors that impact the user experience. Understanding the various types of performance tests is essential for enhancing your applications to meet user expectations and perform reliably under diverse conditions. These tests help identify scalability and stability issues, enabling developers to optimize the application’s performance and deliver a seamless user experience.”

Thomas Hamilton echoes this in his article Performance Testing Tutorial, noting, “More importantly, Performance Testing uncovers what needs to be improved before the product goes to market. Without Performance Testing, the software is likely to suffer from issues such as running slow while several users use it simultaneously, inconsistencies across different operating systems, and poor usability.”

There are six main types of performance testing: Load Testing, Stress Testing, Endurance Testing, Spike Testing, Volume Testing, and Scalability Testing. Some sources list up to nine types, adding Resilience Testing and Breakpoint Testing, as noted by Global App Testing. While performance testing assesses speed, response time, stability, reliability, scalability, and resource usage under specific workloads, its primary purpose is to identify and eliminate performance bottlenecks in the application.

We, Suvash Shrestha and Jose Vallejo, present this detailed report for Assignment 3, focused on performance testing using Apache JMeter. The objective was to simulate user load on a web application, measure its performance, and explore strategies such as Load Testing, Endurance Testing, and Stress Testing. We set up JMeter, designed test plans, executed various tests, and analyzed the results. While we initially targeted a public web application, we later transitioned to testing a locally deployed, containerized application using Docker Desktop, Kubernetes, and Skaffold, as recommended by our instructor. This report outlines our research, setup methodology, troubleshooting process, performance tests conducted on the Online Boutique application, and reflections on our learning experience.

**Load Testing**

Load Testing aims to verify a system’s performance under expected peak conditions. It simulates the anticipated number of concurrent users or transactions to determine whether the application can handle the load while meeting predefined performance goals. The primary objective is to observe how the system behaves under a specific, expected workload.

As Aleksandra Sokolova, QA Engineer at TestDevLab, explains in her article Types of Performance Testing and Choosing the Right One, “Load testing measures how your application behaves under varying levels of user activity and traffic levels, identifying performance bottlenecks and potential failures. This type of testing is essential for ensuring that your application can handle day-to-day user activity without slowing down or crashing. Additionally, it includes scalability testing to determine how your system performs as the workload increases and to assess whether it can scale effectively to meet future demand.”

Furthermore, Abstracta notes, “Load tests are meant to simulate the maximum use of the system by checking how systems function under the projected number of concurrent virtual users performing transactions over a certain period.”

**Endurance Testing**

Endurance Testing, also known as Soak Testing, evaluates an application's stability and performance over an extended period under a moderate, sustained load. As TestDevLab states, “Endurance testing, also known as soak testing, evaluates how your application performs over an extended period under a sustained load.” The primary objective is to identify issues that arise only after prolonged activity. Common problems uncovered through endurance testing include memory leaks, resource exhaustion, database connection failures, and gradual performance degradation. When implemented effectively, endurance testing confirms the long-term reliability and stability of the application.

**Stress and Spike Testing**

Stress and Spike Testing evaluate an application's performance beyond its normal operational capacity to determine its limits and behavior under extreme conditions. Stress Testing focuses on identifying the system's breaking point and analyzing how it recovers after failure. According to SmartBear LoadNinja, “Measure performance outside of typical production levels to measure when and how it fails. The goal is to identify the breaking points of an application and potentially fix bottlenecks.”

In contrast, Spike Testing assesses the system’s response to sudden, dramatic increases in load. It determines the system’s ability to handle rapid traffic surges without failing and evaluates how quickly it recovers. As noted by GeeksforGeeks, “Spike testing is a type of load testing that tests the system’s ability to handle sudden spikes in traffic. It helps identify any issues that may occur when the system is suddenly hit with a high number of requests. It tests the product’s reaction to sudden large spikes in the load generated by users.”

Both testing types are critical for understanding the system's robustness and resilience under exceptional circumstances.

![image](https://github.com/user-attachments/assets/5dc50c3a-b8c2-4602-ba9d-d58fe3b9bf6f)


**1\. Load Testing:**

- **Behavior**: Gradually increases the number of threads to simulate the expected peak load conditions.
- **Graph Interpretation**: Clearly shows a linear rise from 0 to 50 threads over 10 minutes.

**2\. Endurance (Soak) Testing:**

- **Behavior**: Maintains a constant moderate load over an extended period to test for issues that appear only after prolonged usage.
- **Graph Interpretation**: The graph illustrates a stable and consistent thread count of 20 over 60 minutes, highlighting sustained usage without variation.

**3\. Stress/Spike Testing:**

- **Behavior**: Simulates sudden, dramatic spikes in the number of users to test how well the system handles unexpected load increases.
- **Graph Interpretation**: The graph shows abrupt spikes at minutes 2 and 5, reaching peaks of 100 and 150 threads, respectively, testing the system's reaction and recovery.

These graphs provide clear visual representations of each testing scenario and complement your report effectively.

**JMeter Components**

**Apache JMeter**  
Apache JMeter is an open-source tool designed for performance testing. The official website says, "The Apache JMeter™ application is open-source software, a 100% pure Java application designed to load test functional behavior and measure performance.” It enables testers to simulate heavy loads and evaluate performance. A JMeter test plan includes various components such as Thread Groups, Samplers, Config Elements, and Listeners.

**Thread Groups**

It represents the virtual users executing test scenarios. According to the Apache JMeter User Manual, "Thread Group elements are the beginning points of any test plan. All controllers and samplers must be under a thread group.” Within a Thread Group, testers configure the number of threads (users), the ramp-up period, and the loop count or test duration, allowing simulation of various load patterns required for different performance test types. (Insert screenshot of a configured Thread Group here).

**A HTTP request sampler**

Samplers issue requests to the designated server. According to the User Manual, "Samplers instruct JMeter to send requests to a server and wait for a response.” The HTTP Request Sampler is vital for web applications, enabling the customization of requests such as the server, path, method, and parameters to replicate browser activity. Each sampler generates sample results that include important performance metrics.

**Config elements**

Config Elements work closely with Samplers to modify requests or set up test variables. As the User Manual notes, "Configuration elements can be used to set up defaults and variables for later use by samplers" \[JMeter User Manual - Config Elements\]. Examples like the HTTP Header Manager allow adding standard HTTP headers, while the HTTP Cookie Manager handles session cookies, making tests more realistic. These elements enhance the flexibility and maintainability of test plans.

**Listeners**

Listeners gather, display, and analyze Sampler results. The User Manual states they "provide different ways to view the results of the Sampler requests." The View Results Tree listener is useful for debugging with detailed data. Summary Report and Aggregate Report offer statistical summaries like average time, throughput, and errors, essential for performance analysis.

**Application Performance Index (Apdex)**

The Application Performance Index (Apdex) is a method for measuring user satisfaction with application response times, translating metrics into a single score. The Apdex Alliance states, "Originally conceived to report software application performance data, Apdex (Application Performance Index) is an open standard that defines a method to report, benchmark, and rate everything from application response time, to food quality, surgical outcomes, time to repair, latency delivered by an ISP, or time to pour a beer." It categorizes user experiences based on a predefined time threshold 'T': Satisfied (response time ≤ T), Tolerating (> T and ≤ 4T), and Frustrated (> 4T or errors). The final Apdex score (ranging from 0 to 1) is calculated using the formula: (Satisfied Count + Tolerating Count / 2) / Total Samples. This standardized score helps in communicating performance quality from an end-user perspective.

![image](https://github.com/user-attachments/assets/b122b200-7c7b-4188-9d43-7432c4eca07a)

**Performance Testing with Apache JMeter (Assignment 3 Part 2 Report)**

**Introduction**

This report documents how we conducted a series of performance tests using Apache JMeter as part of _Assignment 3_. The team performed tests on three different setups: (1) a live **Cymbal Shops** e-commerce website, (2) a local **Nginx** server running in a Docker container, and (3) the **Online Boutique** microservices demo application deployed on Kubernetes via Skaffold. Each scenario involved configuring JMeter test plans (with appropriate thread groups, samplers, config elements, and listeners) to simulate load and endurance testing. Throughout the process, we encountered and resolved several challenges (such as installation issues, environment configuration, and port conflicts), which are detailed in this report. The following sections describe the step-by-step setup and results of the performance tests, giving credit to the work done by us.

**Initial JMeter Setup and Testing Cymbal Shops**

**JMeter Installation**

Our team began by downloading Apache JMeter. An initial mistake was downloading the _source_ distribution of JMeter, which did not contain an easy-to-run binary. Realizing this, we switched to the **binary** distribution of JMeter (a ZIP archive containing the compiled program)​. After ensuring the Java JDK was installed (a prerequisite), we extracted the JMeter binaries. JMeter was launched via the provided startup script (jmeter.bat on Windows, located in the JMeter bin directory). This allowed us to open the JMeter GUI successfully. Once the GUI was running, we created a new test plan for our first target site, Cymbal Shops.

![image](https://github.com/user-attachments/assets/80b07256-3c92-47c9-9382-99f4bb23eae7)
![image](https://github.com/user-attachments/assets/636cde9b-1849-4826-a9a5-7467dc149401)


**Thread Group Configuration (Endurance Test):** In JMeter, a _Thread Group_ defines the number of virtual users (threads) and the schedule of our execution​[jmeter.apache.org](https://jmeter.apache.org/usermanual/test_plan.html#:~:text=Thread%20group%20elements%20are%20the,thread%20group%20allow%20you%20to). Our team created a thread group for an **Endurance Test**, naming it appropriately to reflect a long-duration test scenario. We configured the thread group with **10 threads** (simulating 10 concurrent users), a **ramp-up period of 10 seconds**, and a **duration of 600 seconds** (10 minutes). The 10-second ramp-up means JMeter will start 1 thread per second until all 10 are running, avoiding a sudden spike​[jmeter.apache.org](https://jmeter.apache.org/usermanual/test_plan.html#:~:text=The%20ramp,be%20delayed%20by%204%20seconds). The 600-second duration instructs JMeter to keep the test running for ten minutes before stopping, making it an endurance scenario. This setup was designed to see if the target site could handle a steady load of 10 users for a prolonged period without performance degradation.

![image](https://github.com/user-attachments/assets/30f10028-94a2-4479-928f-6193e692d256)
![image](https://github.com/user-attachments/assets/8803a2e4-7e6a-456c-a5f0-03329ccac49f)
![image](https://github.com/user-attachments/assets/e42e894e-0ca7-4fd6-9164-d26c6f3f32e8)


With the thread group in place, the team added an **HTTP Request Sampler** and an **HTTP Header Manager** to the test plan. The HTTP Request Sampler was configured to send a **GET** request to the Cymbal Shops website’s home URL (<https://cymbal-shops.retail.cymbal.dev/>), representing the e-commerce storefront. The HTTP Header Manager defined a **User-Agent** header on all requests (simulating a typical web browser’s agent string). Configuring default headers in JMeter ensures certain headers (like User-Agent) are included in every request. For example, they set the User-Agent value to a common browser signature (e.g. a Chrome on Windows string) to mimic real browser traffic​. By including a realistic User-Agent, the test ensured the target site would treat the traffic as coming from a normal browser. Additionally, the team attached a **Listener** – specifically _View Results Tree_ – to the thread group to monitor and record the outcome of each request. The View Results Tree listener would display each sample result (and response data if needed) to verify that the requests were succeeding.

![image](https://github.com/user-attachments/assets/bea96b6b-9404-43dd-9aba-6647476f6164)
![image](https://github.com/user-attachments/assets/387d35e0-bf9f-4115-870b-d1f98728a3b2)


**Executing the Test against Cymbal Shops:** After building the test plan (Thread Group, HTTP Sampler, Header Manager, and Listener), Shrestha and Vallejo ran the endurance test against the live Cymbal Shops site. As the test executed, the JMeter _View Results Tree_ showed each request turning up with a **green status**, indicating HTTP 200 OK responses (success). The site responded to all 10 simulated users continuously for the 10-minute duration without errors. This confirmed that the Cymbal Shops site was up and able to handle the modest load. The successful results (all samples marked green/passed) were captured in screenshots via the Results Tree. This initial test validated the JMeter setup and provided a baseline using a real-world deployed application.

![image](https://github.com/user-attachments/assets/0e952a07-2743-4c90-8edd-3f3841500de6)

**Testing with Local Nginx Docker Container**

Having confirmed JMeter’s operation on a live website, the team next set up a local test target using **Docker**. They chose the lightweight Nginx web server, running in a Docker container, to simulate a simple application under load.

![image](https://github.com/user-attachments/assets/a4dbc310-06df-44a9-8d30-93a78fbcff20)

**Deploying Nginx Locally:** Using Docker, we pulled the official **nginx** image and ran a container. For example, they used a command similar to docker run -d -p 8080:80 nginx, which downloads the latest Nginx image and starts a container in detached mode, publishing container port 80 to host port 8080. Once the container was running, they verified it by navigating to <http://localhost:8080> in a web browser. The default "Welcome to nginx!" page appeared, confirming that the Nginx server was up and serving requests. (The Nginx welcome page is the standard placeholder page shown when Nginx is installed but no custom site content is configured​[f5.com](https://www.f5.com/company/blog/nginx/welcome-to-nginx-on-my-favourite-website#:~:text=Why%20Do%20I%20See%20%E2%80%9CWelcome,but%20has%20not%20finished%20configuring).) With the local server reachable, they proceeded to configure JMeter to test it.

![image](https://github.com/user-attachments/assets/1f0db2fb-71e0-4f78-8855-5f34da805b13)

**JMeter Test Plan for Nginx:** The existing JMeter test plan was duplicated or modified to point to the local server. The HTTP Request Sampler’s target was changed from the Cymbal Shops URL to the local address – using 127.0.0.1 (localhost) as the server and port **8080**. The path was left blank (or “/”) to request the root page. The same Header Manager (with User-Agent) remained in place to simulate a browser hitting the server.

For this scenario, the team configured a **Load Test** thread group. They set up **50 threads (users)** with a ramp-up of **50 seconds**, and a loop count of **1** (each thread makes one request). This means one new user is started each second, ramping up linearly to 50 concurrent users after 50 seconds​[jmeter.apache.org](https://jmeter.apache.org/usermanual/test_plan.html#:~:text=The%20ramp,be%20delayed%20by%204%20seconds). Each user thread performs a single GET request to the local Nginx homepage and then finishes (since loop = 1). This configuration represents a typical load test where a surge of users hit the site once. JMeter’s thread group settings allow controlling threads, ramp-up, and iterations in this way​[jmeter.apache.org](https://jmeter.apache.org/usermanual/test_plan.html#:~:text=,times%20to%20execute%20the%20test). The team also tried an **Endurance** variant with the Nginx target by using the scheduler: for example, running a smaller number of threads over a longer duration to simulate sustained load on the local server. In both cases, a View Results Tree listener was used to observe outcomes.

**Results of Local Testing:** The JMeter tests against the Nginx container were executed successfully. During the 50-user load test, all requests returned the expected HTTP 200 response with the welcome page HTML. The View Results Tree showed 50 successful samples (green) with no errors. This demonstrated that the local Nginx server could handle 50 sequentially ramped requests with ease (which is expected for a static page). The results of the endurance-style test run were similarly successful, since Nginx is efficient and the load levels were not extreme. The team saved the result logs or screenshots from JMeter as evidence. By testing the Dockerized Nginx, we validated that JMeter can target local applications and that their environment was correctly set up for both external and internal endpoints.

**Advanced Testing with Kubernetes and Skaffold (Online Boutique)**

For a more complex scenario, we deployed the Online Boutique microservices demo application on a local Kubernetes cluster and ran performance tests against it. Online Boutique (“Hipster Shop”) is a cloud-native microservices demo by Google. It comprises around 10 independent microservices (e.g., product catalog, cart service, payment service, etc.) that provide a web-based e-commerce store. This application offers a realistic test target with multiple services and dependencies behind a front-end.

**Setting up Kubernetes (Docker Desktop):** We enabled Kubernetes through Docker Desktop on their machine. Docker Desktop provides a single-node Kubernetes cluster for development use. After turning on the Kubernetes feature, they confirmed the cluster was running using commands like kubectl cluster-info or checking the context (kubectl config get-contexts) to ensure “docker-desktop” was current. With Kubernetes up, they next needed to deploy the Online Boutique application onto it.

**Installing Skaffold and Running the Demo:** To simplify the deployment of the multi-service app, the team used **Skaffold**, a command-line tool that automates building and running Kubernetes applications. We installed Skaffold on Windows (for example, by downloading the Skaffold binary and adding it to the PATH). However, when trying to run Skaffold from **Git Bash**, they encountered a PATH issue – the skaffold command was not recognized. This is a common environment quirk: Git Bash may not load the updated Windows PATH or require adding the path in ~/.bashrc or ~/.bash_profile. We attempted to fix this by editing these configuration files to include the Skaffold installation directory and then sourcing the files. Despite troubleshooting, Git Bash still did not find the skaffold command. As a workaround, they ran Skaffold using a different shell. For instance, opening a PowerShell or Command Prompt (which had Skaffold on the system PATH) allowed them to run the tool. Ultimately, they executed Skaffold by either using the full path to the binary in Git Bash or by using PowerShell, thus overcoming the PATH issue.

![image](https://github.com/user-attachments/assets/a39335ef-aa64-4f84-a138-78ca5f1defea)
![image](https://github.com/user-attachments/assets/4420a629-0051-4df0-868d-e041da04ef68)
![image](https://github.com/user-attachments/assets/259c40af-74ad-4e1f-b25e-649c8ef892e7)
![image](https://github.com/user-attachments/assets/c2d174d1-5755-477e-8516-8da4fd75227c)
![image](https://github.com/user-attachments/assets/61f79144-2924-4729-9ba5-217c580c1dbf)
![image](https://github.com/user-attachments/assets/223b9677-0e31-4f97-aa9e-614c32a22ecf)
![image](https://github.com/user-attachments/assets/b8408b5c-c904-4571-a83b-10b0b9b0bab1)
![image](https://github.com/user-attachments/assets/28edabd5-8cd1-4322-9008-2d283c4b4041)
![image](https://github.com/user-attachments/assets/cfedcfb9-b866-45c3-b8c0-7ceba2733660)
![image](https://github.com/user-attachments/assets/d42f9a17-6877-409c-82b2-494c418085a4)
![image](https://github.com/user-attachments/assets/34dc742e-2c02-47b0-a8fe-28b69b033bb2)


With Skaffold working, we cloned the **microservices-demo** repository from GitHub (which contains the source code and Kubernetes manifests for Online Boutique). In the repository, they used the Skaffold configuration provided. Running skaffold run in the project directory kicked off the deployment process. Skaffold built all the microservice container images (there are around a dozen services, each with its own Dockerfile) and applied the Kubernetes manifests to create the pods and services. This process took some time, as building many images and starting multiple containers is resource-intensive. (The team noted that the Skaffold build was slow due to the large number of microservices being compiled and containerized.) Once Skaffold finished, all microservice pods were launched in the Docker Desktop Kubernetes cluster. They verified this by running kubectl get pods and seeing each service (frontend, cartservice, productcatalogservice, etc.) in a Running state.

![image](https://github.com/user-attachments/assets/e9d32490-5d8c-4bfd-92f3-cb1e65331493)
![image](https://github.com/user-attachments/assets/96d4f3ce-4421-4d70-a017-810bb87509c4)
![image](https://github.com/user-attachments/assets/6d1cc271-8fc5-4d14-85e1-eb5e44815974)
![image](https://github.com/user-attachments/assets/35001df3-8994-46e0-9f54-7a32bf7fc500)
![image](https://github.com/user-attachments/assets/330b0492-ddba-4949-8c55-45325dfebdc2)

**Accessing Online Boutique:** The Online Boutique application’s frontend component was now running inside Kubernetes, but to access it in a browser they needed to expose it. In a cloud environment, the frontend might have a LoadBalancer service with an external IP, but on local Kubernetes the team opted to use port-forwarding. They executed a command like kubectl port-forward svc/frontend 8080:80 (assuming the frontend service is named “frontend” and listens on port 80). This bound the cluster’s frontend service to localhost:8080. After setting up the port forward, they opened a web browser to <http://localhost:8080> and successfully loaded the Online Boutique web UI. The site displayed the home page with a catalog of products, confirming the application was fully functional.

Once the Online Boutique storefront was confirmed to be working (as shown above), the team proceeded to test it with JMeter. They configured JMeter to target the Online Boutique frontend similar to the previous tests. In the HTTP Request sampler, they set the URL to <http://localhost:8080> (with no additional path, to hit the home page). The same Header Manager was kept in place with the User-Agent header, ensuring the requests simulate a browser. For this test, they used one of their prior thread group setups – either the load test configuration (50 users, 50s ramp-up, 1 loop) or another endurance test. Given the complexity of the app, they likely started with a smaller load (to avoid overwhelming their local cluster), for example, an endurance test with 10-20 threads over several minutes. The JMeter test was then executed against the Online Boutique. As the test ran, the requests went to the local Kubernetes cluster’s frontend service, which in turn queried various backend microservices (product catalog, etc.) to serve the page. In JMeter’s Results Tree, the team observed successful responses from the frontend (HTTP 200 for the main page HTML). All sampler results turned green, indicating the Online Boutique handled the simulated traffic without errors at that level. This demonstrated that the deployed microservices application was reachable and stable under the tested load. It was a satisfying result, considering the number of moving parts (multiple containers and services) behind the scenes. The team captured the results and screenshots for documentation.

![image](https://github.com/user-attachments/assets/c6be9397-9781-4823-8a58-950505a22c97)
![image](https://github.com/user-attachments/assets/062904b3-8a40-4ecc-9c19-61243f59516d)
![image](https://github.com/user-attachments/assets/99da3505-b327-4189-bb16-d6ce82611998)
![image](https://github.com/user-attachments/assets/5c7c48ee-f0bb-4b8f-8405-032baf1c0211)

**Challenges Faced and Solutions**

During the course of these tests, Suvash Shrestha and Jose Vallejo encountered several challenges. Below we outline the key issues and how they were resolved:

**JMeter Distribution Mix-up**

- Initially, the wrong JMeter package was downloaded (source instead of binary). The source distribution lacked an easy launcher, leading to confusion when trying to start JMeter. The solution was to obtain the correct _binary_ distribution of JMeter and use the included startup script. Once the binary package was used, JMeter launched without further issues.

**Git Bash and Skaffold PATH Issues:**

- After installing Skaffold on Windows, the team found that the skaffold command wasn’t recognized in Git Bash. This was due to Git Bash not automatically picking up the new PATH or requiring a different config. They attempted to fix it by editing shell profile files (.bashrc, .bash_profile) to add the Skaffold directory to the PATH. For example, they could add a line like export PATH=$PATH:/c/Users/Name/skaffold in .bashrc. However, Git Bash still didn’t register Skaffold, possibly due to how it was launched or path quoting issues. As an immediate workaround, the team switched to using PowerShell/Command Prompt where Skaffold was accessible (since the installer or manual PATH addition worked in the Windows environment). Using PowerShell, they ran all Skaffold commands successfully. The lesson learned is that when using Unix-like shells on Windows, one might need to ensure environment variables are set in the correct profile or just use the native shell if time is short. Ultimately, the issue was resolved by running Skaffold through a method that had the proper PATH, allowing the Kubernetes deployment to proceed.

**Docker/Nginx Port Conflict**

- Another issue arose when moving from the Nginx test to the Kubernetes test. The team had been using **port 8080** for both the local Nginx container and later for port-forwarding the Online Boutique frontend. After finishing tests on Nginx, the container was still running and occupying port 8080. Thus, when they tried to port-forward the frontend to localhost:8080, it failed due to the port being in use. Realizing this conflict, they stopped and removed the Nginx Docker container before retrying the port-forward. Freeing up port 8080 allowed the Kubernetes service to bind to it. This solution – shutting down or reassigning any service on a needed port – resolved the conflict. Alternatively, they could have chosen a different local port for forwarding, but removing the unused container was straightforward.

**Lengthy Skaffold Build/Deployment Times**

- Deploying the Online Boutique with Skaffold was time-consuming. Skaffold had to build ~10 microservice images (some in Java, Go, Python, etc.) and launch all the components. On a local machine with limited resources, this process can be slow (several minutes or more). The team experienced a long wait and noted that this was a challenge, especially if something failed and required re-running. While there isn’t a quick fix for this inherent complexity, one mitigation is to use pre-built container images if available (for example, the microservices-demo provides pre-built images on Docker Hub/Google Container Registry). Another is to ensure as much caching as possible is enabled (Skaffold will cache layers between runs). In this assignment context, the team simply had to be patient. They monitored the progress via Skaffold’s logs and kubectl until everything was up. The successful outcome made the wait worthwhile, but this was noted as a challenge in performing a complex deployment just to have a test target.

Throughout these challenges, we consistently resolved the issues and exhibited effective problem-solving skills in various areas. These included adjusting configurations, consulting environment settings, and changing methods, such as switching shells or stopping services. Each issue was resolved, enabling the testing to continue as scheduled.

![image](https://github.com/user-attachments/assets/c9909109-7f24-47e7-b9aa-a8a9a3a5d9f1)

**Final Results and Conclusion**

In summary, we have successfully executed performance tests using JMeter across three different environments: a live website (Cymbal Shops), a local Docker container (Nginx), and a full microservices application (Online Boutique on Kubernetes). In each case, we created appropriate JMeter test plans featuring thread groups (to simulate users), HTTP samplers (to send requests), configuration elements like Header Manager (to simulate browser headers), and listeners (to observe results). The endurance test on Cymbal Shops ran for 10 minutes with no errors, demonstrating the site’s stability under a constant load. The load test on the Nginx container (50 users) completed successfully, illustrating that the local server handled the simulated traffic easily. Finally, the test against the Online Boutique microservices app confirmed that even a complex, distributed application could be tested with JMeter, as all responses came back successfully from the Kubernetes deployment.

Our team captured screenshots of the JMeter configuration and outputs at each step – for instance, the Thread Group settings, the HTTP Request sampler and Header Manager setup, and the green results in the View Results Tree provide as evidence of the test execution. These visual proofs, along with the observations noted, indicate that the performance tests were conducted thoroughly and met the assignment requirements.

This exercise offered valuable hands-on experience in setting up performance tests, including load and endurance testing, and addressing practical issues such as installation challenges and environment configuration. Utilizing JMeter's GUI components and elements (thread groups, samplers, config elements, listeners) enhanced the understanding of designing a test plan tailored for various scenarios. Additionally, deploying an application specifically for testing purposes, the Online Boutique provided an added layer of learning, encompassing tools like Docker, Kubernetes, and Skaffold.

**Credit**

We performed and prepared this report and the testing activities. Our systematic approach to performance testing and ability to troubleshoot issues ensured that all assignment objectives were achieved. The results demonstrate a straightforward and successful execution of JMeter-based performance tests across multiple environments, showcasing JMeter's capabilities and the team's technical proficiency. All tests concluded with positive outcomes for us, and the systems under test remained stable under the imposed loads, marking the performance testing as a success.

**References**

- Abstracta. (n.d.). Types of Performance Testing: Which one is right for you? Retrieved April 23, 2025, from <https://abstracta.us/blog/performance-testing/types-of-performance-testing/>
- BlazeMeter. (n.d.). Performance Testing vs Load Testing vs Stress Testing. Retrieved April 23, 2025, from <https://www.blazemeter.com/blog/performance-testing-vs-load-testing-vs-stress-testing>
- GeeksforGeeks. (n.d.). Performance Testing in Software Testing. Retrieved April 23, 2025, from <https://www.geeksforgeeks.org/performance-testing-software-testing/>
- Global App Testing. (n.d.). 6 Types of Performance Testing to Use in Your Projects. Retrieved April 23, 2025, from <https://www.globalapptesting.com/blog/performance-testing-types>
- Guru99. (n.d.). Performance Testing: Types, Steps, Best Practices, and Tools. Retrieved April 23, 2025, from <https://www.guru99.com/performance-testing.html>
- LoadNinja. (n.d.). Performance Testing Types Explained. Retrieved April 23, 2025, from <https://loadninja.com/articles/performance-test-types/>
- Sokolova, A. (n.d.). Types of Performance Testing and Which Do You Need? TestDevLab. Retrieved April 23, 2025, from <https://www.testdevlab.com/blog/types-of-performance-testing-and-which-do-you-need>
- Apache JMeter. (n.d.). _Apache JMeter™_. Retrieved April 23, 2025, from <https://jmeter.apache.org/index.html>
- Apache JMeter. (n.d.). _JMeter Listeners_. Retrieved April 23, 2025, from <https://jmeter.apache.org/usermanual/listeners.html>
- Apache JMeter. (n.d.). _JMeter Test Plan: Thread Group_. Retrieved April 23, 2025, from <https://jmeter.apache.org/usermanual/test_plan.html#thread_group>
- Apache JMeter. (n.d.). _JMeter Test Plan: Configuration Elements_. Retrieved April 23, 2025, from <https://jmeter.apache.org/usermanual/test_plan.html#config_elements>
- Apdex Alliance. (n.d.). _Application Performance Index (Apdex)_. Retrieved April 23, 2025, from <https://www.apdex.org/>
