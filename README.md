# lsecs

This script lists AWS ECS containers and their IP address.

Why is this useful? For development and debugging.
AWS ECS CLI will return the public IP address, but to find the private IP address you have to log into the web console and click down into the running tasks.
This script will show you the private IP addresses of running containers from inside a bastion server, saving you the extra lookup in the web console.

## Sample Output

```sh
% lsecs
>>> arn:aws:ecs:us-east-1:999999999999:cluster/YOUR-APPLICATION-cluster
Name       Status    Private IP   Task
app-www    RUNNING   10.0.0.5     11111111-2222-3333-4444-555555555555
app-cron   RUNNING   10.0.0.6     11111111-2222-3333-4444-666666666666
```

## Installation

* This script requires python. It will probably run with python 2.7, although python 3 is recommended.
* Install AWS boto3 using pip (google it for latest install instructions)
* Install tabulate for better output, using pip (pip install tabulate --user)

### IAM Policy

This is an example IAM policy that will allow you to list the ECS cluster and task details.

* Replace _REGION_ with your AWS region, like us-east-1
* Replace _ACCOUNT_ID_ with your 12 digit AWS account ID
* (Optional) Replace the wildcards * to restrict access to certain resources

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

## Contributing

Pull requests are welcome.

## License

This software is licensed under the MIT License.
