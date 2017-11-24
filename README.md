# Vagrant AWS Provider

This is a [Vagrant](http://www.vagrantup.com) 1.2+ plugin that adds an [AWS](http://aws.amazon.com)
provider to Vagrant, allowing Vagrant to control and provision machines in
EC2 and VPC.

**NOTE:** This plugin requires Vagrant 1.2+,

## Features

* Boot EC2 or VPC instances.
* SSH into the instances.
* Provision the instances with any built-in Vagrant provisioner.
* Define region-specific configurations so Vagrant can manage machines
  in multiple regions.
* Package running instances into new vagrant-aws friendly boxes
* Create shell script to spin up ELMSLN instance

## Usage

Install using standard Vagrant 1.1+ plugin installation methods. After
installing, `vagrant up` and specify the `aws` provider. An example is
shown below.

```

$ mkdir VagrantELMS && cd VagrantELMS
$ sudo vagrant plugin install vagrant-aws
$ vagrant box add dummy https://github.com/dfusco/vagrant-aws/raw/master/dummy.box
$ vagrant init
...
Visit https://jsfiddle.net/djfusco/6a3o3788/2/ and download .sh script
$ ./elmsinstall.sh
...
```

Of course prior to doing this, you'll need to obtain an AWS-compatible
box file for Vagrant.

## Quick Start

1. Install Vagrant from https://www.vagrantup.com/

2. Create a dummy AWS box and specify all the details manually within a `config.vm.provider` block. So first, add the dummy
box using any name you want:

```
$ vagrant box add dummy https://github.com/dfusco/Vagrant-AWS/raw/master/dummy.box
...
```

3. Edit your Vagrantfile that looks like the following, filling in
your information where necessary.  Place this in VagrantELMS directory.

```
Vagrant.configure("2") do |config|
    config.vm.box = "dummy"
    config.vm.provider :aws do |aws, override|
      aws.access_key_id = "YOUR KEY"
      aws.secret_access_key = "YOUR SECRET KEY"
      aws.keypair_name = "YOUR KEYPAIR NAME"
      aws.instance_type = "INSTANCE LEVEL"
      aws.region = "AWS REGION"
      aws.ami = "INSTANCE IMAGE"
      aws.security_groups = ["INSTANCE SECURITY GROUP"]
      override.ssh.username = "ubuntu"
      override.ssh.private_key_path = "PATH TO YOUR PRIVATE KEY"
    end
end
```


AWS Infomrmation

Access Keys
http://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys

Key Pairs
http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html

EC2 Instance Types
https://aws.amazon.com/ec2/instance-types/

Regions and Endpoints
http://docs.aws.amazon.com/general/latest/gr/rande.html

AMI
http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html

Security Groups
http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html


4. Visit https://jsfiddle.net/djfusco/6a3o3788/2/ and download .sh script to your Vagrant directory

5. Run the shell script you just downloaded

```
$ bash elmsinstall.sh
```

NOTE:
1. You will be asked for a password during the script run:
"Enter new UNIX password: Retype new UNIX password: passwd: password updated successfully
Password:"
--> Enter 'password'
(Don't worry, we'll change this later)

2. IT IS EXTREMELY IMPORTANT THAT YOU COPY THE RANDOM PASSWORD GENERATED FOR YOU DURING THIS SCRIPT INSTALL.  IT WILL BE AT THE END IN GREEEN, ALONG WITH THE DNS INFORMATION NEEDED LATER.

3. At the end of the script, you will be asked to reset the root password.
--> make it a good one!


This will start an AWS instance in the region you've chosen within
your account.  It will then SSH into your instance and install ELMSLN.

Note that normally a lot of this boilerplate is encoded within the box
file, but the box file used for the quick start, the "dummy" box, has
no preconfigured defaults.

If you have issues with SSH connecting, make sure that the instances
are being launched with a security group that allows SSH access.

6. To change the state of your instance:

SSH into your instance
```
$ vagrant ssh
```

Stop your instance
```
$ vagrant halt
```

Stops and deletes all traces of the Vagrant machine
```
$ vagrant destroy
```

## DNS

1. Obtain your IPv4 Public IP from your AWS instance in the
        "EC2 Management Console"
http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html

2. Add this and your ELMS install host information to your
        '/etc/hosts' file (or wherever your hosts file is at)

Example:
34.216.58.24       studio.drdavebuild.com
34.216.58.24       inbox.drdavebuild.com
34.216.58.24       lq.drdavebuild.com
34.216.58.24       blog.drdavebuild.com
34.216.58.24       innovate.drdavebuild.com
34.216.58.24       people.drdavebuild.com
34.216.58.24       hub.drdavebuild.com
34.216.58.24       interact.drdavebuild.com
34.216.58.24       comply.drdavebuild.com
34.216.58.24       media.drdavebuild.com
34.216.58.24       lor.drdavebuild.com
34.216.58.24       grades.drdavebuild.com
34.216.58.24       discuss.drdavebuild.com
34.216.58.24       online.drdavebuild.com
34.216.58.24       courses.drdavebuild.com

34.216.58.24       data-studio.drdavebuild.com
34.216.58.24       data-inbox.drdavebuild.com
34.216.58.24       data-lq.drdavebuild.com
34.216.58.24       data-blog.drdavebuild.com
34.216.58.24       data-innovate.drdavebuild.com
34.216.58.24       data-people.drdavebuild.com
34.216.58.24       data-hub.drdavebuild.com
34.216.58.24       data-interact.drdavebuild.com
34.216.58.24       data-comply.drdavebuild.com
34.216.58.24       data-media.drdavebuild.com
34.216.58.24       data-lor.drdavebuild.com
34.216.58.24       data-grades.drdavebuild.com
34.216.58.24       data-discuss.drdavebuild.com
34.216.58.24       data-online.drdavebuild.com
34.216.58.24       data-courses.drdavebuild.com


## ELMSLN Login

1. You can now to to your live ELMS install at 'online.domain.com'

Example: http://online.drdavebuild.com


2. Enter your login credentials
        Username: 'admin'
        Password: 'RANDOMLY GENERATED PASSWORD FROM SCRIPT'




## Configuration

This provider exposes quite a few provider-specific configuration options:

* `access_key_id` - The access key for accessing AWS
* `ami` - The AMI id to boot, such as "ami-12345678"
* `availability_zone` - The availability zone within the region to launch
  the instance. If nil, it will use the default set by Amazon.
* `aws_profile` - AWS profile in your config files. Defaults to *default*.
* `aws_dir` - AWS config and credentials location. Defaults to *$HOME/.aws/*.
* `instance_ready_timeout` - The number of seconds to wait for the instance
  to become "ready" in AWS. Defaults to 120 seconds.
* `instance_check_interval` - The number of seconds to wait to check the instance's
 state
* `instance_package_timeout` - The number of seconds to wait for the instance
  to be burnt into an AMI during packaging. Defaults to 600 seconds.
* `instance_type` - The type of instance, such as "m3.medium". The default
  value of this if not specified is "m3.medium".  "m1.small" has been
  deprecated in "us-east-1" and "m3.medium" is the smallest instance
  type to support both paravirtualization and hvm AMIs
* `keypair_name` - The name of the keypair to use to bootstrap AMIs
   which support it.
* `monitoring` - Set to "true" to enable detailed monitoring.
* `session_token` - The session token provided by STS
* `private_ip_address` - The private IP address to assign to an instance
  within a [VPC](http://aws.amazon.com/vpc/)
* `elastic_ip` - Can be set to 'true', or to an existing Elastic IP address.
  If true, allocate a new Elastic IP address to the instance. If set
  to an existing Elastic IP address, assign the address to the instance.
* `region` - The region to start the instance in, such as "us-east-1"
* `secret_access_key` - The secret access key for accessing AWS
* `security_groups` - An array of security groups for the instance. If this
  instance will be launched in VPC, this must be a list of security group
  Name. For a nondefault VPC, you must use security group IDs instead (http://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html).
* `iam_instance_profile_arn` - The Amazon resource name (ARN) of the IAM Instance
    Profile to associate with the instance
* `iam_instance_profile_name` - The name of the IAM Instance Profile to associate
  with the instance
* `subnet_id` - The subnet to boot the instance into, for VPC.
* `associate_public_ip` - If true, will associate a public IP address to an instance in a VPC.
* `ssh_host_attribute` - If `:public_ip_address`, `:dns_name`, or
  `:private_ip_address`, will use the public IP address, DNS name, or private
  IP address, respectively, to SSH to the instance. By default Vagrant uses the
  first of these (in this order) that is known. However, this can lead to
  connection issues if, e.g., you are assigning a public IP address but your
  security groups prevent public SSH access and require you to SSH in via the
  private IP address; specify `:private_ip_address` in this case.
* `tenancy` - When running in a VPC configure the tenancy of the instance.  Supports 'default' and 'dedicated'.
* `tags` - A hash of tags to set on the machine.
* `package_tags` - A hash of tags to set on the ami generated during the package operation.
* `use_iam_profile` - If true, will use [IAM profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/instance-profiles.html)
  for credentials.
* `block_device_mapping` - Amazon EC2 Block Device Mapping Property
* `elb` - The ELB name to attach to the instance.
* `unregister_elb_from_az` - Removes the ELB from the AZ on removal of the last instance if true (default). In non default VPC this has to be false.
* `terminate_on_shutdown` - Indicates whether an instance stops or terminates
  when you initiate shutdown from the instance.
* `endpoint` - The endpoint URL for connecting to AWS (or an AWS-like service). Only required for non AWS clouds, such as [eucalyptus](https://github.com/eucalyptus/eucalyptus/wiki).

These can be set like typical provider-specific configuration:

```ruby
Vagrant.configure("2") do |config|
  # ... other stuff

  config.vm.provider :aws do |aws|
    aws.access_key_id = "foo"
    aws.secret_access_key = "bar"
  end
end
```

Note that you do not have to hard code your `aws.access_key_id` or `aws.secret_access_key`
as they will be retrieved from the enviornment variables `AWS_ACCESS_KEY` and `AWS_SECRET_KEY`.

In addition to the above top-level configs, you can use the `region_config`
method to specify region-specific overrides within your Vagrantfile. Note
that the top-level `region` config must always be specified to choose which
region you want to actually use, however. This looks like this:

```ruby
Vagrant.configure("2") do |config|
  # ... other stuff

  config.vm.provider :aws do |aws|
    aws.access_key_id = "foo"
    aws.secret_access_key = "bar"
    aws.region = "us-east-1"

    # Simple region config
    aws.region_config "us-east-1", :ami => "ami-12345678"

    # More comprehensive region config
    aws.region_config "us-west-2" do |region|
      region.ami = "ami-87654321"
      region.keypair_name = "company-west"
    end
  end
end
```

The region-specific configurations will override the top-level
configurations when that region is used. They otherwise inherit
the top-level configurations, as you would probably expect.



## Other Examples

### Tags

To use tags, simply define a hash of key/value for the tags you want to associate to your instance, like:

```ruby
Vagrant.configure("2") do |config|
  # ... other stuff

  config.vm.provider "aws" do |aws|
    aws.tags = {
	  'Name' => 'Some Name',
	  'Some Key' => 'Some Value'
    }
  end
end
```

### User data

You can specify user data for the instance being booted.

```ruby
Vagrant.configure("2") do |config|
  # ... other stuff

  config.vm.provider "aws" do |aws|
    # Option 1: a single string
    aws.user_data = "#!/bin/bash\necho 'got user data' > /tmp/user_data.log\necho"

    # Option 2: use a file
    aws.user_data = File.read("user_data.txt")
  end
end
```

### Disk size

Need more space on your instance disk? Increase the disk size.

```ruby
Vagrant.configure("2") do |config|
  # ... other stuff

  config.vm.provider "aws" do |aws|
    aws.block_device_mapping = [{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 50 }]
  end
end
```

### ELB (Elastic Load Balancers)

You can automatically attach an instance to an ELB during boot and detach on destroy.

```ruby
Vagrant.configure("2") do |config|
  # ... other stuff

  config.vm.provider "aws" do |aws|
    aws.elb = "production-web"
  end
end
```

## Development

To work on the `vagrant-aws` plugin, clone this repository out, and use
[Bundler](http://gembundler.com) to get the dependencies:

```
$ bundle
```

Once you have the dependencies, verify the unit tests pass with `rake`:

```
$ bundle exec rake
```

If those pass, you're ready to start developing the plugin. You can test
the plugin without installing it into your Vagrant environment by just
creating a `Vagrantfile` in the top level of this directory (it is gitignored)
and add the following line to your `Vagrantfile`
```ruby
Vagrant.require_plugin "vagrant-aws"
```
Use bundler to execute Vagrant:
```
$ bundle exec vagrant up --provider=aws
```
