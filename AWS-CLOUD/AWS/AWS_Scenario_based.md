###  1.Scenario: EC2 instance not accessible via SSH

Q: Your EC2 instance is running, but you can't SSH into it. What steps will you take?

A:

* CheckSecurity Group: Ensure inbound port 22 is open for your IP.
* CheckNACLs: Ensure port 22 is allowed in both inbound and outbound.
* CheckKey Pair: Ensure you're using the correct `.pem` file.
* CheckPublic IP: Ensure the instance has a public IP or Elastic IP.
* CheckOS firewall: Ensure `iptables` or `firewalld` isn't blocking.
* UseSession Manager if SSH fails but SSM is enabled.



###  2.Scenario: S3 Bucket should be public for hosting a website, but users get Access Denied

Q: Your static website is hosted on S3, but accessing the URL gives "Access Denied". Why?

A:

* Ensurebucket policy allows public `s3:GetObject`.
* Ensure objects arenot blocked by Block Public Access settings.
* Make sure files have correctACLs or rely solely on bucket policy.
* The index document should be specified inStatic Website Hosting settings.



###  3.Scenario: Auto Scaling Group is not launching instances

Q: Your Auto Scaling Group is created but isn’t launching any instances. What could be wrong?

A:

* Check iflaunch template/configuration is correct.
* CheckVPC subnet availability and capacity.
* Ensure there'sno capacity restriction (e.g., limits or AZ issues).
* VerifyIAM roles for EC2 launch permissions.
* Reviewscaling policies or scheduled actions that might prevent scaling.



###  4.Scenario: Application is slow due to DB latency

Q: Your app is slow and you suspect it's the RDS database. What will you check?

A:

* CheckCloudWatch metrics: CPU, DB connections, IOPS.
* EnablePerformance Insights to analyze slow queries.
* ReviewDB instance class and consider upgrading.
* Checknetwork latency between app and DB (same AZ preferred).
* Considerread replicas for read-heavy workloads.



###  5.Scenario: You need to restrict EC2 access to a specific S3 bucket

Q: How will you allow an EC2 instance to access only one specific S3 bucket?

A:

* Attach anIAM role to the EC2 instance.
* Define anIAM policy like:

```json
{
  "Effect": "Allow",
  "Action": "s3:*",
  "Resource": [
    "arn:aws:s3:::my-bucket",
    "arn:aws:s3:::my-bucket/*"
  ]
}
```

* Avoid using wide `s3:*` access unless necessary.



###  6.Scenario: How to deploy app in multiple AZs with HA

Q: How do you ensure high availability for your web app across AZs?

A:

* UseAuto Scaling Group with instances in multiple AZs.
* Put instances behind anApplication Load Balancer (ALB).
* UseRDS Multi-AZ for database.
* Store static assets inS3.
* UseRoute 53 health checks for DNS-based failover if needed.



###  7.Scenario: Cost is increasing unexpectedly

Q: Your AWS bill suddenly spiked. What do you do?

A:

* CheckAWS Cost Explorer for trends.
* Look for untagged or new resources.
* Usebudgets and alerts for thresholds.
* Checkidle resources: unattached EBS, unused Elastic IPs, underutilized EC2.
* ConsiderSavings Plans orReserved Instances.



###  8.Scenario: S3 data lifecycle management

Q: You want to automatically move S3 data to Glacier after 30 days. How?

A:

* Configure anS3 Lifecycle Policy:

  * Transition to Glacier after 30 days.
  * Optionally delete after 1 year.
  * Apply policy to prefix or entire bucket.

###  9. Scenario: You accidentally terminated a production EC2 instance

Q: What steps do you take to recover?

A:

* Check for EBS volumes—if not deleted, you can attach them to a new EC2.
* Launch a new EC2 instance in the same AZ.
* Attach the old root volume as a secondary volume.
* Recover data or set it as the root if needed.
* Use AMI snapshots or backups if previously configured.



###  10. Scenario: You need to migrate data between S3 buckets in different regions

Q: What’s the best way to do it?

A:

* Use AWS S3 Batch Operations or `aws s3 sync` with `--source-region` and `--region`.
* Example:

  ```bash
  aws s3 sync s3://source-bucket s3://destination-bucket --source-region us-east-1 --region us-west-2
  ```
* Consider S3 Cross-Region Replication for continuous sync.


###  11. Scenario: Team members are accidentally deleting resources

Q: How do you restrict this behavior?

A:

* Use IAM policies with explicit denies on delete actions.
* Implement resource tagging policies.
* Enable CloudTrail to track all delete events.
* Use AWS Config Rules to detect and notify on non-compliant actions.
* Use Service Control Policies (SCPs) if using AWS Organizations.


###  12. Scenario: Application logs are too large and cost is growing

Q: How do you optimize storage of CloudWatch logs?

A:

* Set log retention policies in CloudWatch Logs.
* Export logs to S3 and archive with lifecycle rules (e.g., move to Glacier).
* Compress logs before exporting (if using Lambda or custom processing).
* Use filters to only log important data.



###  13. Scenario: ALB gives 504 Gateway Timeout

Q: What could cause this and how do you fix it?

A:

* Backend is not responding within ALB timeout (default: 60s).
* Health checks failing — check target group status.
* App could be blocking on DB or external API.
* Fix:

  * Optimize backend response time.
  * Adjust ALB timeout if needed.
  * Scale backend properly.
  * Ensure targets are healthy and in service.



###  14. Scenario: You need to restrict S3 upload to only allow specific file types

Q: How do you enforce that only `.jpg` or `.png` files are uploaded?

A:

* Enforce via application logic (client-side check).
* Add a Lambda trigger (on upload) to scan and delete invalid files.
* Optionally use a pre-signed URL pattern with file extension validation.



###  15. Scenario: You want to automate environment provisioning

Q: How do you do this in AWS?

A:

* Use Terraform or CloudFormation to define infrastructure as code.
* Use parameterized templates to support multiple environments (dev/stage/prod).
* Integrate with CI/CD tools like Jenkins, GitHub Actions, or CodePipeline.
* Store state remotely (e.g., Terraform in S3 + DynamoDB for locking).



###  16. Scenario: API Gateway + Lambda throws CORS error

Q: How do you fix this?

A:

* Enable CORS in API Gateway method settings.
* Add `Access-Control-Allow-Origin` in Lambda response headers.
* For `OPTIONS` preflight requests, configure a mock integration or enable in the Gateway directly.

