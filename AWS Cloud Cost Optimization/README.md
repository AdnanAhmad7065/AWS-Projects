# AWS Cloud Cost Optimization - Identifying Stale Resources

## Identifying Stale EBS Snapshots

In this example, we'll create a Lambda function that identifies EBS snapshots that are no longer associated with any active EC2 instance and deletes them to save on storage costs.

### Description:

The Lambda function fetches all EBS snapshots owned by the same account ('self') and also retrieves a list of active EC2 instances (running and stopped). For each snapshot, it checks if the associated volume (if exists) is not associated with any active instance. If it finds a stale snapshot, it deletes it, effectively optimizing storage costs.

# Step-by-Step Implementation :-

### 1. Set Up an EC2 Instance and Create an EBS Snapshot

Go to the AWS Management Console and navigate to EC2.

Launch a new EC2 instance (e.g., Amazon Linux 2 or Ubuntu). AWS will automatically attach an EBS volume to the instance.

Once the EC2 instance is running, create a snapshot of the attached EBS volume:

Select the Volume linked to the instance.

Click Actions → Create Snapshot.

Note down the Snapshot ID for reference.
### 2. Create an AWS Lambda Function

Go to the Lambda service in the AWS Management Console.

Click Create function → Author from scratch.

Enter a Function name (e.g., delete-stale-ebs-snapshots).

Select Python 3.x as the runtime environment.

Set the execution timeout to 10 seconds (default is 3 seconds).

In the Function code section, upload or paste the provided Python script (ebs_stale_snapshots.py).

### 3. Assign Necessary Permissions to Lambda

In the Lambda function's Permissions tab, click on the attached IAM role.

Add the following policies to allow the function to interact with EC2 and delete snapshots:

AmazonEC2ReadOnlyAccess (to fetch volumes and snapshots).

AmazonEC2FullAccess (to delete snapshots).

Alternatively, you can create a custom policy with these permissions:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeSnapshots",
        "ec2:DescribeVolumes",
        "ec2:DeleteSnapshot",
        "ec2:DescribeInstances"
      ],
        "Resource": "*"
    }
  ]
}

### 4. Delete the EC2 Instance

Return to the EC2 Dashboard.

Select and Terminate the EC2 instance you created earlier.

This will also delete the associated EBS volume, making its snapshot stale.

### 5. Test the Lambda Function

In the Lambda console, click on Deploy to save your changes.

Create a test event (you can leave the default JSON input).

Click Test to execute the function.

If successful, any stale EBS snapshots will be deleted automatically, reducing storage costs.

✅ Verification

Go to the EC2 → Snapshots section.

Confirm that the snapshot created earlier has been deleted.
