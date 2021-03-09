
**Template Anatomy:**

- AWSTemplateFormatVersion
- Description
- Metadata 
- Parameters
- Mappings
- Conditions
- Transform
- Resources
- Outputs

**Intrinsic function:**

- Fn::Base64
- Fn::Cidr
- Condition functions
- Fn::FindInMap
- Fn::GetAtt
- Fn::GetAZs
- Fn::ImportValue
- Fn::Join
- Fn::Select
- Fn::Split
- Fn::Sub
- Fn::Transform
- Ref

**Condition functions:**

- Fn::And
- Fn::Equals
- Fn::If
- Fn::Not
- Fn::Or

**Pseudo parameters**  

Pseudo parameters are parameters that are predefined by AWS CloudFormation. You do not declare them in your template. Use them the same way as you would a parameter, as the argument for the Ref function

- AWS::AccountId
- AWS::NotificationARNs
- AWS::NoValue
- AWS::Partition
- AWS::Region
- AWS::StackId
- AWS::StackName
- AWS::URLSuffix



