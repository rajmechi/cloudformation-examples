**Parameters secrtion:**
```
"ParamAllowSSHAccess": {
    "Type": "String",
    "AllowedValues": ["True", "False"],
    "Default":  "True",
    "ConstraintDescription": "chosse true or false for ssh access"
}
```

```
ParamAllowSSHAccess:
  Type: String
  AllowedValues:
  - 'True'
  - 'False'
  Default: 'True'
  ConstraintDescription: chosse true or false for ssh access
  ```

**Conditions Section:**
```
"Conditions": {
    "AllowSSHAccess": {
       "Fn:Equals": [ { "Ref": "ParamAllowSSHAccess"}, "true"]
    }
}
```

```
Conditions:
  AllowSSHAccess:
    Fn:Equals:
    - Ref: ParamAllowSSHAccess
    - 'true'
```

**Resource section:**
```
"SecurityGroup": {
     "Type": "AWS::EC2::SecurityGroup",
     "Condition": "AllowSSHAccess",
     "blabla": "blabla"
}
```

```
SecurityGroup:
  Type: AWS::EC2::SecurityGroup
  Condition: AllowSSHAccess
  blabla: blabla
```

- Condition for resource property:
```
 "SecurityGroupIngress": [
    {
        "IpProtocol": "tcp",
        "FormPort": "80",
        "ToPort": "80",
        "CidrIp": "0.0.0.0/0"
    },
    {
        "Fn:If": [
            "AllowSSHAccess",
              {
                 "IpProtocol": "tcp",
                 "FormPort": "22",
                 "ToPort": "22",
                 "CidrIp": { "Ref": "ParamAllowSSHFromRange" }
              },
              { 
                 "Ref": "AWS::NoValue" 
              }
        ]
    }
]
```

```
SecurityGroupIngress:
- IpProtocol: tcp
  FormPort: '80'
  ToPort: '80'
  CidrIp: 0.0.0.0/0
- Fn:If:
  - AllowSSHAccess
  - IpProtocol: tcp
    FormPort: '22'
    ToPort: '22'
    CidrIp:
      Ref: ParamAllowSSHFromRange
  - Ref: AWS::NoValue
```

```
"NewVolume" : {
  "Type" : "AWS::EC2::Volume",
  "Properties" : {
    "Size" : {
      "Fn::If" : [
        "CreateLargeSize",
        "100",
        "10"
      ]},
    "AvailabilityZone" : { "Fn::GetAtt" : [ "Ec2Instance", "AvailabilityZone" ]}
  },
  "DeletionPolicy" : "Snapshot"
}
```


```
NewVolume:
  Type: AWS::EC2::Volume
  Properties:
    Size:
      Fn::If:
      - CreateLargeSize
      - '100'
      - '10'
    AvailabilityZone:
      Fn::GetAtt:
      - Ec2Instance
      - AvailabilityZone
  DeletionPolicy: Snapshot
```

```
NewVolume:
  Type: "AWS::EC2::Volume"
  Properties: 
    Size: 
      !If [CreateLargeSize, 100, 10]
    AvailabilityZone: !GetAtt: Ec2Instance.AvailabilityZone
  DeletionPolicy: Snapshot
```


```
MyNotCondition:
  Fn::Not:
  - Fn::Equals:
    - Ref: EnvironmentType
    - prod
```


```
"Conditions": {
   "UseDBSnapshot": {"Fn::Not":  [{"Fn::Equals": [{"Ref": "DBSnapshotName"}, "" ]}]}
}
```

```
"DBSnapshotIdentifier": {
    "Fn:If" : [
        "UseDBSnapshot",
        {"Ref" : "DBSnapshotName"},
        {"Ref": "AWS::NoValue"}
    ]
}
```
```
DBSnapshotIdentifier:
  Fn:If:
  - UseDBSnapshot
  - Ref: DBSnapshotName
  - Ref: AWS::NoValue
```

```
"DBSnapshotIdentifier": !If [UseDBSnapshot, !Ref DBSnapshotName, !Ref AWS::NoValue" ]
```