# EKS Factory Reference Guides

This guide will be used as a reference guide on how to get started with the EKS-Blueprint that uses the AWS CDK. The EKS-Blueprint is using Typescript to define all of the EKS resources that you will be using in this guide. 

## Prerequisites

1. Exposure to working with various Typescript constructs or any of the languages supported by the AWS CDK
2. Experience working with the AWS CDK
3. Active AWS Account

## Getting Started

1. Install the AWS CDK
     - Run the following command: ```npm install -g aws-cdk@1.91.0``` Please use the 1.91.0 version specifically to avoid issues with the npm package manager
2. Verify the installation: ```aws-cdk --version```

## Project Setup

1. Create a new CDK project using Typescript
    - ```cdk init app --language typescript```
2. Bootstrap your environment - more information on bootstrapping using the AWS CDK can be found here - https://docs.aws.amazon.com/cdk/latest/guide/bootstrapping.html
    - ```cdk bootstrap aws://<AWS_ACCOUNT_ID>/<AWS_REGION>```

## Usage

1. Add the cdk-eks-blueprint library as a dependency to your CDK project 
```  
      "dependencies": {
          "@shapirov/cdk-eks-blueprint": "0.1.5"
        }
```
1. Replace the contents of bin/<your-main-file>.ts (where your-main-file) by default is the name of the root project directory) with the following:
```       
        import 'source-map-support/register';
        import * as cdk from '@aws-cdk/core';
        import {
            CdkEksBlueprintStack, 
            ArgoCDAddOn,
            MetricsServerAddon, 
            ClusterAutoScaler, 
            ContainerInsightsAddOn, 
            NginxAddon, 
            CalicoNetworkPolicyAddon, 
            ClusterAddOn
        }  from '@shapirov/cdk-eks-blueprint';
        
        const addOns: Array<ClusterAddOn> = [
          new ArgoCDAddOn,
          new MetricsServerAddon,
          new ClusterAutoScaler,
          new ContainerInsightsAddOn,
          new NginxAddon, 
          new CalicoNetworkPolicyAddon,
        ];
        
        const app = new cdk.App();
        new CdkEksBlueprintStack(app, {id: 'east-test-1', addOns: addOns, teams: []}, {
          env: {
              account: 'AWS_ACCOUNT_ID',
              region: 'REGION'
          },
        });
 ```       

## Deploy the stack

1. Run the following to deploy your stack: cdk deploy
2. Once the deployment is complete you should see the following output 

```
✅ east-test-1

Outputs:
east-test-1.easttest1ClusterName8D8E5E5E = east-test-1
east-test-1.easttest1ConfigCommand25ABB520 = aws eks update-kubeconfig --name east-test-1 --region us-east-2 —role-arn arn:aws:iam::572179936737:role/east-test-1-easttest1MastersRole19C9E55A-EESH5L8KOXMJ
east-test-1.easttest1GetTokenCommand337FE3DD = aws eks get-token --cluster-name east-test-1 --region us-east-2 —role-arn arn:aws:iam::572179936737:role/east-test-1-easttest1MastersRole19C9E55A-EESH5L8KOXMJ

Stack ARN:
arn:aws:cloudformation:us-east-2:572179936737:stack/east-test-1/1daae6f0-a2b2-11eb-9bba-02ee62609f84
```

If the stack deployed properly based on the output above you are ready to move on to the next step. The next section will cover how to leverage the various add on features that are included in this blueprint. 