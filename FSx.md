# FSx

Created By: Keishin CHOU
Last Edited: Apr 22, 2020 12:04 PM

### Amazon FSx for Windows (File Server)

- Fully managed **Windows** file system share drive that runs Windows Server Message Block (**SMB**) - based file servers.
- Supports SMB protocol & Windows NTFS.
- Designed for Windows and Windows applications.

### Windows FSx vs EFS

- EFS
    - EFS if for **Linux** server only.
    - A managed NAS filer for EC2 instances based on Network File System (NFS) version 4.
    - One of the first sharing protocols native to Unix and Linux.

### Amazon FSx for Lustre

- Lustre is a type of parallel distributed file system, for large-scale computing.
- Machine Learning, High Performance Computing (HPC)
- Video Processing, Financial Modeling, Electronic Design Automation
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
- Seamless integration with S3
    - Can “read S3” as a file system (through FSx)
    - Can write the output of the computations back to S3 (through FSx)
- Can be used from on-premise servers

### EFS vs FSx for Windows vs FSx for Lustre

- EFS
    - When you need distributed, highly resilient storage for Linux instances and Linux-based applications.
- FSx for Windows
    - When you need to centralized storage for Windows-based applications such as Sharepoint, Microsoft SQL Server, Workspaces, IIS Web Server or any other native Microsoft applications.
- FSx for Lustre
    - When you need high-speed, high-capacity distributed storage. This will be for applications that do High Performance Compute (HPC), financial modeling etc.
    - FSx for Lustre can store data directly on S3.