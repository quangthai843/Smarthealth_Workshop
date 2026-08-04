---
title: "High Availability Simulation"
weight: 3
chapter: false
pre: " <b> 9.3 </b> "
---

*Demonstrating EC2 Auto Scaling responses and database failover mechanics.*

To validate the Solutions Architect Associate (SAA) level of system design, we must prove the architecture can survive catastrophic hardware failures.

### Simulation 1: EC2 Server Crash (Auto Scaling Healing)

1. Open the EC2 Console, select your running `smarthealthcare-api` instance, and manually click **Instance state -> Terminate instance**.
2. Wait a few moments and watch the Auto Scaling Group tab.
3. The ASG will detect that the instance is unhealthy and automatically spin up a brand-new replacement server in a healthy Availability Zone without requiring any human intervention.
4. Once the new instance boots, runs the User Data Docker script, and passes the ALB health check, the API will come back online seamlessly.

![Terminated instance with ASG launching a replacement](/images/testing/asg-replacement.png)

### Simulation 2: Database Data Center Outage (Multi-AZ Failover)

If you enabled Multi-AZ for your databases during Phase 4, you can simulate a total Availability Zone failure.

1. Open the RDS Console, select your `smarthealthcare-db`, click **Actions**, and select **Reboot**.
2. Check the box for **Reboot With Failover** and confirm.
3. AWS will simulate a critical outage on the Primary instance in AZ-A.
4. The system will automatically update the internal DNS to point to the synchronous Standby instance in AZ-B.
5. Try to refresh your application. You will experience a brief interruption (typically under 60-120 seconds).
6. Because your C# code uses retry logic (such as `AbortOnConnectFail = false` for Redis), the application will automatically reconnect to the newly promoted Primary node in AZ-B with zero data loss.