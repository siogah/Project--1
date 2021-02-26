# Project--1
provider "aws" {
    access_key = "AKIAJKENLFRTHCZPBF7A"
    secret_key = "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"
    region  = "us-east-1"
}

#This is a single-line comment.
resource "aws_instance" "myserver" {
  ami           = " ami-03d315ad33b9d49c4"
  instance_type = "t2.meduim"
  key_name = "aws_key_pair.keypair.key_name"
  vpc_security_group_ids = "aws_security_group.allow_ports.id"

  tags = {
      Name = "27global"
  }

}
resource "aws_key_pair" "keypair" {
  key_name   = "mykey"
  public_key = "2048 37 22481117352066932449086198025274745001147659569633735844154957091424500295653415
  7944386312050803805151987509187269804345274657339223464787897877922617055807267847712312156793722076277
  059561879221153885222077014495748948310216248171481976210572671821370503515610880668603823884726510033673
  6524974123527704252272325934162891431204992245525476266372292606916565641908022437626585892842815624458811
  71060749392810486774809654243271487232570649719455540766382193721936146304704664305483093136586463545118081527
  34060201081857920034686517355716995764136709642809337066122628067038076542412899073218893073390545948692349221031 rsa-key-20210225"
}

resource "aws_eip" "myeip" {
  instance = aws_instance.myserver.id
  vpc      = true
}

resource "aws_default_vpc"  "default" {
    tags = {
        Name = "Default VPC"
    }
}

resource "aws_security_group" "allow_ports" {
  name        = "allow_ports"
  description = "Allow  inbound traffic"
  vpc_id      = "aws_default_vpc.default.id"

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTPS"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "allow_ports"
  }
}
