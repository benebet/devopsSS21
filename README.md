# Concept for DevOps lecture
#### By Benedikt Linus Betz

##### Concept idea
- Code is hosted in a Github repository
- CI is done with Github CI workflow after pull request / CD is triggered manually from master branch
- Packer is used to create an image with installed dependencies and repository code
- Terraform sets up AWS environment with all resources and created packer image
- As soon as an instance is online a script is executed automatically that connects to the database and starts the app
- MongoDB is used in an atlas cluster to make it always available

![Flow of Concept](https://i.ibb.co/74zY7j7/Screenshot-2021-09-10-at-21-58-25.png) Image of the CI / CD flow of the concept

###### Detailed breakup
1. A pull request launches the CI workflow which tests the repository code before the branch can be merged
2. If all tests pass, the merge request can be approved
3. The CD can now be triggered manually inside Github actions
4. The workflow builds a packer image that is beating provisioned with a script that installs all necessary dependencies and clones the master branch
5. AWS credentials are stored in Github secrets and the packer image is uploaded to AWS as an AMI
6. Terraform files in the repository define the resources that terraform will create (elb, autoscaling, security groups, etc.)
7. Terraform plan, apply (instances are created)
8. Terraform supplies a shell script in user_data that is executed when the instances come online, the script builds the server, connects to the MongoDB atlas cluster and runs the app
9. Monitoring of the instances is done in AWS, MongoDB monitoring in Atlas cluster

##### Unfinished points
- Issues with shell script in user_data, is it not launched properly? (See startup_todoapp_service.sh)
- Created env variables on the instances have an undefined value in node (used for dbUser, pw, etc.) -> Cannot connect to MongoDB
- MongoDB username and password is not stored safely as secret yet
- No FQDN implemented
- No SSL implemented
