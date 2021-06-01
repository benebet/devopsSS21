# Concept for DevOps lecture
#### By Benedikt Linus Betz

##### Concept idea
- Code is hosted in a Github repository
- CI is done with Github CI workflow after pull request
- Terraform is used to provision a AWS instance for CD

![Flow of Concept](https://i.ibb.co/3fDRjxK/Screenshot-2021-05-31-at-14-10-29.png) Image of the CI / CD flow of the concept

###### Detailed breakup
1. A change in the repository (e.g. commit / pull request) is monitored by Github and triggers the CI
2. A Github workflow is executed, which will start the CI testing with Github
3. If all tests pass, the merge request can be approved. This has to be done manually, since it will trigger the CD
4. As soon as the master branch is updated a Github action is triggered
5. Terraform Cloud is connected to Github and the action establishes a connection to Terraform Cloud
6. Terraform Cloud manages the provisioning of AWS instances and has the AWS credentials stored
7. A new AWS instance with installed prerequisites (npm installations) is created with Terraform
8. The code from the repositories master branch is deployed on the instance
9. Terraform cloud manages the IPs and configuration for the instances
10. (Manage the AWS instances for scaling, redundancy)

##### Questions / Obscurities
- How is Github testing implemented? Testing on Github provided machines (2.000 free minutes a month) or a self managed machine? What is recommended / required?
- How do you manage the scaling of the deployed instances? Directly in the AWS console or in Terraform Cloud?
- Is this concept meeting all requirements? Is it realistic?
- What about load balancing when scaling?
- Does it meet this requirement? What exactly does it mean?
> At least one service (e.g. VCS, Monitoring) other than the application has to be provisioned by yourself (no third-party as-a-service solution)
