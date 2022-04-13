Mi leggo questo [blog](https://www.serverless.com/blog/aws-secrets-management/) sul sito **serverless**

3 possibili approcci

# AWS SSM parameters

Questo è **AWS Systems Manager** che è diverso da **AWS Secrets Manager** che è il secondo approccio.
Di quello il parameter store

Questi funzionano:
`aws ssm get-parameters-by-path --region us-east-1 --path "/"`
`aws ssm get-parameters --region us-east-1 --name "fabio"`
`aws ssm get-parameters --region us-east-1 --name "/fabio"`

# AWS Secrets Manager

(non provato)

# Serverless Framework secrets

(non provato)

