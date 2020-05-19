# MySQL Password Rotation Lambda Function
My personal copy of the auto-generated AWS Lambda function for rotating a MySQL password

# YAML
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Description: >-
  Conducts an AWS SecretsManager secret rotation for RDS MySQL using single user
  rotation scheme
Resources:
  SecretsManagermysqlrotationlambda:
    Type: 'AWS::Serverless::Function'
    Properties:
      Handler: lambda_function.lambda_handler
      Runtime: python3.7
      CodeUri: .
      Description: >-
        Conducts an AWS SecretsManager secret rotation for RDS MySQL using
        single user rotation scheme
      MemorySize: 128
      Timeout: 30
      Role: >-
        [redacted (see role policies)]
      VpcConfig:
        SecurityGroupIds:
          - [redacted]
        SubnetIds:
          - [redacted]
          - [redacted]
          - [redcated]
          - [redacted]
          - [redacted]
          - [redacted]
      Environment:
        Variables:
          SECRETS_MANAGER_ENDPOINT: 'https://secretsmanager.us-east-1.amazonaws.com'
      Tags:
        'serverlessrepo:semanticVersion': 1.1.37
        'lambda:createdBy': SAM
        'serverlessrepo:applicationId': >-
          [redacted]
        SecretsManagerLambda: Rotation
```

# Role Policies
AWSLambdaBasicExecutionRole
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

AWSLambdaVPCAccessExecutionRole
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "ec2:CreateNetworkInterface",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DeleteNetworkInterface"
            ],
            "Resource": "*"
        }
    ]
}
```

SecretsManagerRDSMySQLRotationSingleUserRolePolicy0 (Inline)
```json
{
    "Statement": [
        {
            "Action": [
                "ec2:CreateNetworkInterface",
                "ec2:DeleteNetworkInterface",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DetachNetworkInterface"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

SecretsManagerRDSMySQLRotationSingleUserRolePolicy1 (Inline)
```json
{
    "Statement": [
        {
            "Condition": {
                "StringEquals": {
                    "secretsmanager:resource/AllowRotationLambdaArn": "arn:aws:lambda:us-east-1:201038669559:function:SecretsManagermysql-rotation-lambda"
                }
            },
            "Action": [
                "secretsmanager:DescribeSecret",
                "secretsmanager:GetSecretValue",
                "secretsmanager:PutSecretValue",
                "secretsmanager:UpdateSecretVersionStage"
            ],
            "Resource": "arn:aws:secretsmanager:us-east-1:201038669559:secret:*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "secretsmanager:GetRandomPassword"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```