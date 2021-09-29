---
title: "Configure Execution Environment"
menutitle: "Execution Environment"
date: 2021-09-14T11:16:08-04:00
chapter: false
weight: 2
pre: "<b>2. </b>"
---

Failure injection is a means of testing resiliency by which a specific failure type is simulated on a service and its response is assessed.

You have a choice of environments from which to execute the failure injections for this lab. Bash scripts are a good choice and can be used from a Linux command line. If you prefer Python, Java, Powershell, or C#, then instructions for these are also provided.

In addition to custom scripts, you can also perform failure injection experiments using [AWS Fault Injection Simulator (FIS)](https://aws.amazon.com/fis/).

### 2.1 Setup AWS CloudShell

If you will be using **bash**, **Java**, or **Python**, and are comfortable with Linux, it is highly recommended you use AWS CloudShell for this lab. If you will _not_ be using AWS CloudShell, then skip to [Step 2.2](#setupcreds)

1. Go to the [AWS CloudShell console here](https://us-east-2.console.aws.amazon.com/cloudshell/home)
1. If this is your first time running CloudShell, then it will take less than a minute to create the environment. When you see a prompt like `[cloudshell-user@ip-10-0-49-48 ~]$`, then you can continue
1. Validate that credentials are properly setup. 
    * execute the command `aws sts get-caller-identity`
    * If the command succeeds, anf the **Arn** contains **assumed-role/TeamRole/MasterKey**, then you can continue
1. Skip to [Step 2.3](#setupenv)


### 2.2 Setup AWS credentials and configuration {#setupcreds}

**If you have chosen to use AWS CloudShell or Windows PowerShell, then _skip_ this step** 

Otherwise, your execution environment needs to be configured to enable access to the AWS account you are using for the workshop. This includes:

* Credentials - You identified these credentials [back in step 1]({{< ref "./1_deploy_infra.md#awslogin" >}})
    * AWS access key
    * AWS secret access key
    * AWS session token (used in some cases)

* Configuration
    * Region: us-east-2
    * Default output: JSON

Note: **us-east-2** is the **Ohio** region

* If you already know how to configure these, please do so now.
* If you need help, then [follow these instructions]({{< ref "Documentation/AWS_Credentials.md" >}})
* If you are using **PowerShell** for this lab, skip this step and continue to **Step 2.3**


### 2.3 Set up the programming language environment {#setupenv}

Choose the appropriate section below for your language

Using bash is an effective way to execute the failure injection tests for this workshop. The bash scripts make use of the AWS CLI. Or if you wish, you may choose one of the other languages and scripts.

{{% expand "Click here for instructions if using bash:" %}}

1. Prerequisites

     * `awscli` AWS CLI installed

            $ aws --version
            aws-cli/2.2.15 Python/3.8.8...
         * Version 2.1.12 or higher is fine
         * If you instead got `command not found` then [see instructions here to install `awscli`]({{< ref "Documentation/Software_Install.md#install-aws-cli" >}})

     * `jq` command-line JSON processor installed.

            $ jq --version
            jq-1.5-1-a5b5cbe
         * Version 1.4 or higher is fine
         * If you instead got `command not found` then [see instructions here to install `jq`]({{< ref "Documentation/Software_Install.md#jq" >}})



1. Download the {{% githublink link_name="resiliency bash scripts from GitHub" path="static/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/bash" %}} to a location convenient for you to execute them. You can use the following links to download the scripts:
      * [bash/fail_instance.sh](/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/bash/fail_instance.sh)
      * [bash/failover_rds.sh](/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/bash/failover_rds.sh)
      * [bash/fail_az.sh](/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/bash/fail_az.sh)

1. Set the scripts to be executable.  

        chmod u+x fail_instance.sh
        chmod u+x failover_rds.sh
        chmod u+x fail_az.sh

{{% /expand %}}
{{% expand "Click here for instructions if using Python:" %}}

1. Check that **python 3** is installed. This is _already_ installed with AWS CloudShell or Amazon Linux.
      ```
      $ python3 --version
      Python 3.7.10
      ```
    * Any version is fine

1. The scripts are written in python with **boto3**. This is _already_ installed with AWS CloudShell or Amazon Linux.
    * Check that boto3 is installed using this command `pip show boto3`
    * If it _not_ installed, then use your local operating system instructions to install boto3: <https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html#installation>


1. Download the {{% githublink link_name="resiliency Python scripts from GitHub" path="static/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/python/" %}} to a location convenient for you to execute them. You can use the following links to download the scripts:
      * [python/fail_instance.py](/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/python/fail_instance.py)
      * [python/fail_rds.py](/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/python/fail_rds.py)
      * [python/fail_az.py](/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/python/fail_az.py)


{{% /expand %}}
{{% expand "Click here for instructions if using Java:" %}}

1. Java and Maven must be installed

        $ mvn -version
        Apache Maven 3.0.5 (Red Hat 3.0.5-17)
        Maven home: /usr/share/maven
        Java version: 1.8.0_302, vendor: Red Hat, Inc.
        ...

1. If Maven is not installed, or Java is not 1.8 or higher, then install Maven and Java

      * For Amazon Linux and RedHat

            $ sudo yum install maven

      * For Debian, Ubuntu

            $ sudo apt install maven

1. Next choose one of the following options: **Option A** or **Option B**.

    * **Option A**: If you are comfortable with git.
      1. Clone the aws-well-architected-labs repo

              $ git clone https://github.com/awslabs/aws-well-architected-labs.git
              Cloning into 'aws-well-architected-labs'...
              ...
              Checking out files: 100% (1935/1935), done.

      1. go to the build directory

              cd aws-well-architected-labs/static/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/java/appresiliency/

    * **Option B**:
      1. Download the zipfile of the executables at the following URL <https://s3.us-east-2.amazonaws.com/aws-well-architected-labs-ohio/Reliability/javaresiliency.zip>
      1. unzip it
      1. go to the build directory: `cd java/appresiliency`

1. Build: `sudo mvn clean package shade:shade`    

1. `cd target` - this is where your `jar` files were built and where you can run from the command line


{{% /expand %}}
{{% expand "Click here for instructions if using C#:" %}}

1. Download the zipfile of the executables at the following URL. [https://s3.us-east-2.amazonaws.com/aws-well-architected-labs-ohio/Reliability/csharpresiliency.zip](https://s3.us-east-2.amazonaws.com/aws-well-architected-labs-ohio/Reliability/csharpresiliency.zip)  

2. Unzip the folder in a location convenient for you to execute the command line programs.  

{{% /expand %}}
{{% expand "Click here for instructions if using PowerShell:" %}}

1. To install the necessary AWS Tools for Powershell packages, and to setup AWS credentials for PowerShell follow the [instructions here](../documentation/tools_for_powershell/)

1. Download the {{% githublink link_name="resiliency PowerShell scripts from GitHub" path="static/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/powershell/" %}} to a location convenient for you to execute them. You can use the following links to download the scripts:
      * [powershell/fail_instance.ps1](/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/powershell/fail_instance.ps1)
      * [powershell/failover_rds.ps1](/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/powershell/failover_rds.ps1)
      * [powershell/fail_az.ps1](/Reliability/300_Testing_for_Resiliency_of_EC2_RDS_and_S3/Code/FailureSimulations/powershell/fail_az.ps1)

1. If your PowerShell scripts are refused authorization to access your AWS account, consult [Getting Started with the AWS Tools for Windows PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-started.html)

{{% /expand %}}

### 2.4 IAM Role for FIS

In this lab, some of the experiments will be executed AWS Fault Injection Simulator (FIS) in addition to using custom scripts. FIS needs a service role to inject failures for various components of a workload.

**This IAM Role has already been created for you as part of the infrastructure deployment**

You may proceed to the next step.

{{%expand "If you would like to view the instructions on how to create the IAM Role for FIS (for your information), then click here:" %}}
**These instructions are here for informational purposes only. You DO NOT need to execute these as this IAM Role was created for you as part of the infrastructure deployment**
{{% common/FISExecutionRole %}}
{{% /expand%}}

{{< prev_next_button link_prev_url="../1_deploy_infra" link_next_url="../3_failure_injection_prep/" />}}
