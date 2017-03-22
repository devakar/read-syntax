# App-Autoscaler operator manual

## 1. Introduction 

The App-Autoscaler have been developed to scale up or down the application instances according to given specific policy. It scales an application based on memory usage and schedule. User could be able to scale applications as per their requirements. This document contains relevant information for operating this service.

## 2. Automated deployment on different landscapes

The deployment of autoscaler on any landscape is completely automated, user just have to run some commands (commands are given in later section).

## 2.1. Prerequisites:
  * A Linux system with installed jumpbox or landscape jumpbox.
  * Proper network configuration in the linux system.

## 2.2 Deployment steps:
  * ## Set up configuration file:
    Mention the configuration set up of deployment in ``` config.yml ```file. This file should be in path ```landscape/ deployments/autoscaler/config.yml```. User could get the format and specification of config file in,
   ```landscape/products/cf-hcp/components/autoscaler/example/config-example.yml```.

  * ## Set up credential file:
    Mention the credential set up of deployment in ```credentials.yml``` file. This file should be in path ```landscape/ deployments/autoscaler/credentials.yml```. User could get the format and specification of credentials file in, ```landscape/products/cf-hcp/components/autoscaler/example/credentials-example.yml```.

  * ## Set up certificate files:
    Before deployment, make sure certificate files exist in path ```landscape/deployments/autoscaler/credentials```   as mentioned in ```landscape/products/cf-hcp/components/autoscaler/example/credentials``` directory.
If these certificate files does not exist, one can use certicate files from ```landscape/products/cf-hcp/components/autoscaler/example/credentials``` directory. But, this could lead to security issue.
There is a script which could generate all these certicates files and save to ```landscape/products/cf-hcp/components/autoscaler/example/credentials``` directory, that should be used. The command to generate certificate files:
      ```
      iac -d autoscaler action create_certs
      ```

  
    
  * ## Deploy command:
    Run the following command to deploy the app-autoscaler to the landscape:
    ```
    iac -d autoscaler lifecycle deploy
    ```
## 2.3 Test the deployment:
Run the following command to run the acceptance test of the app-autoscaler on the landscape:
 ```
        iac -d autoscaler lifecycle test  
 ```
 
## 3. Troubleshoot problems:
 The logs of created VMs are advisory for all categories of issues. 
## 3.1. Check logs of created VMs:
* Set Bosh deployment with app-autoscaler manifest
   ```
   bosh deployment <path to app-autoscaler manifest file>
   ```
    The app-autoscaler manifest file exist in path ```landscape/deployments/autoscaler/gen/manifest.yml```
* SSH to specific VM for which one need to check logs
  ```
  bosh ssh
  ```
  The above cammand will ask to enter in specific VM, enter the specfic VM.
* Logs of different job
  ```
  cd /var/vcap/sys/log/<job-name>
  vi <log-file>
  ```
## 3.2. VM is failing or stopped:
Check logs of VMs which is failing or stopped, by the procedure mentioned in section **3.1**

## 3.3. Scaling didn't occured for memory usage scaling:
* Check logs of VM having **metricscollector** job by the procedure mentioned in section **3.1**, to verify that metricscollector polled metrices from loggregator successfully or not.
* Check logs of VM having **eventgenerator** job by the procedure mentioned in section **3.1**, to verfiy that eventgenerator triggered scaling request to scalingengine or not.
* Check logs of VM having **scalingengine** job by the procedure mentioned in section **3.1**, to verify that scalingengine processed the scaling request successfully or not.

## 3.4 Scaling didn't occured for schedule based scaling:
* Check logs of VM having **scheduler** job by the procedure mentioned in section **3.1**, to verify that scheduler triggered scaling request to scalingengine or not.
* Check logs of VM having **scalingengine** job by the procedure mentioned in section **3.1**, to verify that scalingengine processed the scaling request successfully or not.