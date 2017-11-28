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
      
   9. Image Streams 
    
       1. Gone through Docker Registry, Docker Image, Docker Image Tag, Docker Image ID.
       2. Gone through Image Stream Images, Image Stream Tags, Image Stream Change Triggers, Image Stream Tags, Image Stream             Mappings.
       3. Gone through Configuring and Working With Image Streams.
       
  10. Deployments
  
        1. Deployment Configuration
        2. Deployment Triggers
        3. Rolling Deployment Strategy : The Rolling strategy is the default strategy used if no strategy is specified on a                                            deployment configuration.
        
                                          "strategy": {
                                                        "type": "Rolling",
                                                        "rollingParams": {
                                                          "timeoutSeconds": 120, 
                                                          "maxSurge": "20%", 
                                                          "maxUnavailable": "10%" 
                                                          "pre": {}, 
                                                          "post": {}
                                                        }
                                                      }
           
                                                      
The Rolling Strategy will :

Execute any pre-lifecycle hooks.

Scale up the new deployment based on the surge configuration.

Scale down the old deployment based on the max unavailable configuration.

Repeat this scaling until the new deployment has reached the desired replica count and the old deployment has been scaled to zero.

Execute any post lifecycle hook.
        4. Cannary Deployment Strategy
        5. Recreate Deployment Strategy :
                              "strategy": {
                                "type": "Recreate",
                                "recreateParams": { 
                                  "pre": {}, 
                                  "post": {}
                                }
                              }
        6. Blue-Green Deployent Strategy
        7. A/B Deployment strategy
        8. Lifecycle Hooks :
            The Recreate and Rolling strategies support lifecycle hooks, which allow behavior to be injected into the                     deployment process at predefined points within the strategy:

          The following is an example of a "pre" lifecycle hook:

            "pre": {
              "failurePolicy": "Abort",
              "execNewPod": {} 
            }
            execNewPod is a pod-based lifecycle hook
            failurePolicy has the following types :
              a. Abort - The deployment should be considered a failure if the hook fails
              b. Retry - The hook execution should be retried until it succeeds.
              c. Ignore - Any hook failure should be ignored and the deployment should proceed.
              
  11. 4. Health checks
           1. Liveness Probe : A liveness probe checks if the container in which it is configured is still running. If the                                    liveness probe fails,the kubelet kills the container, which will be subjected to its restart                                  policy. Set a liveness check by configuring the template.spec.containers.livenessprobe stanza                                  of a pod configuration.
           2. Readiness Probe : A readiness probe determines if a container is ready to service requests. If the readiness                                     probe fails a container,the endpoints controller ensures the container has its IP address                                     removed from the endpoints of all services. A readiness probe can be used to signal to the                                     endpoints controller that even though a container is running, it should not receive any                                       traffic from a proxy. Set a readiness check by configuring the                                                                 template.spec.containers.readinessprobe stanza of a pod configuration.
           Both probes can be configured in three ways :
                    1. HTTP checks
                            readinessProbe:
                                      httpGet:
                                        path: /healthz
                                        port: 8080
                                      initialDelaySeconds: 15
                                      timeoutSeconds: 1
                     2. Container Execution Checks :
                            ...
                                  livenessProbe:
                                    exec:
                                      command:
                                      - cat
                                      - /tmp/health
                                    initialDelaySeconds: 15
                                    timeoutSeconds: 1
                      3. TCP Socket Checks :
                            ...
                                    livenessProbe:
                                      tcpSocket:
                                        port: 8080
                                      initialDelaySeconds: 15
                                      timeoutSeconds: 1
                                    ...
                          
                   
  
          
  
  
  
  
  
  
  
