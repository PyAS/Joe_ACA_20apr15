[![](http://www.genome.com/skin/frontend/base/default/images/genome-logo.png)](http://www.genome.com/)

# Amazon EC2
## Using Amazon Command Line Interface (CLI)


> Ref: http://docs.aws.amazon.com/cli/latest/reference/ec2/index.html

*******************

### Installation

- Download and Install `pip`
```
$ curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
$ sudo python get-pip.py
```
- Install AWS CLI using `pip`
```
sudo pip install awscli
```

### Configuration

```
aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-east-1
Default output format [None]: json
```

Item     | Description
:-------------- | :---
Default region name | This is the name of the region you want to make calls against by default. _Note: Use us-east-1 for this tutorial (the AMI we will use is specific to this region). We can change the default region later by running aws configure again._
Default output format	| This format can be either json, text, or table. If you don't specify an output format, json will be used.

##### Note
These configuration are stored into two files called `config` and `credentials` in the directory `~/.aws/`

### Launch Instances

**Run the following commands**

```
aws ec2 run-instances --count 1 --image-id 'ami-d089b0b8' --key-name 'GE-DEMO-KEY1'
--security-group-ids 'sg-62eb9206' --instance-type t2.micro --subnet-id 'subnet-2a945c5d'
--instance-initiated-shutdown-behavior stop –enable-api-termination
```

#### Description

The above command creates a t2.micro instance from the existing AMI, security group and subnets.

And sets shudown command on instance initiates 'stop' behaviour and Allow the APIs to terminate the instance.

##### Note
If we dont want to enable api termination then option –disable-api-termination is replaced
instead of –enable-api-termination

#### Output: (JSON Format)

```
{
	"OwnerId": "764231095541",
	"ReservationId": "r-8499ee6e",
	"Groups": [],
	"Instances": [
		{
			"Monitoring": {
				"State": "disabled"
			},
			"PublicDnsName": null,
			"RootDeviceType": "ebs",
			"State": {
				"Code": 0,
				"Name": "pending"
			},
			"EbsOptimized": false,
			"LaunchTime": "2015-04-13T14:08:52.000Z",
			"PrivateIpAddress": "172.30.1.189",
			"ProductCodes": [],
			"VpcId": "vpc-1fdd757a",
			"StateTransitionReason": null,
			"InstanceId": "i-7e299f83",
			"ImageId": "ami-d089b0b8",
			"PrivateDnsName": "ip-172-30-1-
			189.ec2.internal",
			"KeyName": "GE-DEMO-KEY1",
			"SecurityGroups": [
			{
				"GroupName": "launch-wizard-42",
				"GroupId": "sg-62eb9206"
			}
			],
			"ClientToken": null,
			"SubnetId": "subnet-2a945c5d",
			"InstanceType": "t2.micro",
			"NetworkInterfaces": [
			{
			"Status": "in-use",
			"MacAddress": "0a:22:49:e4:55:db",
			"SourceDestCheck": true,
			"VpcId": "vpc-1fdd757a",
			"Description": null,
			"NetworkInterfaceId": "eni-6b953e23",
			"PrivateIpAddresses": [
				{
					"Primary": true,
					"PrivateIpAddress": "172.30.1.189"
				}
			],
			"Attachment": {
				"Status": "attaching",
				"DeviceIndex": 0,
				"DeleteOnTermination": true,
				"AttachmentId": "eni-attach-3632c953",
				"AttachTime": "2015-04-13T14:08:52.000Z"
			},
			"Groups": [
				{
					"GroupName": "launch-wizard-42",
					"GroupId": "sg-62eb9206"
				}
			],
			"SubnetId": "subnet-2a945c5d",
			"OwnerId": "764231095541",
			"PrivateIpAddress": "172.30.1.189"
			}
		],
		"SourceDestCheck": true,
		"Placement": {
			"Tenancy": "default",
			"GroupName": null,
			"AvailabilityZone": "us-east-1b"
		},
		"Hypervisor": "xen",
		"BlockDeviceMappings": [],
		"Architecture": "x86_64",
		"StateReason": {
			"Message": "pending",
			"Code": "pending"
		},
		"RootDeviceName": "/dev/sda1",
		"VirtualizationType": "hvm",
		"AmiLaunchIndex": 0
		}
	]
}
```

### Stop Instances

```
aws ec2 stop-instances --instance-id i-dcb30521
```

#### Output: (JSON Format)
```
{
	"StoppingInstances": [
	{
		"InstanceId": "i-7e299f83",
		"CurrentState": {
			"Code": 64,
			"Name": "stopping"
		},
		"PreviousState": {
			"Code": 16,
			"Name": "running"
			}
		}
	]
}
```
### Instance State Code

Code	|	Description
:--------|-----------------
0 | Pending
16 | running
32 | shutting down
48 | terminated
64 | stopping
80 | stopped

### Start stopped Instances

```
aws ec2 start-instances --instance-ids i-dcb30521
```

### Terminate Instances
```
aws ec2 terminate-instances --instance-ids i-dcb30521
```

### Describe Instances
```
aws ec2 describe-instances
```
Inspite of the instance states stopped or running, the above command shows details of all the instances.

```
aws ec2 describe-instances --instance-id i-dcb30521
```

The above command shows details of particular instance with instance-id
```
'i-dcb30521'
```


### Create Keypair

```
aws ec2 create-key-pair --key-name MyNewKey --query 'KeyMaterial' --output text > MyNewKey.pem
```

Output:

```
MyNewKey.pem
```

### Create a Security Group
```
aws ec2 create-security-group --group-name MyNewSG-sg
```

#### Output: JSON format
```
{
"GroupId": "sg-b018ced5"
}
```

### Add Rules to Security Group
```
aws ec2 authorize-security-group-ingress --group-name MyNewSG-sg –protocol tcp --port 22 --cidr 0.0.0.0/0
```

#### Description
The above command creates rules to access `TCP port 22` from anywhere.
