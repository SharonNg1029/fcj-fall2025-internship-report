---
title: "Blog 2"
date: 2025-10-04
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Introducing managed accounting for AWS Parallel Computing Service

> by Ramin Torabi, Nick Ihli, and Tarun Mathur | on 15 MAY 2025 | in [AWS Parallel Computing Service](https://aws.amazon.com/blogs/hpc/category/compute/aws-parallel-computing-service/) , [High Performance Computing](https://aws.amazon.com/blogs/hpc/category/high-performance-computing/), [ Technical How-to](https://aws.amazon.com/blogs/hpc/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/hpc/introducing-managed-accounting-for-aws-parallel-computing-service/) | [Share](https://aws.amazon.com/vi/blogs/hpc/introducing-managed-accounting-for-aws-parallel-computing-service/?fbclid=IwY2xjawNM0VpleHRuA2FlbQIxMABicmlkETFyQ01ESjRnbnpIU2RNWE5YAR4f-K9Oq4I7g2m7uxmLNKga5n8wdWk8TNtUjDNzteuPZ7RO-knTAHgx9EDW-A_aem_5AzfzcVcB_1yEvrtRQn-vA)

_This post was contributed by Ramin Torabi and Tarun Mathur from AWS, and Nick Ihli from SchedMD._

AWS Parallel Computing Service (PCS) is a managed service that makes it easier for you to run and scale your high performance computing (HPC) workloads on AWS using Slurm. Organizations running HPC clusters want to monitor resource utilization, enforce resource limits, and manage access-control to specific capacity across users and projects. They want to understand “who did what” in their cluster for leadership reporting, capacity planning, and budgeting purposes. PCS now supports accounting, a Slurm feature that enables these activities in a cluster. PCS manages the accounting database for the cluster, so that you don’t have to setup and manage a separate accounting database.

In this post, we’ll show you how this works, and point you to some actual use cases you can try yourself.

## Setup

Follow these steps to enable accounting in PCS:

1. [Create a new PCS cluster](https://docs.aws.amazon.com/pcs/latest/userguide/getting-started_create-cluster.html) with Slurm 24.11 or later, opt in to the accounting feature, and configure [optional accounting parameters](https://docs.aws.amazon.com/pcs/latest/userguide/slurm-accounting.html) as shown in Figure-1.
2. Once the cluster status is Active, verify accounting is enabled and review configured parameters in the cluster details console page.
3. Configure and connect to a login node [as described here](https://docs.aws.amazon.com/pcs/latest/userguide/working-with_login-nodes.html), and perform accounting commands using the root account.

![The "Create Cluster" console experience where you can enable and configure accounting](/images/3-Translated-Blogs/Blog2_1.png)

<center><i>Figure 1 – The “Create Cluster” console experience where you can enable and configure accounting.</i></center>

## Use Case 1: Attribute Usage to Projects

Organizations want visibility into resource usage for each project or department so they can internally chargeback relevant cost centers. To do so, they need to track and attribute usage at different levels. One scenario in Figure-2:

1. Create 3 users, create accounts `proj\_physics` and `proj\_chemistry`, and add users to accounts with the `sacctmgr` function. An ‘account’ is an organizational unit used to group and manage users.
2. Each user submits a single job attributed to the `proj\_physics` account with the `–account= <account\_name>` flag.
3. Validate that those jobs are attributed to the correct account proj_physics by looking up accounting data with the `sacct` function.
4. Validate that user1 is a member of both `proj\_physics` and `proj\_chemistry` accounts.

![Example workflow of project attribution](/images/3-Translated-Blogs/Blog2_2.png)

<center><i>Figure 2 – Example workflow of project attribution.</i></center>

## Use Case 2: Enforce Limits

Organizations want to set constraints on particular users or projects so one party isn’t hoarding resources. One scenario:

1. Set a limit of 6000 CPU minutes (100 CPU hours) for a particular user with the command
   `sacctmgr modify user username set GrpTRESRunMins=cpu=6000`
2. Run the validation in Figure-3 to check the limit was set correctly.
3. Let’s assume the user has already used 95 CPU hours, and then tries to submit a job that would exceed their quota:
   `sbatch --cpus-per-task=10 --time=1:00:00 myjob.sh`. This job requests 10 CPUs for 1 hour, which would use 10 CPU hours, exceeding the remaining 5 CPU hours in the user’s quota.
4. The job submission will fail and the user will see the error in Figure-4.
5. The user can then submit a smaller job of say 4 CPU hours which would be accepted as it fits within the remaining quota:
   `sbatch --cpus-per-task=2 --time=2:00:00 [smalljob.sh](http://smalljob.sh)`

![Example check to identify a limit was set correctly](/images/3-Translated-Blogs/Blog2_3.png)

<center><i>Figure 3 – Example check to identify a limit was set correctly.</i></center>

![Example output of an error due to a job submission that exceeds user limit](/images/3-Translated-Blogs/Blog2_4.png)

<center><i>Figure 4 – Example output of an error due to a job submission that exceeds user limit.</i></center>

## Use Case 3: Generate Usage Reports

Organizations want usage report summaries to assess their resource utilization and plan future capacity allocations. One scenario:

1. Query all jobs run in your cluster in the past week with the command
   ```
   sacct --starttime=$(date -d "7 days ago" +%Y-%m-%d) --
   format="JobID,User,JobName,Partition,Account,AllocCPUS,State,ExitCode"
   ```
2. The example output in Figure-5 shows the unique JobID of each job submission, which user submitted it, which partition (queue) the job was submitted from, how many CPUs were used to run that job, and the status of that job. Analyze that data to identify broader trends in your cluster. Note that most submitted jobs were completed, yet job 1236 failed, job 1238 got cancelled, and job 1240 is a large running job with 16 allocated CPUs.
3. Query utilization over a single month with the command

```
sreport cluster AccountUtilizationByUser start=2025-04-01 end=2025-04-30 -t percent
format="Accounts,Login,Proper,Used"
```

4. The example output in Figure-6 shows cluster utilization by project and user. Analyze these trends to identify adjustments to your allocation strategy across users and accounts. Identify whether it is fair that `project\_a01` and particularly `user1` are hoarding 42% of resources over the month.
5. Query top users over the prior month with the command

```
sreport user topusage start=2025-03-01 end=2025-03-31
```

6. The example output in Figure-7 lists the top users by CPU minutes in the cluster. Note that `user1` continues to be the largest user in the prior month.

![Figure 5 – Weekly Jobs report using `sacct` command.](/images/3-Translated-Blogs/Blog2_5.png)

<center><i>Figure 5 – Weekly Jobs report using  <code>sacct</code> command.</i></center>

![Figure 6 – Monthly cluster utilization `report` using sreport command.](/images/3-Translated-Blogs/Blog2_6.png)

<center><i>Figure 6 – Monthly cluster utilization report using  <code>sreport</code> command.</i></center>

![Figure 7 – Monthly top users report using `sreport` command.](/images/3-Translated-Blogs/Blog2_7.png)

<center><i>Figure 7 – Monthly top users report using <code>sreport</code> command.</i></center>

## Use Case 4: Identify Job Issues

Individual users want usage report summaries to identify and remediate job failures. One scenario:

1. User checks their failed jobs in the past week with the command
   ```
   sacct -u username --starttime=$(date -d "7 days ago" +%Y-%m-%d) --format="JobID,JobName,State,ExitCode,Start,End,MaxRSS,MaxVMSize,Comment"
   ```
2. The example output in Figure-8 helps the user identify that two jobs have failed due to high memory usage, and the third job succeeded as it was more memory efficient (2800MB used).
3. The user reviews the job scripts for both cnn_test and bert_run and identifies the root cause – the scripts are not requesting enough memory. The user can then evaluate whether they want to modify the scripts to request sufficient memory and re-submit those two jobs.

![Figure 8 – Weekly job failures report for a particular user using `sacct` command.](/images/3-Translated-Blogs/Blog2_8.png)

<center><i>Figure 8 – Weekly job failures report for a particular user using  <code>sacct</code>command.</i></center>

For more accounting use cases see SchedMD documentation for the [sacctmgr](https://slurm.schedmd.com/sacctmgr.html), [sacct](https://slurm.schedmd.com/sacct.html), and [sreport](https://slurm.schedmd.com/sreport.html) commands.

## Pricing

Enabling accounting will incur two additional charges – an hourly accounting usage fee that varies by cluster controller size chosen, and an accounting storage fee that is billed in per GB-month increments. The accounting usage fee is billed while accounting is enabled on your cluster, and the accounting storage fee scales based on the number of accounting records stored (configurable via the Default Purge Time parameter explained in the [documentation](https://docs.aws.amazon.com/pcs/latest/userguide/slurm-accounting.html)). More details are on the [PCS pricing page](https://aws.amazon.com/pcs/pricing/).

## Conclusion

In this post, we showed how you can leverage the managed accounting feature in AWS Parallel Computing Service to monitor cluster usage on your cluster. Get started today by visiting the [AWS PCS management console](https://console.aws.amazon.com/pcs/home). Let us know what you think!

TAGS: [HPC](https://aws.amazon.com/blogs/hpc/tag/hpc/)

|                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![Ramin Torabi](/images/3-Translated-Blogs/Blog2_9.jpg)  | **Ramin Torabi**<br><br>Ramin Torabi is a Senior Specialist HPC Solutions Architect at AWS. He enables customers in central Europe architecting their solutions around HPC. He has more than 15 years of experience with HPC and CAE especially in the Automotive, Aerospace, and other manufacturing industries. Ramin received a "Dr. rer. nat." (PhD) in theoretical nuclear structure physics from the Technical University of Darmstadt in 2009. |
| ![Nick Ihli](/images/3-Translated-Blogs/Blog2_10.jpg)    | **Nick Ihli**<br><br>Nick Ihli is the Director of Solutions Engineering at SchedMD. He's been focused on HPC schedulers for 20 years and specifically has expertise with running Slurm in the cloud. He loves helping customers become more efficient with Slurm in HPC and AI environments. When he's not talking Slurm, you can find him coaching youth on the football field or basketball court.                                                  |
| ![Tarun Mathur](/images/3-Translated-Blogs/Blog2_11.jpg) | **Tarun Mathur**<br><br>Tarun Mathur is a Product Manager covering HPC and scientific computing at AWS. His goal is for customers running HPC workloads to have a smooth orchestration experience on AWS. Outside work, he enjoys Brazilian jiu-jitsu, mountaineering, and searching for the best rooftop bar.                                                                                                                                        |
