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
