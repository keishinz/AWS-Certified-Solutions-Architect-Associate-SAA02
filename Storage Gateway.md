# Storage Gateway

Created By: Keishin CHOU
Last Edited: Apr 22, 2020 10:52 AM

### Overview

- AWS Storage Gateway connects an on-premises software appliance with cloud-based storage to provide seamless integration with data security features between your on-premises IT environment and the AWS storage infrastructure.
- AWS Storage Gateway offers file-based, volume-based, and tape-based storage solutions.

![Storage%20Gateway/Untitled.png](Storage%20Gateway/Untitled.png)

### File Gateway

- A hard disk in the cloud. Store files as objects in S3, with a local cache for low-latency access to your most recently used data.
- Configured S3 buckets are accessible using NFS and/or SMB protocol.
- Supports S3 standard, S3 IA, S3 One Zone IA.
- Bucket access using IAM roles for each File Gateway.
- Most recently used data is cached in the File Gateway.
- Can be mounted on many servers.

![Storage%20Gateway/Untitled%201.png](Storage%20Gateway/Untitled%201.png)

### Volume Gateway

- Block storage in S3 with point-in-time backups as EBS snapshots.
- Dataset backup in the cloud using iSCSI block protocol.
- Data written to these volumes can be asynchronously backed up as point-in-time snapshots of your volumes, and stored in the cloud as EBS snapshots.
- Snapshots are incremental backups that capture only changed blocks.
- Cached Volumes
    - Use S3 as your primary data storage.
    - Retain recently used data locally in Storage Gateway.
    - ⇒ **S3 is primary storage. Save recently used data (not all data) locally.**

    ![Storage%20Gateway/Untitled%202.png](Storage%20Gateway/Untitled%202.png)

- Stored Volumes
    - You store your (primary) data locally.
    - Asynchronously back up the **whole data** to AWS with schedule.
    - In the form of EBS snapshot.
    - ⇒ **Local storage is primary. Back up the whole data to S3.**

    ![Storage%20Gateway/Untitled%203.png](Storage%20Gateway/Untitled%203.png)

    ### Tape Gateway

    - Back up your data to S3 and archive in Glacier using your existing **tape-based** processes.

    ![Storage%20Gateway/Untitled%204.png](Storage%20Gateway/Untitled%204.png)

    ### Storage Gateway Exam Tips

    - On premise data to the cloud ⇒ Storage Gateway
    - File access / NFS ⇒ File Gateway (Backed up by S3)
    - Volumes / Block Storage / iSCSI ⇒ Volume Gateway (Backed up by S3 with EBS Snapshots)
    - VTL Tape solution / Back up with iSCSI ⇒ Tape Gateway (Backed up with S3 and Glacier)