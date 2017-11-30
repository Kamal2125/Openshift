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
  12. Services
  
      1. Services :
            A Kubernetes service serves as an internal load balancer. It identifies a set of replicated
pods in order to proxy the connections it receives to them. Backing pods can be added to
or removed from a service arbitrarily while the service remains consistently available, enabling
anything that depends on the service to refer to it at a consistent address. The default
service clusterIP addresses are from the OpenShift Origin internal network and they are
used to permit pods to access each other.
To permit external access to the service, additional externalIP and ingressIP addresses that
are external to the cluster can be assigned to the service. These externalIP addresses can
also be virtual IP addresses that provide highly available access to the service.
     2. Service externalIPs : 
     The user can configure IP addresses that are external to the cluster.
The externalIPs must be selected by the cluster adminitrators from the ExternalIPNetworkCIDRs
range configured in master-config.yaml file. When master-config.yaml is changed, the
master service must be restarted.

# Sample ExternalIPNetworkCIDR /etc/origin/master/master-config.yaml
networkConfig:
  ExternalIPNetworkCIDR: 172.47.0.0/24
  
  # Service externalIPs Definition (JSON)
{
    "kind": "Service",
    "apiVersion": "v1",
    "metadata": {
        "name": "my-service"
    },
    "spec": {
        "selector": {
            "app": "MyApp"
        },
        "ports": [
            {
                "name": "http",
                "protocol": "TCP",
                "port": 80,
                "targetPort": 9376
            }
        ],
        "externalIPs" : [
            "80.11.12.10"        
        ]
    }
}

"80.11.12.10"           List of External IP addresses on which the port is exposed. In addition to
the internal IP addresses)
  3. Service IngressIPs : 
    In non-cloud clusters, externalIP addresses can be automatically assigned from a pool of addresses.
The pool is configured in /etc/origin/master/master-config.yaml file. After changing this file,
restart the master service.
The ingressIPNetworkCIDR is set to 172.29.0.0/16 by default. If the cluster environment is
not already using this private range, use the default range or set a custom range.

  If you are using high availability, then this range must be less than 256 addresses.

  # Sample ingressIPNetworkCIDR /etc/origin/master/master-config.yaml
networkConfig:
  ingressIPNetworkCIDR: 172.29.0.0/16

  4. Service Nodeport : 
  
    Setting the service type=NodePort will allocate a port from a flag-configured range (default:
30000-32767), and each node will proxy that port (the same port number on every node)
into your service.
The selected port will be reported in the service configuration, under
spec.ports[*].nodePort.
To specify a custom port just place the port number in the nodePort field. The custom
port number must be in the configured range for nodePorts. When 'master-config.yaml' is
changed the master service must be restarted

        # Sample servicesNodePortRange /etc/origin/master/master-config.yaml
kubernetesMasterConfig:

  servicesNodePortRange: ""
  
  The service will be visible as both the <NodeIP>:spec.ports[].nodePort and
spec.clusterIp:spec.ports[].port
  
  5. Service Proxy Node :
      
        OpenShift Origin has two different implementations of the service-routing infrastructure:
        
          ● iptables-based
          ● user space
          
The default implementation is entirely iptables-based, and uses probabilistic iptables rewriting
rules to distribute incoming service connections between the endpoint pods. The older
implementation uses a user space process to accept incoming connections and then proxy
traffic between the client and one of the endpoint pods.
The iptables-based implementation is much more efficient, but it requires that all endpoints
are always able to accept connections; the user space implementation is slower, but can try
multiple endpoints in turn until it finds one that works. If you have good readiness checks
(or generally reliable nodes and pods), then the iptables-based service proxy is the best
choice. Otherwise, you can enable the user space-based proxy when installing, or after deploying
the cluster by editing the node configuration file.

      
