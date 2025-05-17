# lsecs and lsec2

These scripts list AWS ECS containers, EC2 servers and their IP address.

* Use ```lsecs``` to list your ECS containers
* Use ```lsec2``` to list your EC2 servers

Why is this useful? For development and debugging.
AWS ECS CLI will return the public IP address, but to find the private IP address you have to log into the web console and click down into the running tasks.
These scripts will show you the private IP addresses of running containers from inside a bastion server, saving you the extra lookup in the web console.

## Sample Output

```sh
# List ECS resources
% lsecs
>>> arn:aws:ecs:us-east-1:999999999999:cluster/YOUR-APPLICATION-cluster
Name       Status    Private IP   Task                                   Age
app-www    RUNNING   10.0.0.5     11111111-2222-3333-4444-555555555555   20 days, 19:02:30
app-cron   RUNNING   10.0.0.6     11111111-2222-3333-4444-666666666666   15 days, 12:45:00

# List EC2 resources
% lsec2
Name         Public IP     ID                    Type       AZ           Age
compute-01   10.0.10.100   i-77777777777777777   t3.large   us-east-1a   25 days, 14:36:20
pipline-01   10.0.10.101   i-88888888888888888   t3.micro   us-east-1a   66 days, 23:15:10
web-01       10.0.10.102   i-99999999999999999   t3.large   us-east-1b   45 days, 1:05:40
```

## Installation

* Python 3 is recommended (but will run with python 2.7)
* Install AWS boto3 using pip (google it for latest install instructions)
* Install prettytable for better output, using pip (pip install prettytable --user)

### IAM

* Replace _REGION_ with your AWS region, like us-east-1
* Replace _ACCOUNT_ID_ with your 12 digit AWS account ID
* (Optional) Replace the wildcards * to restrict access to certain resources

#### IAM Policy for lsecs

This is an example IAM policy that will allow you to list the ECS cluster and task details.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ecs:ListContainerInstances",
            "Resource": "arn:aws:ecs:_REGION_:_ACCOUNT_ID_:cluster/*"
        },
        {
            "Effect": "Allow",
            "Action": "ecs:DescribeTasks",
            "Resource": "arn:aws:ecs:_REGION_:_ACCOUNT_ID_:task/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecs:ListTasks",
                "ecs:ListClusters"
            ],
            "Resource": "*"
        }
    ]
}
```

#### IAM Policy for lsec2

This is an example IAM policy that will allow you to list the EC2 servers.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances"
            ],
            "Resource": "*"
        }
    ]
}
```

## Contributing

Pull requests are welcome.

## License

This software is licensed under the MIT License.
