

# DevOps-Interview-Questions

Round 1 – Streaming at Scale, K8s, Cloud & Linux (45 mins)
1. How would you design auto-scaling for 50M+ concurrent viewers across multiple K8s clusters without over-provisioning?
2. During an IPL final, a new region needs to spin up instantly. How would you pre-warm nodes & scale workloads with zero cold-start impact?
3. Explain how you’d use Envoy + Istio to route low-latency live streams differently from VOD without service restarts.
4. What’s your approach to multi-zone pod affinity/anti-affinity to ensure a node failure doesn’t impact regional streaming SLAs?
5. How would you monitor HPA scaling decisions in real-time and detect if the metrics server is lagging?
6. Describe K8s readiness/liveness probe configs to catch buffering/lag issues in stream-processing microservices before users notice.
7. A kube-proxy update rolls out mid-match. What’s your network rollback plan to avoid packet drops?

Round 2 – RCA, Fire Drills & Streaming Chaos (75 mins)
1. Playback failures spike for only 3% of users in the APAC region. CPU, memory, and pods look fine. Could you walk through your triage plan?
2. Your Kafka ingestion pipeline lags by 2 minutes during a traffic surge. Producers are fine, consumers are idle. What’s your debug path?
3. Sudden tail latency on Redis-based stream session store during a Champions League match, how do you find & fix the bottleneck?
4. HPA refuses to scale in a critical podset even though Prometheus shows CPU > 90%. Root cause & fix?
5. NAT gateway costs double in 24 hours during a live series; no infra changes were made. What could be silently causing it?

Round 3 – Leadership, Reliability Culture & Scaling Influence (30 mins)
1. How do you build a culture where latency SLOs are enforced like uptime SLAs in a streaming org?
2. You’re asked to ship a multi-region failover for live events in 2 weeks with no DNS-based routing allowed. What’s your plan?
3. How would you simulate chaos in a streaming pipeline without risking real user impact?
4. How do you justify infra costs for pre-warmed scaling capacity to executives before a major sports event?


1. Core SRE Concepts
What is the primary goal of SRE?
Answer: SRE ensures scalable, reliable systems by applying software engineering principles to operations. Key goals include automating repetitive tasks (toil reduction), defining SLIs/SLOs, and balancing innovation with reliability using error budgets.
Explain the concept of an “error budget.”
Answer: An error budget quantifies the maximum acceptable downtime for a service. For example, a 99.9% uptime SLO allows 8.76 hours/year downtime. Teams use this budget to prioritize feature releases or reliability improvements.
What’s the difference between SLI, SLO, and SLA?
Answer:
SLI (Service Level Indicator): A measurable metric (e.g., request latency, error rate).
SLO (Service Level Objective): The target value for an SLI (e.g., 99.95% uptime).
SLA (Service Level Agreement): A contractual commitment with penalties if SLOs are violated.
4. How do SREs reduce “toil”?
Answer: Automating repetitive tasks (e.g., deployments, backups) using tools like Kubernetes or Ansible. For example, replacing manual server scaling with auto-scaling groups.

5. What is the role of blameless post-mortems?
Answer: To identify systemic issues (not individual blame) and implement preventive measures.

Example: If a deployment fails due to missing tests, the fix might involve improving CI/CD pipelines.

6. What are the “Four Golden Signals” of monitoring?
Answer:

Latency: Time to serve requests.
Traffic: Request volume (e.g., queries per second).
Errors: Rate of failed requests.
Saturation: Resource utilization (e.g., CPU, memory).
7. How do SREs handle capacity planning?
Answer: Analyzing historical data to predict resource needs (e.g., adding nodes to a Kubernetes cluster during peak traffic). Tools like Prometheus forecast usage trends.

8. Explain “Chaos Engineering.”
Answer: Proactively testing system resilience by simulating failures (e.g., shutting down nodes with Chaos Monkey).

9. SRE vs. DevOps: Key differences?
Answer:

DevOps focuses on cultural collaboration between dev and ops teams.
SRE applies engineering rigor to operations (e.g., SLOs, error budgets).
10. What is “MTTR” and “MTBF”?
Answer:

MTTR (Mean Time to Recover): Average time to resolve incidents.
MTBF (Mean Time Between Failures): Average time between system failures.
12. What is the role of SRE in bridging Development and Operations?
Answer: SRE merges software engineering with infrastructure management to ensure systems are reliable, scalable, and cost-effective. Key tasks include automating deployments and defining SLOs.

13. What are the key responsibilities of an SRE?
Answer:

Define and monitor SLIs/SLOs.
Automate toil (e.g., CI/CD pipelines).
Conduct blameless post-mortems.
Optimize cloud resource usage.
2. Linux/Unix
How do you troubleshoot high CPU usage?
Answer:
Use top or htop to identify resource-heavy processes.
Profile with strace or perf. Example: A Java app with high CPU might need garbage collection tuning.
What is the difference between kill -9 and kill -15?
Answer:
kill -15 (SIGTERM): Requests graceful shutdown.
kill -9 (SIGKILL): Forces immediate termination.
How to check open ports on a Linux server?
Answer: Use netstat -tuln or ss -tuln. Example: ss -tuln | grep 443 checks HTTPS usage.
Explain inode in Linux.
Answer: Stores file metadata (permissions, timestamps). Use df -i to check inode usage.
What is a zombie process?
Answer: A terminated process lingering in the process table. Fix by reaping its exit status via the parent process.
List common Linux signals.
Answer:
SIGHUP (1): Reload configurations.
SIGINT (2): Interrupt process (Ctrl+C).
SIGKILL (9): Force termination.
SIGTERM (15): Graceful shutdown.
How to find files modified in the last 7 days?
Answer:
bash
find /path -type f -mtime -7
What does lsof do?
Answer: Lists open files and their processes. Example: lsof /var/log/syslog identifies log access.
3. Networking
TCP vs. UDP: Key differences?
Answer:
TCP: Reliable, connection-oriented (e.g., HTTP).
UDP: Unreliable, connectionless (e.g., VoIP).
Describe the three-way handshake.
Answer:
Client sends SYN.
Server responds with SYN-ACK.
Client sends ACK.
What is BGP?
Answer: Border Gateway Protocol routes traffic between autonomous systems (e.g., internet backbone).
How does HTTPS work?
Answer: Encrypts HTTP traffic via TLS:
Server sends certificate.
Client verifies it.
Symmetric key exchange.
What is a CDN?
Answer: Content Delivery Network caches static assets globally to reduce latency (e.g., Cloudflare).
What is DHCP, and why is it used?
Answer: Dynamically assigns IP addresses to devices, reducing manual configuration errors.
SNAT vs. DNAT
Answer:
SNAT changes the source IP (e.g., private to public IP).
DNAT changes the destination IP (e.g., routing traffic to a backend server).
What is ICMP?
Answer: Internet Control Message Protocol (used by ping for connectivity checks).
4. Cloud Computing (AWS/GCP/Azure)
What is AWS Lambda?
Answer: Serverless compute service for event-driven code (e.g., processing S3 uploads).
Explain Azure Blob Storage.
Answer: Scalable object storage for unstructured data (images, logs).
What is GCP BigQuery?
Answer: Serverless data warehouse for SQL queries on large datasets.
How does AWS Auto Scaling work?
Answer: Automatically adjusts EC2 instances based on demand (e.g., CPU utilization).
What is Azure Availability Set?
Answer: Distributes VMs across fault domains to ensure redundancy during hardware failures.
What is IAM?
Answer: Identity and Access Management controls permissions for cloud resources.
5. Docker
Docker Image vs. Container
Answer:
Image: Template with app code and dependencies.
Container: Running instance of an image.
How to reduce Docker image size?
Answer: Use multi-stage builds and Alpine Linux base images.
What are Docker volumes?
Answer: Persistent storage for containers (e.g., databases).
How to secure Docker containers?
Answer:
Run containers as non-root users.
Scan images for vulnerabilities (e.g., Trivy).
Enable Docker Content Trust.
6. Kubernetes
What is a Pod?
Answer: The smallest deployable unit in Kubernetes, hosting one or more containers.
Deployment vs. StatefulSet
Answer:
Deployment: Manages stateless apps (e.g., web servers).
StatefulSet: Manages stateful apps (e.g., databases).
How to scale a Deployment?
Answer:
bash
kubectl scale deployment/myapp --replicas=5
What is a ConfigMap?
Answer: Stores non-sensitive configuration data (e.g., environment variables).
Explain Horizontal Pod Autoscaler (HPA).
Answer: Scales pods based on CPU/memory usage or custom metrics.
7. Monitoring & Logging
Prometheus vs. Grafana
Answer:
Prometheus: Collects and stores metrics.
Grafana: Visualizes metrics via dashboards.
What is an APM tool?
Answer: Application Performance Monitoring (e.g., New Relic) tracks latency, errors, and throughput.
ELK Stack Components
Answer:
Elasticsearch: Search/analytics engine.
Logstash: Data processing pipeline.
Kibana: Visualization tool.
What is Jaeger?
Answer: Distributed tracing tool for microservices (e.g., tracking request flows).
8. CI/CD
What is a canary deployment?
Answer: Roll out changes to a small user subset before full deployment to minimize risk.
GitLab CI vs. Jenkins
Answer:
GitLab CI: Integrated with GitLab.
Jenkins: Standalone, plugin-driven automation server.
9. Scripting & Automation
Write a script to count “ERROR” lines in a log file.
Answer:
bash
grep "ERROR" app.log | wc -l
How to parse JSON in Python?
Answer:
python
import json
data = json.loads(json_string)
10. Incident Management
Steps to resolve an outage?
Answer: Detect → Acknowledge → Diagnose → Fix → Post-mortem → Prevent recurrence.
What is a “playbook”?
Answer: Step-by-step guide for common incidents (e.g., database failure).
11. Security & Compliance
Principle of Least Privilege
Answer: Grant minimal permissions required for users/roles.
How to secure SSH?
Answer: Disable root login, use SSH keys, and enable 2FA.
12. Coding Challenges
Find top 5 IPs in a log file.
Answer:
bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -5
Check if a string is a palindrome (Python).
Answer:
python
def is_palindrome(s):
return s == s[::-1]


1. A critical production system is experiencing frequent outages. How would you identify root causes, improve reliability, and implement long-term fixes?


2. Deployment pipelines are slow and error-prone. How would you design and optimize CI/CD pipelines for speed, reliability, and security?


3. Infrastructure is not scalable and struggles under peak load. How would you design a scalable and resilient architecture?


4. Incidents are not detected early, causing business impact. How would you implement observability (logging, monitoring, tracing) across systems?


5. High operational toil is affecting team productivity. How would you automate repetitive tasks and improve efficiency?


6. Leadership requires high system availability and defined SLAs/SLOs. How would you design and enforce SRE practices and reliability engineering?


7. Multiple teams are deploying inconsistently across environments. How would you standardize infrastructure and deployments using tools like Terraform or Ansible?


8. Cloud costs are increasing without clear visibility. How would you optimize cloud usage and implement cost governance in platforms like Amazon Web Services or Microsoft Azure?


9. You are tasked with implementing containerization and orchestration. How would you design solutions using Docker and Kubernetes?


10. You are leading a platform engineering transformation initiative. How would you define strategy, build internal developer platforms, ensure security, and drive adoption across teams?

1. How Does Kubernetes Work Internally? What Happens When You Apply a Manifest?

Act 1: Client Request
deployment.yaml Creation:
A user defines the desired state in a YAML manifest. This includes the deployment name, replica count, and the pod specification (e.g., container image, ports).
kubectl Execution:
The kubectl apply -f deployment.yaml command is executed.
kubectl parses the YAML, converts it to a JSON payload, and makes a REST API call to the kube-apiserver.


Act 2: Control Plane - Initial Processing
kube-apiserver Receives Request:
Authentication: Verifies the identity of the client (e.g., via a user's kubeconfig or a service account token).
Authorization: Checks if the authenticated client is permitted to perform the requested action (e.g., create a Deployment) on the specified resource, typically using Role-Based Access Control (RBAC).
Admission Control: Executes a sequence of validation and mutation checks. This ensures the object conforms to cluster policies before it is persisted.
Persistence to etcd:
After the request is fully validated, the kube-apiserver writes the Deployment object to the etcd key-value store.
This write action solidifies the desired state as the cluster's new source of truth. etcd then notifies subscribers (like the API server itself) of the change.


Act 3: Control Plane - Reconciliation Loops
Deployment Controller (in kube-controller-manager):
Watch: This controller constantly watches the API Server for changes to Deployment objects.
Goal: To align the actual state with the desired state defined in the Deployment object, primarily by managing ReplicaSet versions.
Action:
It evaluates the Deployment and determines a ReplicaSet needs to be created or updated.
It generates a new ReplicaSet object with a name derived from the Deployment and a Pod template copied from the Deployment's specification.
It sends this ReplicaSet object to the kube-apiserver to be stored in etcd.

ReplicaSet Controller (in kube-controller-manager):
Watch: This controller watches for changes to ReplicaSet objects.
Goal: To ensure that the number of Pods matching its selector always equals the .spec.replicas count.
Action:
It compares the desired replica count from its spec (e.g., 3) against the number of currently active Pods it manages (currently 0).
Based on the difference, it creates the required number of Pod objects from its Pod template.
It sends these Pod objects to the kube-apiserver. The Pods are stored in etcd in the Pending phase because the .spec.nodeName field is not yet set.


Act 4: Control Plane - Scheduling
kube-scheduler:
Watch: The scheduler's sole focus is watching for Pod objects that have an empty .spec.nodeName field.
Goal: To assign each un-scheduled Pod to the most suitable worker Node.
Action (for each Pod):
Filtering: It identifies a list of viable Nodes that can run the Pod. This involves checking for constraints like resource requirements (CPU, memory), node selectors, taints/tolerations, and volume compatibility.
Scoring: It ranks the viable Nodes by applying a set of priority functions. These functions score nodes based on factors like resource utilization or trying to spread Pods across failure domains.
Binding: It commits to the highest-scoring Node and notifies the kube-apiserver. This action updates the Pod object in etcd, setting the .spec.nodeName to the chosen Node.


Act 5: Node-Level Execution
Kubelet (on the assigned Node):
Watch: Each Kubelet agent watches the API Server for Pods that have been scheduled to its node.
Goal: To ensure the containers described in a Pod's specification are running and healthy on its Node.
Action:
It detects the new Pod assigned to it.
It interacts with the Container Runtime (via the Container Runtime Interface - CRI) to perform the following:
Image Pull: Instructs the runtime to pull the container image (nginx:latest) if it's not cached locally.
Container Creation: Instructs the runtime to create and run the container based on the image and Pod spec.
Resource Setup: Configures the Pod's sandbox environment, including setting up networking (via CNI plugin) and mounting any specified data volumes.

Status Reporting:
The Kubelet continuously monitors the status of the Pod's containers.
It reports the Pod's status, events, and IP address back to the kube-apiserver, which updates the Pod's .status field in etcd. This makes the state visible to the rest of the cluster.
The Key Insight: This entire process, from controllers to the scheduler to the kubelet, is a series of reconciliation loops. Each component continuously watches the "desired state" in etcd and works independently to make the "actual state" of the world match it.

2. How Does DNS Resolution Work in Kubernetes?

When a Pod tries to resolve a service name:

- The Pod's /etc/resolv.conf is configured by kubelet to point to CoreDNS (typically at 10.96.0.10 or similar cluster IP).
- CoreDNS receives the DNS query and checks if it matches the cluster domain pattern (.svc.cluster.local).
- For cluster services, CoreDNS queries the Kubernetes API to find the Service object and returns its ClusterIP.
- For external domains, CoreDNS forwards the query to upstream DNS servers (defined in the CoreDNS ConfigMap).
- The Pod receives the IP and establishes a connection. If it's a Service ClusterIP, kube-proxy (or iptables/IPVS rules) handles the load balancing to backend Pods.

Common Pitfalls:

Using wrong namespace in service name
CoreDNS ConfigMap misconfiguration for external DNS
Network policies blocking DNS traffic on port 53

3. Taints/Tolerations vs Node Affinity: When to Use What?

Taints/Tolerations:

Purpose: Repel Pods from nodes unless they explicitly tolerate the taint
Direction: Node-centric (nodes push away Pods)
Use Cases:
- Dedicated nodes for specific workloads (e.g., GPU nodes)
- Keeping nodes reserved during maintenance
- Isolating problematic or experimental workloads



Node Affinity:

Purpose: Attract Pods to specific nodes based on labels
Direction: Pod-centric (Pods pull toward nodes)
Use Cases:
- Placing Pods on nodes with specific hardware (SSD, high-memory)
- Multi-tenant clusters with customer-specific node pools
- Co-locating related Pods for performance

4. How Does AWS VPC CNI Work?

The AWS VPC CNI plugin assigns actual VPC IP addresses to Pods:

ENI Attachment: When a node starts, the CNI plugin attaches multiple Elastic Network Interfaces (ENIs) to the EC2 instance.
IP Pre-allocation: The plugin pre-allocates secondary IP addresses from your VPC subnet to these ENIs.
Pod Assignment: When a Pod is scheduled, the CNI assigns one of these pre-allocated IPs directly to the Pod.
Routing: The Pod's traffic flows through the node's ENI, making the Pod routable within the VPC without NAT.

5. How Does CoreDNS Actually Work?

CoreDNS is a plugin-based DNS server deployed as a Deployment in the kube-system namespace:

Listening: CoreDNS Pods listen on port 53 (UDP/TCP) via a Service ClusterIP.
Plugin Chain: Each request goes through a plugin chain defined in the CoreDNS ConfigMap:

- errors: Logs errors
- health: Provides health endpoints
- kubernetes: Handles cluster-internal DNS queries by watching the K8s API for Service and Pod objects
- forward: Forwards external queries to upstream DNS (e.g., 8.8.8.8)
- cache: Caches responses to reduce API load


Service Discovery in CoreDNS system: 
- For internal queries, the kubernetes plugin:
1. Parses the FQDN (e.g., nginx.default.svc.cluster.local)
2. Queries the API server for the Service in the specified namespace
3. Returns the ClusterIP as an A record

6. Persistent Volume (PV) Errors and Troubleshooting

   The PV/PVC binding process involves several steps:

PVC Creation: A PersistentVolumeClaim defines the storage requirement (size, access mode, storage class).
Dynamic Provisioning: If a StorageClass is specified with a provisioner (e.g., EBS CSI driver), it dynamically creates a PV.
Binding: The PVC binds to a PV that matches its requirements.
Mounting: The kubelet mounts the volume into the Pod's filesystem.

Common Issues:

No matching PV: Check if StorageClass exists and provisioner is working (kubectl get sc)
Access mode mismatch: ReadWriteOnce vs ReadWriteMany conflicts
Quota exceeded: AWS EBS volume limits or storage quotas
Node affinity: PV is in a different zone than the node (check topology constraints)
CSI driver issues: Check CSI controller and node driver pods in kube-system

7. How Does ECR Image Pulling Work from EKS Pods?

   The authentication flow for ECR in EKS:

1. Pod Specification: The Pod spec references an ECR image (e.g., 123456789.dkr.ecr.us-east-1.amazonaws.com/my-app:v1).
2. Kubelet Initiates Pull: The kubelet on the node tells the container runtime to pull the image.
3. IAM Role Authentication:
   - If the node has an IAM instance profile, it uses those credentials
   - If IRSA (IAM Roles for Service Accounts) is configured, the Pod's service account provides credentials
4. ECR Token: The container runtime (or kubelet credential helper) calls ECR's GetAuthorizationToken API to get a temporary Docker login token.
5. Image Pull: Using the token, the runtime authenticates to ECR and pulls the image layers.
6. Caching: The image is cached locally on the node for future use.

Common Issues:
1. Missing IAM permissions: The node role or IRSA role lacks ecr:GetAuthorizationToken, ecr:BatchGetImage, ecr:GetDownloadUrlForLayer
2. Wrong region: Image is in a different region than specified in the URL
3. Private ECR in different account: Need cross-account IAM permissions
4. Image doesn't exist: Typo in image tag or repository name

   


1️⃣ You mentioned you’ve worked with Argo CD – how do you use it?

2️⃣ There’s a scenario where someone wants to deploy infrastructure on AWS via Terraform. Which tool will you use and why?

3️⃣ How do you maintain Argo CD and use it to deploy on Kubernetes?

4️⃣ How do you manage rollbacks of an application in Argo CD?

5️⃣ In Kubernetes, how many master and worker nodes you had configure?

6️⃣ Would you use an entirely different AWS account, a separate cluster, or just a namespace to differentiate Dev and Prod?

7️⃣ If you have 1 master node and 1 worker node running an application, how would you upgrade Kubernetes on both?

8️⃣ Do you store your Terraform state file locally or remotely? Why?

9️⃣ If you deployed 12 EC2 instances and 2 were deleted manually, what happens when you run terraform apply again?

🔟 How do you update your Terraform state file to match current AWS resources? Which command will you use?

1️⃣1️⃣ How would you deploy to multiple AWS accounts like Dev, UAT, and Prod?

1️⃣2️⃣ Can we do it with Terraform workspaces?

1️⃣3️⃣ What does terraform init do?

1️⃣4️⃣ If an EC2 instance has an IAM role to access S3 but gets “Permission Denied,” how do you troubleshoot?

1️⃣5️⃣ You use ALB for EKS external traffic – how do you manage traffic inside EKS to different paths?

1️⃣6️⃣ For cost optimization, how do AWS Savings Plans work?

1️⃣7️⃣ You use rolling updates for deployments, but what if you want to use a Blue-Green approach with Argo CD inside Kubernetes?

Here’s what I ask 👇
1. How does DNS resolution work inside a pod?
→ And what’s the first thing you check when a service isn’t reachable by name?

2. Please walk me through the controller manager’s role during a Deployment.
→ Not just kubectl rollout status — I want the reconciliation logic.

3. What happens if a node with local storage gets autoscaled down?
→ Hint: this is where most real prod data loss stories begin.

4. Post-deploy, latency spikes for 30% of users. No errors. No logs. No alerts.
→ What’s your 3-step triage?

5. How do you enforce runtime security in Kubernetes?
→ PSP? AppArmor? OPA? Or… just crossing fingers?

6. HPA vs VPA vs Karpenter — when would you NOT use each?
→ Bonus: How do you simulate HPA behavior in staging?

7. Tell me about the last Kubernetes outage you debugged.
→ No postmortem? You probably weren’t in the war room.

Round 1 – Systems at Scale, K8s, Cloud & Linux (45 mins)
• How would you implement fine-grained service discovery across 1000+ microservices using Envoy or Istio?
• Explain how you’d leverage eBPF + Cilium to enforce network security policies at runtime, and what the advantages are over traditional CNIs?
• Netflix runs multi-cloud. Describe your approach to cross-cloud routing, IAM, and secret syncing.
• What happens when systemd units fail intermittently on EKS nodes? How do you detect and heal?
• Your app teams demand custom AMIs. What’s your pre-prod vetting strategy at kernel and runtime?
• Walk through advanced kube-probe configurations to detect business logic failures, not just HTTP 200.
• How do you handle DNS-level outages inside a service mesh without a full app redeploy?
• Terraform remote_state backend suddenly times out. What’s your recovery and damage containment strategy?

Round 2 – RCA, Fire Drills, and Netflix-Scale Chaos (75 mins)
• A new Envoy config rollout silently broke mTLS between edge and mesh. What’s your RCA trace?
• HPA refuses to scale even though Prometheus shows CPU > 80%. Diagnose with cloud + K8s metrics.
• You introduced a sidecar-based caching layer. Suddenly, tail latency spikes. What’s your debug path?
• 504 errors on the playback service. Cloud LB shows healthy, mesh sidecars pass, but users can’t stream. Triage?
• You notice surging NAT costs. No infra changes this week. What could silently trigger that?

Round 3 – Leadership, Chaos Culture & Engineering Influence (30 mins)
• How do you build an engineering culture where SLOs are owned, not ignored?
• You’re asked to ship a multi-region failover in 3 weeks no DNS layer allowed. Your plan?
• Describe how you’d test infra chaos and graceful degradation for a Netflix Originals release.
• How do you prove ROI of infra modernization to non-technical execs?

🔐 How do you design a DevSecOps pipeline for regulated industries like banking or insurance?
📂 What’s your approach to auditing IAM policies and access controls across AWS or Azure in an enterprise setup?
⚙️ Explain how you’d manage infrastructure provisioning across multiple environments using Terraform.
🚀 How do you ensure rollback safety and transaction integrity in multi-step deployments?
🔍 How do you approach drift detection and reconciliation in GitOps workflows?
🛡️ What steps do you take to implement network policies and pod security in Kubernetes clusters?
📈 How would you monitor and alert for critical services in a payment gateway system using Prometheus/Grafana or Splunk?
🧩 Have you worked on integrating CI/CD with security scanning tools like Snyk, Aqua, or Trivy? Explain the integration flow.
🔄 How do you automate patching and OS hardening in cloud-based systems at scale?
🌐 What’s your experience with designing HA (high availability) and DR (disaster recovery) architecture for enterprise apps?
🧠 How would you implement secrets rotation for database credentials across Kubernetes and CI/CD pipelines?
📦 Explain the difference between Helm and Kustomize. When would you use one over the other?
🔄 Can you walk us through how you set up Blue-Green or Canary deployments in Azure DevOps or ArgoCD?
⚖️ How do you handle cost optimization in cloud-native infrastructure, especially in a FinTech context?
🧩 What are the compliance checks you automate before production deployments (e.g., CIS benchmarks, encryption validation)?
📋 Describe how you would manage and track configuration drifts in a large multi-account AWS environment.
🛠️ How do you automate SSL/TLS certificate renewal in Kubernetes ingress controllers?
🔐 How do you implement Zero Trust principles in internal tooling and CI/CD systems?
📊 Have you implemented centralized logging and monitoring for audit trails and anomaly detection? Which stack did you use?
🧪 How would you use tools like LitmusChaos or Gremlin for testing system resilience in production-like environments?

1. Scalability – Vertical vs horizontal; default to stateless horizontal scale
2. Latency & Throughput – Know P50/P95/P99 and tradeoffs
3. Capacity Estimation – Estimate QPS, storage, bandwidth
4. Networking Basics – TCP, HTTP/HTTPS, TLS, UDP, DNS
5. SQL vs NoSQL – Decide by access patterns, joins, consistency
6. Data Modeling – Structure to optimize hot queries
7. Indexing – Right indexes help reads, wrong ones hurt writes
8. Normalization – Normalize for writes, denormalize for reads
9. Caching – App/DB/CDN; write-through/back, TTLs
10. Cache Invalidation – TTLs, versioning, stampede protection

11. Load Balancing – L4/L7, RR, least-connections, hashing
12. CDN & Edge – Serve static content near users
13. Sharding – Hash/range/geo; handle hot keys, rebalancing
14. Replication – Sync/async, leader/follower, read replicas
15. Consistency Models – Strong, eventual, causal
16. CAP Theorem – In partition, pick A or C
17. Concurrency – Locks, MVCC, retries
18. Multithreading – Pools, contention, switching
19. Idempotency – Same request, same effect
20. Rate Limiting – Fairness and protection

21. Queues & Streams – Kafka, RabbitMQ; smooth spikes
22. Backpressure – Manage slow consumers/load shedding
23. Delivery Semantics – At-most/least/exactly-once
24. API Design – REST vs gRPC; human vs machine optimized
25. API Versioning – Additive only; never break clients
26. AuthN & AuthZ – OAuth2, JWT, RBAC/ABAC
27. Resilience – Circuit breakers, timeouts, retries
28. Observability – Logs, metrics, traces, SLI/SLO
29. Health Checks – Detect failures, auto-replace
30. Redundancy – Avoid SPOFs; use multi-AZ/region

31. Service Discovery – Dynamic endpoints, central config
32. Deployment – Canary, blue/green, rollbacks
33. Monolith vs Microservices – Split only if ops cost justified
34. Distributed Transactions – Use sagas, avoid 2PC
35. Event Sourcing/CQRS – Append-only logs, split models
36. Consensus – Raft, Paxos, quorum reads/writes
37. Data Privacy – Encryption, GDPR/CCPA
38. Disaster Recovery – Backups, RPO/RTO, test it
39. Serialization – JSON, Avro, Proto
40. Real-Time Delivery – Polling, SSE, WebSockets

● Tell me about yourself (Self-Intro)
● What source code management tool are you using?
● What CI/CD tools do you use?
● How do you troubleshoot issues in CI/CD pipelines?
● What kind of pipelines have you worked on?
● What is a Jenkinsfile?
● What are your daily DevOps activities?
● Explain Git merge and branching strategies.
● Describe a full CI/CD flow in your project.
● How do you build a Docker image? Explain the steps.
● How do you check CPU usage of a server?
● What operating systems have you worked with?
● What is Ansible? Why did you use it in your project?
● What is Terraform used for?
● What's the difference between Ansible and Terraform?
● Have you worked with Kubernetes? Explain your use case.
● What AWS services are you familiar with?
● Have you used monitoring tools? How and why are they used?
● What do you do if there's a Git issue while merging or pushing?
● What kind of Jenkins plugins have you used?
● How do you trigger an Ansible playbook? What’s the command?
● How to run a specific task in Ansible?
● How do you manage Jira tickets or handle tasks?

1. You’ve got an app running in one AWS account that needs to access an S3 bucket in another account. How would you set that up securely?
2. Can you write a Dockerfile for a Node.js app using multi-stage builds — just something you’d actually use in a real project?
3. Suppose your Terraform state file gets corrupted or out of sync. What steps would you take to recover from that?
4. You have an EC2 in a private subnet and can’t use a NAT Gateway. How else can it reach the internet for updates or downloads?
5. A container has crashed or exited suddenly — how would you go about figuring out what went wrong?
6. There’s an existing AWS VPC created manually, but now you want to manage it through Terraform. How would you import it and make sure nothing breaks?
7. How would you roll out blue-green deployments in a Kubernetes cluster? What would that look like in practice?
8. When using Terraform, how do you manage sensitive values like secrets or API keys without hardcoding them?
9. In a Dockerfile, what’s the practical difference between using COPY and ADD? When would you prefer one over the other?
10. If you had to create resources across two different AWS accounts using Terraform, how would you set that up?
11. Imagine you have a PHP app in a Docker container that needs MySQL credentials — how would you pass those securely?
12. If someone manually changed an S3 bucket policy that was originally created with Terraform, how would you deal with that kind of drift?
13. How do you enforce rules in Kubernetes to control which pods can talk to each other?
14. Could you write a small Python script that backs up all files older than 30 days from a folder?
15. If your team is seeing a spike in cloud costs, how would you go about figuring out why — and cutting cost without hurting performance?
16. Say you want to serve users in different countries using AWS. How would you route traffic based on user location?
17. You’re on-call. A production Kubernetes cluster is a mess — pods aren’t pulling images, some are getting evicted, and users are seeing errors. How do you troubleshoot this, and how would you prevent it next time?

𝐑𝐨𝐮𝐧𝐝 𝟑: 𝐁𝐞𝐡𝐚𝐯𝐢𝐨𝐫𝐚𝐥 𝐑𝐨𝐮𝐧𝐝
1. What would you do if you were asked to work on a tool or technology you’ve never used before?
2. Can you share a time when you had to deliver something quickly, but didn’t have all the time or resources you needed? How did you manage it?
3. Tell me about a mistake or outage you were involved in — how did you respond, and what did you learn from it?
4. What’s the hardest technical issue you’ve faced so far? How did you go about solving it?
5. If you wanted to convince your team or manager to adopt a new tool or process, how would you make your case?

Round 1: Tech Screening (45 mins)
• Explain your entire CI/CD, where’s the blast radius?
• Difference between ALB and NLB, when does latency matter more than routing logic?
• Terraform taint vs terraform state rm, when would you use either?
• How does CoreDNS handle plugin chain resolution?
• Choose between EBS, EFS, and FSx for different workloads
• Bash: Find all .log files over 500MB modified in the last 48h
• DB migration without downtime? Explain rollback contingency
• Roll back a failed Helm release. What’s your exact command chain?
• Secure Vault secrets in GitHub Actions without hardcoding
• Node is Ready, but kubelet logs are silent. What now?

Round 2: Live Scenarios & RCA (75 mins)
• Your autoscaler doubled the infrastructure costs. Logs are normal. What do you check?
• A prod pod restarts 8 times/hour. Logs look fine. What’s your triage plan?
• Terraform drift detected. Who gets paged, and what’s the fix sequence?
• Disaster recovery setup, what does it look like? When was it last tested?
• Simulate a DNS outage with the CoreDNS plugin misorder. how?
• Dockerfile: Multi-stage image with caching and minimized attack surface
• Secure gRPC traffic across K8s namespaces
• S3 bucket deleted by mistake, walk me through infra + IAM prevention
• Helm worked in staging but broke in prod. Explain the failure class.
• App returns 200 OK, but functionality is broken. What did you monitor?

Round 3: Behavioral Grit Check (30 mins)
• How do you onboard juniors without overwhelming them?
• What’s the worst production day you’ve faced? What broke you?
• Ever shipped under pressure and regretted it?
• How do you protect engineering from non-technical deadlines?
• What does a “no blame” postmortem look like in your team?

💡 TL;DR:
If you haven’t:
1. Simulated a DNS cache corruption
2. Recovered a Terraform state loss
3. Diagnosed a 200/OK but broken service
4. Debugged CoreDNS plugin chain at midnight

1. How do you design a secure and scalable CI/CD pipeline in Jenkins or Azure DevOps?

2. Explain the difference between build and release pipelines.

3. How do you handle rollback strategies in CI/CD?

4. What are Jenkins Shared Libraries? Why and how do you use them?

5. How would you implement multi-branch pipelines for microservices?

6. How do you handle pipeline-level caching to speed up builds?

7. How do you inject secrets into your pipeline safely?

8. What are manual approval gates, and where do you implement them?

9. Explain pipeline-as-code. How do you implement it in Azure DevOps YAML format?

10. How do you enforce compliance and quality checks in a CI/CD pipeline?

11. What is the Terraform state file? How do you manage it securely in teams?

12. Difference between terraform refresh, plan, and apply.

13. Explain how you modularize Terraform code for reuse.

14. What happens if you manually change infrastructure outside Terraform? How do you detect drift?

15. Have you used Terraform with Azure or AWS? How do you set up backend state in Azure Blob or S3?

16. How do you implement count, for_each, and dynamic blocks in Terraform?

17. What is the lifecycle block in Terraform used for?

18. Write a Dockerfile for a Node.js or Python application.

19. Explain how multi-stage Docker builds help with image optimization.

20. What’s the difference between a Docker volume and bind mount?

21. What’s the role of kubelet, kube-proxy, and kube-apiserver?

22. How do you perform a blue-green deployment in Kubernetes?

23. What is a StatefulSet vs Deployment in K8s?

24. What are readiness and liveness probes and how do they impact scaling?

25. How do you manage secrets in Kubernetes?

26. What is the difference between a private and service endpoint in Azure?

27. How do you manage access between services using IAM roles or
Managed Identity?

28. What’s the difference between Azure VNet and AWS VPC?

29. How do you implement VNet peering or inter-region connectivity securely?

30. How do you handle high availability and disaster recovery in cloud deployments?

31. What logging and monitoring solutions have you implemented in Azure or AWS?

32. How do you enforce security baselines in your CI/CD pipelines?

33. Have you used tools like Trivy, Aqua, or Snyk for container scanning?

34. How do you integrate Azure Key Vault (or AWS Secrets Manager) in your pipeline securely?

35. How do you rotate secrets without downtime?

36. How do you set up RBAC in Jenkins, Azure DevOps, or Kubernetes?

37. You deployed to production and a service is down. How do you debug and rollback safely!

38. A junior DevOps engineer deleted a critical Terraform state file — how do you recover?

39. A Docker container works locally but fails in Jenkins — how would you troubleshoot it?

40. One pod in Kubernetes is restarting in a CrashLoopBackOff state — how do you fix it?

𝗥𝗼𝘂𝗻𝗱 𝟭: 𝗧𝗲𝗰𝗵𝗻𝗶𝗰𝗮𝗹 𝗦𝗰𝗿𝗲𝗲𝗻𝗶𝗻𝗴

1. Explain the CI/CD workflow you follow and the kind of pipeline you use. How do you define and invoke pipelines in Jenkins?
2. What are shared libraries in Jenkins, and how are they written and defined?
3. What kind of applications do you deploy using Jenkins pipelines, and what deployment tools do you use?
4. If the Jenkins pipeline runs but the build doesn’t happen, what possible issues could be causing it?
5. What is the purpose of a webhook, and how is it used in a CI/CD pipeline?
6. How do you create and manage Kubernetes clusters (using tools like Terraform), and what are the master and worker nodes?
7. What are common Kubernetes errors you’ve faced (like CrashLoopBackOff, ImagePullError), and how did you resolve them?
8. What is the command to access a pod and how can you define or create a Kubernetes class or object?
9. Explain the folder structure of a basic Helm chart. What commands do you use to deploy with Helm?
10. What are the stages in a Docker image build? Why do we use ENTRYPOINT and CMD instructions?
11. How do you manage and connect services like DBs, EC2, EKS, or ECS? Include the command to connect to ECS.
12. Which container registry do you use for storing Docker images?

𝗥𝗼𝘂𝗻𝗱 𝟮: 𝗜𝗻-𝗱𝗲𝗽𝘁𝗵 𝗧𝗲𝗰𝗵𝗻𝗶𝗰𝗮𝗹 𝗦𝗰𝗿𝗲𝗲𝗻𝗶𝗻g

1. What branching strategy do you follow, and how do you handle merges to avoid breaking the release branch? If a bug appears in production, what’s your approach to resolving it?
2. Describe your typical deployment flow and CI/CD workflow. What stages do you define in your Jenkins pipeline, and how do you ensure full quality checks during deployment?
3. How do you use Jenkins shared libraries? Explain their typical structure and how they are integrated into your Jenkinsfiles.
4. Are you aware of security scanning tools? How do you scan Docker images—both during build and at the registry level? Are you using any extensions or tools for image scanning?
5. How do you pass environment variables during Docker build commands? What services do you use for storing Docker images?
6. How do you establish a connection with databases in your deployments or infrastructure setup?
7. How do you handle authentication for EKS clusters and store secrets securely in your environment?
8. How do you create AWS Lambda functions and manage the artifacts for deployment? What options do you use to push artifacts to Lambda?
9. What is email signing and Helm chart signing? Which tools do you use to sign Helm charts?

1) Explain the control plane components and their functionalities, and how they interact with each other.
2) Explain how the kube-proxy component works on the node.
3) Explain the CNI and CRI interfaces.
4) How to troubleshoot etcd issues.


📍Intermediate Level [Kubernetes ]
5) Explain the core components and benefits of a service mesh like Istio or Linkerd.
6) Describe how traffic management (e.g., canary deployments, A/B testing) is implemented using a service mesh.
7) Discuss the security implications and best practices for implementing mutual TLS (mTLS) in a service mesh.
8) How do you troubleshoot a performance issue originating from the service mesh?
9) Explain the differences between ClusterIP, NodePort, LoadBalancer, and Ingress services.
10) Describe how you would implement a multi-cluster ingress solution.
11) Discuss the challenges of implementing network policies and how you would troubleshoot network connectivity issues within a Kubernetes cluster.
12) Explain how to implement and troubleshoot egress network policies.
13) Explain the differences between PersistentVolumes (PVs), PersistentVolumeClaims (PVCs), and StorageClasses.

📍Advanced Level [ Kubernetes ]
14) Describe how you would implement dynamic provisioning of persistent storage for stateful applications.
15) Discuss the challenges of managing stateful applications in Kubernetes and how you would ensure data consistency and availability.
16) How to take and restore a snapshot of a persistent volume?
Security Best Practices:
17) Describe how you would implement role-based access control (RBAC) to restrict access to Kubernetes resources.
18) Discuss the importance of container image security and how you would implement image scanning and vulnerability management.
19) Explain how you would implement secrets management using Kubernetes Secrets or an external secrets management solution (e.g., HashiCorp Vault).
20) How would you implement and enforce pod security policies, and how have pod security standards changed the landscape?
21) Describe how you would implement a comprehensive monitoring and logging solution for a Kubernetes cluster.
22) Discuss the importance of distributed tracing and how you would implement it using tools like Jaeger or Zipkin. 

1. What happens when your etcd store goes stale or corrupted?
→ Talk backup strategies, RAFT consistency, and disaster recovery. This isn’t your bootcamp final project.

🔥 2. How would you debug a Pod that’s intermittently unreachable across nodes but passes health checks?
→ Show me your CNI layer chops. Calico? Cilium? Routing tables? Let’s go.

🔥 3. What’s your strategy for zero-downtime upgrades of Ingress Controllers at scale?
→ Hint: Rolling updates + traffic shifting + TLS + chaos = test of your soul.

🔥 4. How do you enforce multi-tenant isolation in a shared cluster?
→ Namespaces are just the start. Let’s talk about RBAC boundaries, NetworkPolicies, and resource quotas that don’t break prod.

🔥 5. What’s your incident response when Karpenter overscales during a sudden traffic spike and cost goes 3x?
→ HPA saves your app. But cost visibility saves your job.

🔥 6. Walk me through how you’d rotate secrets across hundreds of microservices without downtime.
→ Let’s see your secret manager + CSI driver + rollout design. Don’t say “we just restart the pods.”

🔥 7. You just recovered from an outage. Now what?
→ Real engineers talk SLO breaches, RCAs, postmortems, chaos testing. Not “it’s working now, all good.”

💭 Still think Kubernetes is “just a container orchestrator”?
You don’t need more tutorials.
You need fire drills.

👉 1. What is the status of a Pod with initContainers that fail if the main container has a restartPolicy of Never?

👉 2. In a StatefulSet with three replicas, if you remove replica-1, will replica-2 and replica-3 be renamed to keep their sequential order?

👉 3. Can a DaemonSet Pod be placed on a master node that has a NoSchedule taint without adding tolerations?

👉 4. When you update a Deployment's image during an ongoing rolling update, does K8s wait for the current rollout to finish or initiate a new one right away?

👉 5. How long does it take for Pods to be evicted when a node becomes NotReady, and can this eviction time be customized for each Pod?

👉 6. Is it possible for multiple containers within a Pod to share the same port on localhost, and what occurs if they attempt to bind at the same time?

👉 7. If you create a PVC with ReadWriteOnce access mode, can several Pods on the same node access it at the same time?

👉 8. What happens to the Horizontal Pod Autoscaler with custom metrics if the metrics server goes down during peak load?

👉 9. Is it possible to use kubectl port-forward to a Pod in a CrashLoopBackOff state, and will it function?

👉 10. What occurs with the mounted tokens and API access if a ServiceAccount is deleted while its associated Pods are still running?

👉 11. When implementing anti-affinity rules, can a situation arise where no new Pods are able to be scheduled?

👉 12. If you have a Job set to parallelism: 3 and one Pod fails with a restartPolicy of Never, will the Job spawn a replacement Pod?

👉 13. Can you alter a Pod's resource requests after it has been created, and what is the distinction between requests and limits in OOM situations?

👉 14. In network policies, if egress rules aren't specified, are outbound connections automatically denied?

👉 15. If a Persistent Volume becomes corrupted, can multiple PVCs linked to it lead to failures in different namespaces?

Write a Terraform script that will provision, set up, and configure a static website1

. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗽𝗼𝗱𝘀 --𝗮𝗹𝗹-𝗻𝗮𝗺𝗲𝘀𝗽𝗮𝗰𝗲𝘀: Check the status of all pods across namespaces to identify failures.
2. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗱𝗲𝘀𝗰𝗿𝗶𝗯𝗲 𝗽𝗼𝗱 𝗽𝗼𝗱_𝗻𝗮𝗺𝗲: Gather detailed information about a failed pod.
3. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗹𝗼𝗴𝘀 𝗽𝗼𝗱_𝗻𝗮𝗺𝗲 -𝗰 𝗰𝗼𝗻𝘁𝗮𝗶𝗻𝗲𝗿_𝗻𝗮𝗺𝗲: View logs of a specific container inside a pod to troubleshoot issues.
4. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗲𝘃𝗲𝗻𝘁𝘀 --𝗮𝗹𝗹-𝗻𝗮𝗺𝗲𝘀𝗽𝗮𝗰𝗲𝘀 --𝘀𝗼𝗿𝘁-𝗯𝘆='.𝗺𝗲𝘁𝗮𝗱𝗮𝘁𝗮.𝗰𝗿𝗲𝗮𝘁𝗶𝗼𝗻𝗧𝗶𝗺𝗲𝘀𝘁𝗮𝗺𝗽': Review recent events for clues on crashes and errors.
5. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗻𝗼𝗱𝗲𝘀: Verify the status of nodes in the cluster, checking for node failures.
6. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗱𝗿𝗮𝗶𝗻 𝗻𝗼𝗱𝗲_𝗻𝗮𝗺𝗲 --𝗶𝗴𝗻𝗼𝗿𝗲-𝗱𝗮𝗲𝗺𝗼𝗻𝘀𝗲𝘁𝘀: Safely evacuate and cordon a node for recovery operations.
7. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗰𝗼𝗿𝗱𝗼𝗻 𝗻𝗼𝗱𝗲_𝗻𝗮𝗺𝗲: Mark a node as unschedulable to prevent new pods from being scheduled during recovery.
8. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗱𝗲𝗹𝗲𝘁𝗲 𝗽𝗼𝗱 𝗽𝗼𝗱_𝗻𝗮𝗺𝗲 --𝗴𝗿𝗮𝗰𝗲-𝗽𝗲𝗿𝗶𝗼𝗱=0 --𝗳𝗼𝗿𝗰𝗲: Forcefully delete a crashed pod to restart it or clear it for recovery.
9. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗿𝗼𝗹𝗹𝗼𝘂𝘁 𝘂𝗻𝗱𝗼 𝗱𝗲𝗽𝗹𝗼𝘆𝗺𝗲𝗻𝘁 𝗱𝗲𝗽𝗹𝗼𝘆𝗺𝗲𝗻𝘁_𝗻𝗮𝗺𝗲: Roll back a deployment in case a new rollout causes crashes.
10. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗲𝘅𝗲𝗰 -𝗶𝘁 𝗽𝗼𝗱_𝗻𝗮𝗺𝗲 -- /𝗯𝗶𝗻/𝘀𝗵: Access a container to debug and resolve application issues directly inside the pod.
11. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗰𝗼𝗺𝗽𝗼𝗻𝗲𝗻𝘁𝘀𝘁𝗮𝘁𝘂𝘀𝗲𝘀: Check the health of core cluster components like etcd, kube-apiserver, and more.
12. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝘁𝗼𝗽 𝗻𝗼𝗱𝗲𝘀: Monitor node resource usage to detect resource exhaustion causing crashes.
13. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝘁𝗼𝗽 𝗽𝗼𝗱𝘀 --𝗮𝗹𝗹-𝗻𝗮𝗺𝗲𝘀𝗽𝗮𝗰𝗲𝘀: Check pod resource usage across namespaces, identifying bottlenecks leading to crashes.
14. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗱𝗲𝗹𝗲𝘁𝗲 𝗻𝗼𝗱𝗲 𝗻𝗼𝗱𝗲_𝗻𝗮𝗺𝗲: Remove a failed node from the cluster to allow recovery operations.
15. 𝗲𝘁𝗰𝗱𝗰𝘁𝗹 --𝗲𝗻𝗱𝗽𝗼𝗶𝗻𝘁𝘀=𝗵𝘁𝘁𝗽𝘀://𝗲𝘁𝗰𝗱-𝘀𝗲𝗿𝘃𝗲𝗿:2379 𝘀𝗻𝗮𝗽𝘀𝗵𝗼𝘁 𝗿𝗲𝘀𝘁𝗼𝗿𝗲 𝗯𝗮𝗰𝗸𝘂𝗽.𝗱𝗯: Restore etcd from a snapshot in case of etcd failure.
16. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗮𝗽𝗽𝗹𝘆 -𝗳 𝗯𝗮𝗰𝗸𝘂𝗽.𝘆𝗮𝗺𝗹: Reapply configurations from a backup manifest during recovery.
17. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝘁𝗮𝗶𝗻𝘁 𝗻𝗼𝗱𝗲𝘀 𝗻𝗼𝗱𝗲_𝗻𝗮𝗺𝗲 𝗸𝗲𝘆=𝘃𝗮𝗹𝘂𝗲:𝗡𝗼𝗦𝗰𝗵𝗲𝗱𝘂𝗹𝗲: Prevent scheduling on a node experiencing issues during recovery.
18. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗲𝗻𝗱𝗽𝗼𝗶𝗻𝘁𝘀 𝘀𝗲𝗿𝘃𝗶𝗰𝗲_𝗻𝗮𝗺𝗲: Verify service endpoints during recovery to ensure services are resolving correctly.

1. What scripting languages are you familiar with?
2. What are artifacts in GitLab CI?
3. What is a private module registry in Terraform?
4. If you delete the local Terraform state file and it's not stored in S3 or DynamoDB, how can you recover it?
5. How do you import resources into Terraform?
6. What is a dynamic block in Terraform?
7. How can you create EC2 instances in two different AWS accounts simultaneously using Terraform?
8. How do you handle an error stating that the resource already exists when creating resources with Terraform?
9. How does Terraform refresh work?
10. How would you upgrade Terraform plugins?
11. What are the different types of Kubernetes volumes?
12. If a pod is in a crash loop, what might be the reasons, and how can you recover it?
13. What is the difference between StatefulSet and DaemonSet?
14. What is a sidecar container in Kubernetes, and what are its use cases?
15. If pods fail to start during a rolling update, what strategy would you use to identify the issue and rollback?
16. How can we enable communication between 500 AWS accounts internally?
17. How to configure a solution where a Lambda function triggers on an S3 upload and updates DynamoDB?
18. What is the standard port for RDP?
19. How do you configure a Windows EC2 instance to join an Active Directory domain?
20. How can you copy files from a Linux server to an S3 bucket?
21. What permissions do you need to grant for that S3 bucket?
22. What are the different types of VPC endpoints and when do you use them?
23. How to resolve an image pullback error when using an Alpine image pushed to ECR in a pipeline?
24. What is the maximum size of an S3 object?
25. What encryption options do we have in S3?
26. Can you explain IAM user, IAM role, and IAM group in AWS?
27. What is the difference between an IAM role and an IAM policy document?
28. What are inline policies and managed policies?
29. How can we add a load balancer to Route 53?
30. What are A records and CNAME records?
31. What is the use of a target group in a load balancer?
32. If a target group is unhealthy, what might be the reasons?
33. Can you share your screen and write a Jenkins pipeline?
34. How do you write parallel jobs in a Jenkins pipeline?

Networking Interview Q & A for Cloud and DevOps

1.What is a subnet?
Answer :- A subnet is a smaller section of a larger network. It helps organize and manage devices more effectively, just like cutting a big pizza into smaller slices makes it easier to share.

2.What is a firewall?
answer:- A firewall acts like a security guard, monitoring traffic to protect your network from unwanted access. 

3.What is NAT (Network Address Translation)?
answer:- NAT is like a translator for your network, allowing multiple devices to share one public IP address while keeping them secure. 

4.What is DNS (Domain Name System)?
answer:- DNS is the internet’s phonebook, translating website names into IP addresses so computers can find each other easily. 

5.What is a VPN (Virtual Private Network)?
answer:- A VPN creates a secure tunnel for your internet connection, protecting your data from prying eyes, especially on public Wi-Fi. 

6.What is the purpose of a load balancer?
answer:- A load balancer distributes traffic evenly across servers, preventing any one server from getting overwhelmed. 

7.What is the OSI model?
answer:- The OSI model is a framework used to understand network interactions in seven layers: Physical, Data Link, Network, Transport, Session, Presentation, and Application. Each layer has specific functions. 

8.What is a VPN tunnel?
answer:- A VPN tunnel is a secure, encrypted connection between two endpoints over the internet. It ensures that data packets are protected from interception, maintaining privacy and security. 

9.What is a public cloud?
answer:- A public cloud is like a shared space where multiple users can access resources over the internet, managed by a provider like AWS. 

10.What is an API (Application Programming Interface)?
answer:- An API is like a waiter, letting different software applications communicate and request data from each other. 

11.What is a proxy server?
answer:- A proxy server is a middleman for your internet requests, adding security and sometimes speeding up access to websites. 

12.What is bandwidth?
answer:- Bandwidth is the amount of data that can travel over a network at once, similar to how wide a highway is for cars

Observation 1:
The people who got $200k-$500k+ offers talked extensively about 3 topics: 

- mentors they learned from
- metrics they measure (and why)
- how they drive revenue for the business 

Observation 2: 
The people who got these offers only talked ~50% of the time.

They asked intentional questions about the 
- hiring manager
- company
- culture
- team
- goals

Observation 3: 
The people who did NOT get offers 
- didn't ask questions throughout the call
- didn't talk about business outcomes
- didn't talk about metrics 
- didn't give credit to mentors/team
- tried to look completely perfect


If I were interviewing right now, I would
- list the metrics I measure (and why)
- know how I drive revenue for the business 
- research the company/interviewers
- ask Qs that can't be answered by Google
- know the value I bring to the company 
- make a list of my mentors + send them a note of gratitude for teaching me XYZ 


1. Tell me about your project.
2. How does GitLab trigger pipelines automatically when a developer pushes code?
3. How do you declare dependent stages in a CI/CD YAML file?
4. What is `terraform init`?
5. What is `backend.tf`?
6. What does `terraform plan` do?
7. What happens if you give the wrong configuration or code in Terraform and run it?
8. Where do you store Docker images?
9. Kubernetes: Describe the `pod` command.
10. How do you declare context in Kubernetes?
11. What if you have more contexts and namespaces in Kubernetes?
12. What are the types of services in Kubernetes?
13. NodePort vs. ClusterIP — What's the difference?
14. Horizontal Pod Scaling vs. Vertical Pod Scaling — Explain.
15. Limit vs. request in a Kubernetes manifest file.
16. What is AWS Lambda, and what have you done with it?
17. How do you set up CloudWatch alarms, metrics, and alerts?
18. How do you create dashboards on CloudWatch for different resources or services on AWS?
19. Which build tool do you use for Java? How about for Python?
20. Explain the complete structure of GitLab CI/CD with stages.
21. How do you integrate Azure DevOps with AWS for deployments?
22. Write Terraform code for an EC2 instance with security groups using `depends_on`.
7. What is the difference between public IP and elastic IP in AWS?
8. What is the difference between an application load balancer and a Network Load Balancer?
9. What layer is the Network Load Balancer?
10. What is the difference between Application Gateway and load balancer?

1. 🔧 Explain how Linux mechanisms work, especially when the system starts.
2. 🐳 What happens when you run a container in Kubernetes? Explain the internal workings.
3. 🔀 What is the difference between git merge and git rebase?
4. 🎯 Why and when would you use the git cherry-pick command?
5. ⚙️ Key differences between Jenkins Declarative Pipeline and Scripted Pipeline?
6. 🌐 What is DNS and how does it work?
7. 🛣️ Explain how routes work in a network environment.
8. 🛡️ How do you implement network policies in Kubernetes?
9. 📊 How do Prometheus and Grafana interact? What is the source of data for Prometheus?
10. 🗄️ Which database did you use in your recent project, and why?
11. 🧠 What is an SQL index, and how does it work?
12. 🔍 Can you share a situation where you faced a challenge working with a non-technical team, and how you solved it?
13.👫 Would you describe yourself as a communicator or a problem-solver?
14. 🏅 Tell me about a situation where you took on a leadership role.
15. 🛠️ What values do you consider important when working with a team?
16. ⚽ What are your hobbies outside of technical work?
17. 📦 Why did you choose Docker in your recent project?

1. Explain the orchestration of kubernetes...?
2. What is the difference between state less and state full applications..?
3. What is the difference between Deployment and Statefulset and explain with example..?
4. What is PVC and how you are using this component in your current organization..?
5. How you can configure the ebs volume to a pod..?
6. Write a sample HPA (Horizantal pod Autoscaler) file..?
7. How do you configure Argocd for deployment purpose in k8s..?
8. Default ports of prometheus and grafana..? Prometheus - 9090 , Grafana - 3000
9. Explain Network policies in k8s..?
10. Write a sample jenkins declerative pipeline..?
11. What is the use of jenkins file..?
12. Tell me some jenkins plugins and use cases..?
13. How do you handle user authentication in jenkins..?
14. Explain the terraform commands that can be used to deploying a module ..?
15. What is the use of backend file in terraform..?
16. what is state file..?
17. what is the difference between .tf and state file..?
18. what is sonarlint..?
19. What are common errors that you are seeing in the kubernetes pods..?
20. What is the source for prometheus..?
21. Which database types are using the opensearch and grafana..?
22. Difference between daemonset and statefulset..?

1. 𝗴𝗶𝘁 𝗰𝗼𝗻𝗳𝗶𝗴 --𝗴𝗹𝗼𝗯𝗮𝗹 𝘂𝘀𝗲𝗿.𝗻𝗮𝗺𝗲 "𝘂𝘀𝗲𝗿𝗻𝗮𝗺𝗲": Set your global Git username to associate commits with your identity.
2. 𝗴𝗶𝘁 𝗶𝗻𝗶𝘁: Initialize a new Git repository in the current directory.
3. 𝗴𝗶𝘁 𝘀𝘁𝗮𝘁𝘂𝘀: Check the status of files in your working directory, showing tracked, untracked, and staged files.
4. 𝗴𝗶𝘁 𝗮𝗱𝗱 𝗳𝗶𝗹𝗲𝗻𝗮𝗺𝗲: Stage a specific file for the next commit.
5. 𝗴𝗶𝘁 𝗰𝗼𝗺𝗺𝗶𝘁 -𝗺 "𝗺𝗲𝘀𝘀𝗮𝗴𝗲": Commit your staged changes with a descriptive message.
6. 𝗴𝗶𝘁 𝗰𝗹𝗼𝗻𝗲 𝗿𝗲𝗽𝗼_𝘂𝗿𝗹: Clone a remote Git repository to your local machine.
7. 𝗴𝗶𝘁 𝗰𝗵𝗲𝗰𝗸𝗼𝘂𝘁 -𝗯 𝗯𝗿𝗮𝗻𝗰𝗵_𝗻𝗮𝗺𝗲: Create and switch to a new branch for development.
8. 𝗴𝗶𝘁 𝗯𝗿𝗮𝗻𝗰𝗵: List all branches in the repository.
9. 𝗴𝗶𝘁 𝘀𝘄𝗶𝘁𝗰𝗵 𝗯𝗿𝗮𝗻𝗰𝗵_𝗻𝗮𝗺𝗲: Switch to a specific branch in your repository.
10. 𝗴𝗶𝘁 𝗽𝘂𝘀𝗵 𝗼𝗿𝗶𝗴𝗶𝗻 𝗯𝗿𝗮𝗻𝗰𝗵_𝗻𝗮𝗺𝗲: Push your local commits to the remote repository on a specific branch.
11. 𝗴𝗶𝘁 𝗽𝘂𝗹𝗹: Fetch and merge changes from the remote repository to your local branch.
12. 𝗴𝗶𝘁 𝘀𝗵𝗼𝘄: Display information about a specific commit, including changes made and commit details.

Next Level:

1. 𝗴𝗶𝘁 𝗰𝗼𝗺𝗺𝗶𝘁 --𝗮𝗺𝗲𝗻𝗱: Modify the most recent commit with new changes or a new message.
2. 𝗴𝗶𝘁 𝗿𝗲𝗯𝗮𝘀𝗲 𝗯𝗿𝗮𝗻𝗰𝗵_𝗻𝗮𝗺𝗲: Move or combine a sequence of commits to a new base commit.
3. 𝗴𝗶𝘁 𝗺𝗲𝗿𝗴𝗲 𝗯𝗿𝗮𝗻𝗰𝗵_𝗻𝗮𝗺𝗲: Combine changes from another branch into the current branch.
4. 𝗴𝗶𝘁 𝗹𝗼𝗴: Display a detailed log of all commits in the current branch.
5. 𝗴𝗶𝘁 𝗿𝗲𝘀𝗲𝘁 --𝗵𝗮𝗿𝗱 𝗵𝗲𝗮𝗱~𝟭: Revert all changes and reset to the previous commit.
6. 𝗴𝗶𝘁 𝘀𝘁𝗮𝘀𝗵: Temporarily save your changes without committing, allowing you to work on something else.
7. 𝗴𝗶𝘁 𝗰𝗵𝗲𝗿𝗿𝘆-𝗽𝗶𝗰𝗸 𝗰𝗼𝗺𝗺𝗶𝘁_𝗵𝗮𝘀𝗵: Apply changes from a specific commit to your current branch.
8. 𝗴𝗶𝘁 𝗳𝗲𝘁𝗰𝗵: Download changes from a remote repository without applying them to the local branch.
9. 𝗴𝗶𝘁 𝗿𝗲𝗳𝗹𝗼𝗴: Show a history of where the HEAD pointer has moved, useful for recovering from mistakes.
10. 𝗴𝗶𝘁 𝗯𝗶𝘀𝗲𝗰𝘁: Use binary search to find the commit that introduced a bug.

So here are some real-time questions; zero to hero folks do give them a try:

* Resource Name Constraints: Let's say I want a predefined prefix for all resources in Terraform based on the environment (e.g., dev_ for the development environment and qa_ for the QA environment). How will you plan to do this?

* Importing Existing Resources: You have existing infrastructure not managed by Terraform. How would you bring these resources under Terraform management?

* Lifecycle Block: What is it, and why is it used on resources?

* Toolsets: What are you using to validate and check Terraform code for issues and abnormalities?

* Validating Policies: How can you define and enforce policies for resources that are created with Terraform?

-How do you handle credentials for a PHP application accessing MySQL or any other secrets in Docker?
-What is the command for running container logs?
-Have you upgraded any Kubernetes clusters?
-How do you deploy an application in a Kubernetes cluster?
-How do you communicate with a Jenkins server and a Kubernetes cluster?
-Which DevOps tools are you proficient with?
-Can you describe the CI/CD workflow in your project?
-How do you handle the continuous delivery (CD) aspect in your projects?
-What methods do you use to check for code vulnerabilities?
-What AWS services are you proficient in?
-How would you access data in an S3 bucket from Account A when your application is running on an EC2 instance in Account B?
-How do you provide access to an S3 bucket, and what permissions need to be set on the bucket side?
-How can Instance 2, with a static IP, communicate with Instance 1, which is in a private subnet and mapped to a multi-AZ load balancer?
-How do you pass arguments to a VPC while using the `terraform import` command?
-What are the prerequisites before importing a VPC in Terraform?
-If an S3 bucket was created through Terraform but someone manually added a policy to it, how do you handle this situation using IaC?
-For an EC2 instance in a private subnet, how can it verify and download required packages from the internet without using a NAT gateway or bastion host? Are there any other AWS services that can facilitate this?
-What is the typical latency for a load balancer, and if you encounter high latency, what monitoring steps would you take?
-If your application is hosted in S3 and users are in different geographic locations, how can you reduce latency?
-Which services can be integrated with a CDN (Content Delivery Network)?
-How do you dynamically retrieve VPC details from AWS to create an EC2 instance using IaC?
-How do you manage unmanaged AWS resources in Terraform?
-Write BASH Script for Prime numbers.

𝐉𝐞𝐧𝐤𝐢𝐧𝐬
- How do you integrate unit testing, integration testing, and end-to-end testing into a Jenkins pipeline?
- How do you troubleshoot common Jenkins issues, such as build failures or performance problems?
- How do you configure a Jenkins job to trigger automatically on code changes in a Git repository?

𝐋𝐢𝐧𝐮𝐱
- How do you monitor the CPU and memory of the linux systems?
- Explain the concept of system calls and how they interact with the kernel.
- How do you manage file systems using fsck, mount, and umount?
- How do you secure a Linux system using iptables or firewalld?

𝐀𝐧𝐬𝐢𝐛𝐥𝐞
- What are dynamic inventory in ansible and how do you manage it?
- Create a ansible role to install httpd in 10 servers with tags
- Can you design a playbook with notifiers and handlers for 10 tools installations?

𝐊𝐮𝐛𝐞𝐫𝐧𝐞𝐭𝐞𝐬
- Design the kubernetes manifest file to create the CronJob with concurrency as forbid and to run for every 1 minute with nginx container having tag as latest
- What will be the command to add the annotation and the labels for the existing pod?
- Design the kubernetes cluster with Ingress.
- A sudden surge in traffic causes a web application to become unresponsive what will be the steps you will take to mitigate
- Design the deployment of the pod with replica set set as 3 and having apache httpd image running as a container.

𝗖𝗜/𝗖𝗗
- How do you integrate automated testing into a CI/CD pipeline for ETL jobs?
- How do you manage environment-specific configurations in a CI/CD pipeline?
- How is version control managed for database schemas and ETL scripts in a CI/CD pipeline?

𝐃𝐨𝐜𝐤𝐞𝐫
- How do you define a multi-container application using Docker Compose?
- How do you ensure the security of Docker images and containers?
- How does Docker handle network communication between containers?

𝗣𝘆𝘁𝗵𝗼𝗻
- What are the key differences between Python 2 and Python 3?
- How do you integrate Python with popular DevOps tools like Jenkins and Ansible?
- How do you use Python to interact with cloud platforms like AWS, GCP, and Azure?
- How do you troubleshoot Python scripts in a DevOps environment?
- What are the benefits of using Python for configuration management over other tools?

Make a multi stage declarative jenkinsfile with the following requirements:
 
1. Only allowed to run on nodes labeled 'test-runner'
2. First stage - Installs dependencies with the bash command "flutter pub get"
3. Second Stage and Third stage run in parallel.
4. Second Stage runs a style check with the bash command "flutter analyze".
5. Third Stage runs a static analyze check with the bash command "flutter format".
6. Make sure all stages store individual logs for each command.
7. Archive these logs in Jenkins so they are available to developers.
8. Send an alert with Success message if all stages pass. If there are errors, send an alert with the name of the stage that failed.

pipeline {
    agent { label 'test-runner' }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    def logFile = 'install_dependencies.log'
                    sh "flutter pub get > ${logFile} 2>&1"
                    archiveArtifacts artifacts: logFile, allowEmptyArchive: true
                }
            }
        }
        stage('Parallel Stages') {
            parallel {
                stage('Style Check') {
                    steps {
                        script {
                            def logFile = 'style_check.log'
                            sh "flutter analyze > ${logFile} 2>&1"
                            archiveArtifacts artifacts: logFile, allowEmptyArchive: true
                        }
                    }
                }
                stage('Static Analyze Check') {
                    steps {
                        script {
                            def logFile = 'static_analyze_check.log'
                            sh "flutter format > ${logFile} 2>&1"
                            archiveArtifacts artifacts: logFile, allowEmptyArchive: true
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                def failedStages = []
                if (currentBuild.result == 'FAILURE') {
                    failedStages = currentBuild.stages.findAll { it.status == 'FAILED' }.collect { it.name }
                }
                if (failedStages.isEmpty()) {
                    emailext subject: 'Jenkins Build Success', body: 'All stages passed successfully!', to: 'developers@example.com'
                } else {
                    emailext subject: 'Jenkins Build Failure', body: "The following stages failed: ${failedStages.join(', ')}", to: 'developers@example.com'
                }
            }
        }
    }
}

Explain how kubernetes networking works, save this post and share with your community

Pod Networking:

Pod: A group of containers that share the same network namespace.
Pod IP: Each pod is assigned a unique IP address within the cluster.
CNI (Container Network Interface): A plugin that manages pod networking. It creates network interfaces, assigns IP addresses, and configures routes.

Cluster Networking:

Network Plugin: A component responsible for providing network connectivity between pods and external services.
Overlay Networks: These networks create a virtual network on top of the physical network.
Routing: The network plugin handles routing between pods in different nodes.
Service Discovery: Kubernetes provides a mechanism to discover services within the cluster. This allows pods to communicate with each other without knowing their exact IP addresses.

Service Mesh:

A layer of abstraction: A service mesh provides a layer of abstraction for service-to-service communication.
Traffic Management: It handles tasks like load balancing, fault tolerance, and security.
Sidecar Proxy: Each pod typically has a sidecar proxy that handles network traffic.

Key Concepts and Components:

CNI (Container Network Interface): A plugin that manages pod networking.
Network Plugin: A component responsible for providing network connectivity between pods and external services.
Overlay Networks: These networks create a virtual network on top of the physical network.
Service Discovery: Kubernetes provides a mechanism to discover services within the cluster.
Service Mesh: A layer of abstraction for service-to-service communication.
Sidecar Proxy: A proxy that handles network traffic for a pod.

Common Networking Approaches:

Overlay Networks: VXLAN, Calico, Flannel, Weave Net, and Cilium are common overlay network solutions.
Host-Port Networking: Pods can be directly exposed to the host network.
IP-Per-Pod: Each pod gets its own IP address.
Additional Considerations:
Network Policies: Kubernetes allows you to define network policies to control traffic flow between pods.
Ingress Controllers: Ingress controllers handle incoming traffic to the cluster.
Load Balancing: Kubernetes provides built-in load balancing capabilities.

1. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗽𝗼𝗱𝘀 --𝗮𝗹𝗹-𝗻𝗮𝗺𝗲𝘀𝗽𝗮𝗰𝗲𝘀: Check the status of all pods across namespaces to identify failures.
2. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗱𝗲𝘀𝗰𝗿𝗶𝗯𝗲 𝗽𝗼𝗱 𝗽𝗼𝗱_𝗻𝗮𝗺𝗲: Gather detailed information about a failed pod.
3. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗹𝗼𝗴𝘀 𝗽𝗼𝗱_𝗻𝗮𝗺𝗲 -𝗰 𝗰𝗼𝗻𝘁𝗮𝗶𝗻𝗲𝗿_𝗻𝗮𝗺𝗲: View logs of a specific container inside a pod to troubleshoot issues.
4. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗲𝘃𝗲𝗻𝘁𝘀 --𝗮𝗹𝗹-𝗻𝗮𝗺𝗲𝘀𝗽𝗮𝗰𝗲𝘀 --𝘀𝗼𝗿𝘁-𝗯𝘆='.𝗺𝗲𝘁𝗮𝗱𝗮𝘁𝗮.𝗰𝗿𝗲𝗮𝘁𝗶𝗼𝗻𝗧𝗶𝗺𝗲𝘀𝘁𝗮𝗺𝗽': Review recent events for clues on crashes and errors.
5. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗻𝗼𝗱𝗲𝘀: Verify the status of nodes in the cluster, checking for node failures.
6. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗱𝗿𝗮𝗶𝗻 𝗻𝗼𝗱𝗲_𝗻𝗮𝗺𝗲 --𝗶𝗴𝗻𝗼𝗿𝗲-𝗱𝗮𝗲𝗺𝗼𝗻𝘀𝗲𝘁𝘀: Safely evacuate and cordon a node for recovery operations.
7. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗰𝗼𝗿𝗱𝗼𝗻 𝗻𝗼𝗱𝗲_𝗻𝗮𝗺𝗲: Mark a node as unschedulable to prevent new pods from being scheduled during recovery.
8. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗱𝗲𝗹𝗲𝘁𝗲 𝗽𝗼𝗱 𝗽𝗼𝗱_𝗻𝗮𝗺𝗲 --𝗴𝗿𝗮𝗰𝗲-𝗽𝗲𝗿𝗶𝗼𝗱=0 --𝗳𝗼𝗿𝗰𝗲: Forcefully delete a crashed pod to restart it or clear it for recovery.
9. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗿𝗼𝗹𝗹𝗼𝘂𝘁 𝘂𝗻𝗱𝗼 𝗱𝗲𝗽𝗹𝗼𝘆𝗺𝗲𝗻𝘁 𝗱𝗲𝗽𝗹𝗼𝘆𝗺𝗲𝗻𝘁_𝗻𝗮𝗺𝗲: Roll back a deployment in case a new rollout causes crashes.
10. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗲𝘅𝗲𝗰 -𝗶𝘁 𝗽𝗼𝗱_𝗻𝗮𝗺𝗲 -- /𝗯𝗶𝗻/𝘀𝗵: Access a container to debug and resolve application issues directly inside the pod.
11. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗰𝗼𝗺𝗽𝗼𝗻𝗲𝗻𝘁𝘀𝘁𝗮𝘁𝘂𝘀𝗲𝘀: Check the health of core cluster components like etcd, kube-apiserver, and more.
12. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝘁𝗼𝗽 𝗻𝗼𝗱𝗲𝘀: Monitor node resource usage to detect resource exhaustion causing crashes.
13. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝘁𝗼𝗽 𝗽𝗼𝗱𝘀 --𝗮𝗹𝗹-𝗻𝗮𝗺𝗲𝘀𝗽𝗮𝗰𝗲𝘀: Check pod resource usage across namespaces, identifying bottlenecks leading to crashes.
14. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗱𝗲𝗹𝗲𝘁𝗲 𝗻𝗼𝗱𝗲 𝗻𝗼𝗱𝗲_𝗻𝗮𝗺𝗲: Remove a failed node from the cluster to allow recovery operations.
15. 𝗲𝘁𝗰𝗱𝗰𝘁𝗹 --𝗲𝗻𝗱𝗽𝗼𝗶𝗻𝘁𝘀=𝗵𝘁𝘁𝗽𝘀://𝗲𝘁𝗰𝗱-𝘀𝗲𝗿𝘃𝗲𝗿:2379 𝘀𝗻𝗮𝗽𝘀𝗵𝗼𝘁 𝗿𝗲𝘀𝘁𝗼𝗿𝗲 𝗯𝗮𝗰𝗸𝘂𝗽.𝗱𝗯: Restore etcd from a snapshot in case of etcd failure.
16. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗮𝗽𝗽𝗹𝘆 -𝗳 𝗯𝗮𝗰𝗸𝘂𝗽.𝘆𝗮𝗺𝗹: Reapply configurations from a backup manifest during recovery.
17. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝘁𝗮𝗶𝗻𝘁 𝗻𝗼𝗱𝗲𝘀 𝗻𝗼𝗱𝗲_𝗻𝗮𝗺𝗲 𝗸𝗲𝘆=𝘃𝗮𝗹𝘂𝗲:𝗡𝗼𝗦𝗰𝗵𝗲𝗱𝘂𝗹𝗲: Prevent scheduling on a node experiencing issues during recovery.
18. 𝗸𝘂𝗯𝗲𝗰𝘁𝗹 𝗴𝗲𝘁 𝗲𝗻𝗱𝗽𝗼𝗶𝗻𝘁𝘀 𝘀𝗲𝗿𝘃𝗶𝗰𝗲_𝗻𝗮𝗺𝗲: Verify service endpoints during recovery to ensure services are resolving correctly.


18 -  Key Release Management Metrics

1. Percent of release success rate
This metric simply shows the percentage of planned releases that are deployed on time. There are many factors that can prevent releases from happening on schedule, some of which are difficult to avoid (such as changing priorities). For example, the 2020 State of Code Review survey cites changing requirements as the single biggest reason for missing release deadlines. Another common cause of delays is miscommunication when handing off work between departments, which can be improved by streamlining release processes and strengthening collaboration via culture and tools.

When pinpointing bottlenecks in your release process, it’s helpful to have a quick, all-inclusive view of where you are spending your time. Plutora’s dashboard below breaks down releases into phases and types, providing a granular view of the average phase and gate duration, bringing to light where constraints are in the release process.  


2. Percentage of Escaped Defects
This measures the defects that make it past production and are found by customers post-release. Naturally it’s impossible (and perhaps even undesirable) to catch all bugs, as that would involve laboriously checking and rechecking features at the cost of velocity. For example, NASA was able to achieve zero defects for its Space Shuttle Software, but at great expense -- thousands of dollars per line of code. Most projects won’t require that level of scrutiny; however,  it’s still an important measure that directly impacts customer satisfaction and maintenance costs. Ultimately, it’s up to the business to find that “sweet spot” of balancing quality and speed to best serve customers.  

Percentage of escaped defects is generally used as one of the key indicators of QA performance, but keep in mind that all bugs are not equal -- some are mission critical, while others are merely cosmetic. Since this metric does not weigh for importance, it’s helpful to discuss with your QA team for context.

3. Defect Density
The number of known defects per module or X lines of code. Defect density is a useful indicator of quality, and 1 defect per 1000 lines of code is typically seen as the standard for “good” quality. 

As always, keep context in mind. If your development team is putting forth substantial effort into optimizing code, defect density could increase even while the organization is improving behind the scenes. 

4. Number of Releases
The number of releases over a given timespan. It gives you insight into whether your release frequency is increasing, inconsistent or decreasing. Similar to the other metrics, context is helpful here. Perhaps in August there were several major releases, as opposed to minor ones in September. That’s why this data is best broken down by type or portfolio for additional detail. 

Refer to this sample dashboard from Plutora, which gives a quick and simple view of the types releases over time:


5. Deployment Duration
Usually measured in hours, this is the time it takes to execute a deployment. Deployment duration is useful for understanding whether deployments are causing delays. Many businesses are turning now to automation, which takes the load off of developers, reduces manual errors, and aids in pushing small changes to production. 

6. Release Duration
Usually measured in days, this is the time it takes to execute a release. This number measures how quickly a business can serve its customers, and can be useful for roadmap planning. Like other release metrics, this metric can be broken down into type to provide better context.

For instance, have a look at Plutora's release duration dashboard: 

release duration over time
7. Number of Release Backouts
Also known as a rollback, this counts the number of releases that fail to meet expectations and have to be reversed. If a workaround exists, a Request for Change (RFC) should be created to deploy the fix into production. Tracking the number of release backouts indicates the quality and thoroughness of development and testing.  

8. Proportion of Automatic Release Distribution
Usually a percentage, this metric refers to the proportion of new releases that are distributed automatically. A high number here indicates that your ITSM environment is operating effectively.

9. Downtime
Usually measured in minutes, downtime refers to the cumulative amount of time that users cannot access your software. Downtime is not always avoidable, but should be minimized as much as possible.

10. Number of Outages Caused by a Release
The number of outages, or interruptions to your service, that are directly caused by a release. Depending on the length of the outages, these can be very serious -- virtually incapacitating businesses, causing financial loss, and eroding customer trust. This metric helps business stakeholders to get a clear picture of the costs associated with any given release.

11. Number of Incidents Caused by a Release
The amount of incidents caused by a release. Incidents are also referred to as defects or bugs, and can be characterized as unexpected deviations in system behavior. This number is often used as a key metric in evaluating release team performance. 

Take a look at Plutora’s breakdown of releases by risk level to understand the risk profile of your releases. This can then be correlated with incidents in production.

releases by risk level
12. Percentage of All Changes Causing Major Incidents
Similar to the number of incidents caused by a release, this measures whether the change management team is doing their job effectively. 

In the dashboard below, you can see the breakdown of different types of changes over time. Simply overlay incident data over this chart to get a quick sense of where and when incidents are occurring.

changes over time dashboard
13. On-time Delivery
This measures the timeliness of delivery. When deadlines are consistently missed, it doesn’t necessarily mean that your team is doing a bad job -- perhaps the problem is a dearth of resources, or the scope of the project needs to be cut. 

14. Releases Delivered on Schedule by Application
Provides a breakdown of timely and successful releases by application. If an application is consistently behind schedule, that impacts the business when it comes to market share, security, and compliance concerns. 

For example, this dashboard shows the various releases by portfolio:

releases by portfolio
15. Mean Time to Repair (MTTR)
MTTR represents the average time required to repair a failed component or device. It calculates the time from the very beginning of an incident until the moment it’s solved, and provides a strong indication of how fast your organization responds to problems. 

To get this number, find out the total time spent on unplanned maintenance for a specific asset and divide that number by the number of failures that happened with that asset over a period of time. In general, a MTTR value of five hours or less is considered to be quite good.  

16. Average Lead Time
Also referred to as Time to Value, this measures the period of time between accepting a work item and pushing it to production. It tells you how long it takes for you to deliver on customers requests. When examining lead time, it’s important to know exactly what you’re measuring. You probably don’t want to include low priority tickets in this metric because it will lead to unreasonably high numbers; keep it limited to high and medium priority tickets. 

Average lead time is an essential metric for ensuring business continuity and planning, as it enables the PMs and C-suite to plan accordingly.

lead time dashboard
17. Average Cycle Time
Cycle time is how long it takes your team to deliver something after starting work on it. It provides a lot of insight to engineering leaders as to how their team is allocating its time. For instance, if features are sitting for days waiting for QA after developers are done coding, that’s a useful observation that can generate real change. A good dashboard here is invaluable, as it can help you to drill down deeper and see the exact distribution of how time is spent. 

cycle time dashboard
18. Average Cost Per Release
Calculate man-hours and salary per release to arrive at this number. This will give you an idea of the ROI of each release, resulting in better business decisions.


Question 7:
Problem: Given an array A of N integers, return the maximum product of any two distinct integers in the array.

Input: A = [1, 10, 2, 6, 5, 3]
Output: 60 (10 * 6)


Question 8:
Problem: Write a function that takes two sorted arrays A and B and returns a new array that contains the elements of both A and B in sorted order.

Input: A = [1, 3, 5], B = [2, 4, 6]
Output: [1, 2, 3, 4, 5, 6]
Constraints: Try to Solve this without inbuilt functions

Question 9:
Problem: Write a function that checks if a string is a palindrome. A palindrome is a string that reads the same backward as forward.

Input: S = "racecar"
Output: True
Constraints: Ignore case sensitivity and spaces for this problem.



1️⃣ Ping Test: 
Start with the `ping` command to check connectivity to a website or IP address. It helps determine whether your device can communicate with the server.

hashtag#Command
ping google.com

- Success: You're connected to the network.
- Failure: There may be a network issue or the destination is unreachable.

2️⃣ DNS Lookup:
If you can ping an IP but not a domain, there may be a DNS issue. Use `nslookup` or `dig` to test if DNS is resolving correctly.

hashtag#Command
nslookup example.com

This will show the IP address your DNS server resolves for the domain.

3️⃣ Check Network Configuration:
Check your network configuration using `ifconfig` (Linux/macOS) or `ipconfig` (Windows) to make sure your device has a proper IP address, gateway, and DNS settings.

hashtag#Command
ipconfig 

4️⃣ Telnet to Check Open Ports: 
If you're trying to access a service but it's not responding, use `telnet` to check if a specific port is open and accessible.

hashtag#Command
telnet example.com 80

If the connection fails, the service might be down or blocked by a firewall.

5️⃣Netstat for Active Connections:
Run netstat to view active network connections and see which ports are being used by applications. This is useful for checking open ports and connections on your device.

hashtag#Command
netstat -an

It will display all active network connections, which can help identify potential issues.

6️⃣ Traceroute:
Use traceroute to map the path data packets take to reach a destination, which helps diagnose where delays or blockages occur in the network.

hashtag#Command
traceroute google.com

tcpdump 

nc

1.What are the types of pipeline in Jenkins you can write.
2.How do you resolve if the build starts to fail.
3.Master slave Architecture of Jenkins.
4.Architecture of Kubernetes
5.How do you define any yaml file.
6.What are the kinds services in k8s.
7.How do you make your cluster secure.
8.Difference between Stateless and stateful.
9.Tell me about Network Policy in K8s.
10.How do you create EC2 instances using terraform
11.How do you write terraform script, explain with example.
12.Terraform as Cloud Infrastructure.
13.How do you take back up in Jenkins.

• Understanding HTTP is more important than Nginx vs Apache
 - Know how web protocols work
 - Master request/response cycles
 - Learn load balancing concepts

• Understanding Linux is more important than Ubuntu vs RHEL
 - Know how processes work
 - Master file systems
 - Learn system troubleshooting

• Understanding networks is more important than Cisco vs Juniper
 - Know how routing works
 - Master TCP/IP basics
 - Learn network debugging

• Understanding security is more important than tools vs scanners
 - Know how attacks happen
 - Master access controls
 - Learn threat modeling

• Understanding storage is more important than EBS vs EFS
 - Know how disks work
 - Master data persistence
 - Learn backup strategies

• Understanding monitoring is more important than Grafana vs Datadog
 - Know what metrics matter
 - Master alert design
 - Learn troubleshooting
 
 General Questions: 
1- Introduce yourself. 
2- Describe a challenge you faced as a devops engineer and how you overcame it. 
3- Would you like to work individually or in a team. 
4 - Tell me about something where you got a chance to learn and implement from scratch. 
5- How would you be handling a situation where you are not getting any help from your team members. 
 
AWS Questions: 
1- Launch template vs launch configuration. 
2- How do you communicate with AWS services privately without exposing to internet. 
3- NAT g/w vs NAT instance v/s Egress only IGW v/s IGW 
4- Design a 3 tier infrastructure using the AWS services which should be secure and highly available. 
5- Statefull v/s stateless firewalls. 
6- Differentiate between NLB and ALB. 
7- You want to redirect traffic from x.company.in to company.in/x, how to achieve it. 
8- Your database initially performs well, but after certain month you face slowness. How to troubleshoot and fix the same. 
 
K8s: 
1- K8s architecture 
2- Deployment v/s stateful set v/s replica set 
3- What is config map 
4- Role of etcd in kubernetes. 
5- How rolling updates work in a deployment? 
 
Docker: 
1- ADD v/s COPY 
2- Entrypoint v/s CMD 
3- How to remove all unwanted or unused docker objects from system? 
4- Multistage docker build. 
5- Is docker file immutable 
 
Terraform: 
1- What does terraform init do ? 
2- How to auto approve the terraform changes? 
3- count v/s foreach 
4- How to import an existing resource to terraform? 
5- Data black in terraform. 
6- What are provisioners in terraform? 
7- Remote backend. 
 
Linux: 
1- How to check the list of installed packages? Ans ) apt --installed
2- Command to check kernel version. Ans) uname -r . More detailed information uname -a
3- How to create a new user and add it as sudo? 
4- Command to check if a process called "a" is running or not. If running how stop it. 
5- Command to list all files less than 5mb. 
6- Hard link v/s soft link. 
7- Commands to check disk space usage and free RAM. 
 
CI/CD: 
1- Git fetch v/s Git pull 
2- Sonarqube quality gate vs quality profile. 
3- What is sonar runner. 
4- Types of pipeline in Jenkins. 
5- Scripted vs Declarative pipeline. 
6- Should we prefer artifactory to store artifacts or should we store them in s3. 
7- How to upgrade plugins in Jenkins. 
8- Usermanagement in Jenkins. 
9- Concepts about Gitlab runners. 
10- How to upgrade Jenkins. 
 
Ansible: 
1- loops in Ansible. 
2- Ansible Roles. 
3- Is Ansible idempotent? 
4- Ansible script to install nginx. 
5- Conditionals in Ansible.
