Thursday,23rd November 2017

1. Installed Minishift in my local machine.
2. Read documentation for Deployment and learned about the types of deployment.
3. Started with Custom Docker image and deployed successfully.


Friday,24th November 2017

1. Read each and every parameters on Deployment YAML file.
2. Deployed a sample application using YAML file.
3. Gone through the Infrastructure components and core concepts.
4. Will look into this later

Monday,27th November 2017

Builds
  1. Source-to-image(S2I): 
  2. Pipeline(Will do later)
  3. Docker : Gone through Docker concepts,Signed up in docker and pushed and pulled images from Docker.
  4. Custom : Deployed using custom docker image.
  5. Build Input / Output :
      Build input - 3 ways mainly - 1. Docker File, 2. Git 3. Image
      Build output - ImageStream
  6. Build trigger - 3 ways
      1. Webhook
      2. Image Change
      3. Configuration Change
  7. Build Hooks : Build hooks using postCommit in different ways
      1.Shell script :
        postCommit:
            script: "bundle exec rake test --verbose"
      2. Command as the image entry point
        postCommit:
           command: ["/bin/bash", "-c", "bundle exec rake test --verbose"]
           
   8. Run Policy : 3 ways to run policy
      1. Parallel run policy
      2. Serial run policy
      3. SerialLatest run policy
      

