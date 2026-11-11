# Deploy the stack

aws cloudformation deploy
  --template-file template.yml
  --stack-name employee-data-shyamkumar
  --capabilities CAPABILITY_IAM
  --profile employee-lab

# Gives Plan (Changes) and Won't execute 

aws cloudformation deploy 
  --template-file template.yml 
  --stack-name employee-data-shyamkumar
  --capabilities CAPABILITY_IAM 
  --no-execute-changeset 
  --profile employee-lab

# Execute change set
aws cloudformation execute-change-set 
  --stack-name employee-data-shyamkumar 
  --change-set-name YOUR_CHANGE_SET_NAME 
  --profile employee-lab

# Destroy the stack

$ACCOUNT = aws sts get-caller-identity --query Account --output text --profile employee-lab

## 1. Empty the bucket (raw/, curated/, scripts/, athena-results/)
aws s3 rm s3://employee-data-shyamkumar-$ACCOUNT --recursive --profile employee-lab

## 2. Delete the stack
aws cloudformation delete-stack --stack-name employee-data-shyamkumar --profile employee-lab

## 3. Wait until it is gone (returns with no output when done)
aws cloudformation wait stack-delete-complete --stack-name employee-data-shyamkumar --profile employee-lab
