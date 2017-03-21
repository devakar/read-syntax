# App-Autoscaler operator manual

## 1. Introduction 

The App-Autoscaler have been developed to scale up or down the application instances according to the given specific policy. It scales an application based on memory usages and schedules. User could be able to scale applications as per their requirements. This document contains relevant information for operating this service.

## 2. Automated deployment on different landscapes

The deployment of autoscaler on any landscape is completely automated, operator just have to set up all the prerequisites and run the mentioned commands.

## 2.1. Prerequisites:
  * A Linux system with installed jumpbox or landscape jumpbox.
  * Proper network configuration in the linux system.

## 2.2 Deployment steps:
  * ## Set up configuration file:
    Mention the configuration set up of deployment in ``` config.yml ``` file. This file should be in path ```landscape/deployments/autoscaler/config.yml```. Opertaor could get the format and specification of config file in, ```landscape/products/cf-hcp/components/autoscaler/example/config-example.yml```.

  * ## Set up credential file:
    Mention the credential set up of deployment in ```credentials.yml``` file. This file should be in path ```landscape/deployments/autoscaler/credentials.yml```. Opertaor could get the format and specification of credentials file in,  ```landscape/products/cf-hcp/components/autoscaler/example/credentials-example.yml```.

  * ## Set up certificate files:
    Before deployment, make sure certificate files exist in  ```path landscape/deployments/autoscaler/credentials``` directory as mentioned in ```landscape/products/cf-hcp/components/autoscaler/example/credentials``` directory.
If these certificate files does not exist, one can use certicate files from ```landscape/products/cf-hcp/components/autoscaler/example/credentials``` directory. But, this could lead to security threats.
There is a script which could generate all these certicates files and save to ```landscape/products/cf-hcp/components/autoscaler/example/credentials``` directory, that should be used for better security. The command to generate certificate files:
      ```
      iac -d autoscaler action create_certs
      ```

  
    
  * ## Deployment command:
    Run the following command to deploy the app-autoscaler on the landscape:
    ```
    iac -d autoscaler lifecycle deploy
    ```
## 2.3 Test the deployment
Run the following command to run the acceptance test of the app-autoscaler on the landscape:
 ```
        iac -d autoscaler lifecycle test  
 ```
 
                            

