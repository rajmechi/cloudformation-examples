**Fn::FindInMap**
The intrinsic function Fn::FindInMap returns the value corresponding to keys in a two-level map that is declared in the Mappings section.

!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]

```
Mappings: 
  RegionMap: 
    us-east-1: 
      HVM64: "ami-0ff8a91507f77f867"
      HVMG2: "ami-0a584ac55a7631c0c"
    us-west-1: 
      HVM64: "ami-0bdb828fd58c52235"
      HVMG2: "ami-066ee5fd4a9ef77f1"
    eu-west-1: 
      HVM64: "ami-047bb4163c506cd98"
      HVMG2: "ami-31c2f645"
    ap-southeast-1: 
      HVM64: "ami-08569b978cc4dfa10"
      HVMG2: "ami-0be9df32ae9f92309"
    ap-northeast-1: 
      HVM64: "ami-06cd52961ce9f0d85"
      HVMG2: "ami-053cdd503598e4a9d"
Resources: 
  myEC2Instance: 
    Type: "AWS::EC2::Instance"
    Properties: 
      ImageId: !FindInMap
        - RegionMap      
        - !Ref 'AWS::Region'
        - HVM64
      InstanceType: m1.small
```

**Fn::GetAtt**
The Fn::GetAtt intrinsic function returns the value of an attribute from a resource in the template. For more information about GetAtt return values for a particular resource, refer to the documentation for that resource in the Resource and property reference. https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html

!GetAtt logicalNameOfResource.attributeName

```
AWSTemplateFormatVersion: 2010-09-09
Resources:
  myELB:
    Type: AWS::ElasticLoadBalancing::LoadBalancer
    Properties:
      AvailabilityZones:
        - eu-west-1a
      Listeners:
        - LoadBalancerPort: '80'
          InstancePort: '80'
          Protocol: HTTP
  myELBIngressGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: ELB ingress group
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          SourceSecurityGroupOwnerId: !GetAtt myELB.SourceSecurityGroup.OwnerAlias
          SourceSecurityGroupName:
            Fn::GetAtt:
            - myELB
            - SourceSecurityGroup.GroupName
```


**Fn::GetAZs**

The intrinsic function Fn::GetAZs returns an array that lists Availability Zones for a specified region in alaphabetical order. 

!GetAZs region

```
mySubnet: 
  Type: "AWS::EC2::Subnet"
  Properties: 
    VpcId: 
      !Ref VPC
    CidrBlock: 10.0.0.0/24
    AvailabilityZone: 
      Fn::Select: 
        - 0
        - Fn::GetAZs: ""
```
```
AvailabilityZone: !Select 
  - 0
  - !GetAZs 
    Ref: 'AWS::Region'
```
```
AvailabilityZone: !Select 
  - 0
  - Fn::GetAZs: !Ref 'AWS::Region'
```


**Fn::ImportValue**

The intrinsic function Fn::ImportValue returns the value of an output exported by another stack.

```
"Outputs" : {
  "PublicSubnet" : {
    "Description" : "The subnet ID to use for public web servers",
    "Value" :  { "Ref" : "PublicSubnet" },
    "Export" : { "Name" : {"Fn::Sub": "${AWS::StackName}-SubnetID" }}
  },
  "WebServerSecurityGroup" : {
    "Description" : "The security group ID to use for public web servers",
    "Value" :  { "Fn::GetAtt" : ["WebServerSecurityGroup", "GroupId"] },
    "Export" : { "Name" : {"Fn::Sub": "${AWS::StackName}-SecurityGroupID" }}
  }
}
```
```
"Resources" : {
  "WebServerInstance" : {
    "Type" : "AWS::EC2::Instance",
    "Properties" : {
      "InstanceType" : "t2.micro",
      "ImageId" : "ami-a1b23456",
      "NetworkInterfaces" : [{
        "GroupSet" : [{"Fn::ImportValue" : {"Fn::Sub" : "${NetworkStackNameParameter}-SecurityGroupID"}}],
        "AssociatePublicIpAddress" : "true",
        "DeviceIndex" : "0",
        "DeleteOnTermination" : "true",
        "SubnetId" : {"Fn::ImportValue" : {"Fn::Sub" : "${NetworkStackNameParameter}-SubnetID"}}
      }]
    }
  }
}
```

You can't use the short form of !ImportValue when it contains a !Sub. The following example is valid for AWS CloudFormation, but not valid for YAML:

**Fn::Join**
The intrinsic function Fn::Join appends a set of values into a single value, separated by the specified delimiter. If a delimiter is the empty string, the set of values are concatenated with no delimiter.

```
!Join
  - ''
  - - 'arn:'
    - !Ref AWS::Partition
    - ':s3:::elasticbeanstalk-*-'
    - !Ref 'AWS::AccountId'
```
For the Fn::Join list of values, you can use the following functions:

Fn::Base64

Fn::FindInMap

Fn::GetAtt

Fn::GetAZs

Fn::If

Fn::ImportValue

Fn::Join

Fn::Split

Fn::Select

Fn::Sub

Ref


**Fn::Select**
The intrinsic function Fn::Select returns a single object from a list of objects by index.





**Fn::Sub**

Variables can be template parameter names, resource logical IDs, resource attributes, or a variable in a key-value map. 









