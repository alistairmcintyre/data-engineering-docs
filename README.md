# data-engineering-docs
docs to help decide most appropriate data engineering solutions, given a set of conditions



## Spark on AWS - What Infrastructure to use

EKS gives full control over Spark version, JVM tuning, custom base images, resource quotas, spot instance bin-packing, and cluster autoscaler behaviour — none of which EMR exposes cleanly

  | Scale / Control Need | Typical Choice | Pros | Cons |                                                                                                                                                      
  |---|---|---|---|                                                                                                                                                                                            
  | Small-medium team, managed infra | EMR Serverless | Zero cluster management; auto-scales to zero; pay per use; no AMI/bootstrap overhead; uses Docker image directly | Hard disk ceiling (200 GB standard, 
  2 TB shuffle-optimized); limited JVM tuning; no custom AMI; cold start latency; worker size caps (max 16 vCPU / 120 GB) |                                                                                    
  | Mid-scale, need more control | EMR on EC2 (transient clusters) | Instance-store NVMe (multi-TB scratch); full instance type choice; familiar AWS managed service; no K8s operational overhead | Bootstrap  
  overhead on every run; source distribution story (no Docker — needs `--py-files` or similar); slower cluster startup (~5 min); paying for idle master node; AWS config constraints still apply |             
  | Large scale, full control, K8s native | Spark on EKS | Full Spark version control; custom base images; bin-pack spot instances efficiently; cluster autoscaler; multi-tenant; portable across clouds | High
   operational burden (K8s expertise required); complex networking (pod-to-pod shuffle); shuffle spill to EBS not instance-store by default; harder to debug than EMR console |                                
  | Hyperscaler (Apple, CrowdStrike, Google) | Custom internal platform on K8s | Complete control of every layer; Scala/Java first-class; petabyte-scale tuning; multi-tenant with resource quotas; no vendor
  lock-in | Requires dedicated platform engineering team to build and maintain; years of investment; not viable for small orgs |    
