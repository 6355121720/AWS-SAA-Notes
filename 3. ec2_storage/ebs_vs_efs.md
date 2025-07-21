| Feature          | **EBS (Elastic Block Store)**                       | **EFS (Elastic File System)**                                         |
| ---------------- | --------------------------------------------------- | --------------------------------------------------------------------- |
| **Storage Type** | Block storage                                       | File storage                                                          |
| **Attachment**   | Attached to **one EC2 instance** at a time          | Shared with **multiple EC2 instances** (NFS protocol)                 |
| **Availability** | Tied to a **single AZ**                             | Multi-AZ, accessible across a **region**                              |
| **Performance**  | Measured in **IOPS (Input/Output per Second)**      | Measured in **Throughput (MB/s)**                                     |
| **Durability**   | 99.999% durability (data replicated within AZ)      | 99.999999999% durability (across AZs)                                 |
| **Latency**      | **Low latency**, suitable for block-level access    | **Higher latency**, due to NFS and multi-AZ                           |
| **Data Access**  | Through OS block device (e.g., `/dev/xvdf`)         | Via mount target (e.g., `/mnt/efs`)                                   |
| **Use Cases**    | OS boot volumes, RDS, NoSQL DBs, transactional apps | Web servers, container storage, Big Data pipelines                    |
| **Backup**       | Use **EBS Snapshots** to S3                         | **Automatic backups** (if enabled) to S3                              |
| **Scaling**      | Manual scaling (volume size, type, performance)     | **Auto-scalable** (grows and shrinks as needed)                       |
| **Pricing**      | Pay for provisioned size (GB/month + IOPS)          | Pay for actual usage (GB/month) + throughput if burst limits exceeded |
| **Encryption**   | KMS encryption supported                            | KMS encryption supported                                              |
