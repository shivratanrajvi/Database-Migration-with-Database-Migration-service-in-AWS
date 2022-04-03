# Database Migration with the Database MIgration Service

This advanced demo consists of 5 steps :-

- STEP 1 : Provision the environment and review tasks 
- STEP 2 : Establish Private Connectivity Between the environments (VPC Peer) **<= THIS STAGE**
- STEP 3 : Create & Configure the AWS Side infrastructure (App and DB)
- STEP 4 : Migrate Database & Cutover
- STEP 5 : Cleanup the account

# STEP 2 - Create a VPC peer between On-Premises and AWS

Move to the VPC Console https://console.aws.amazon.com/vpc/home?region=us-east-1#  
Click on `Peering Connections` under `Virtual Private Cloud`  
Click `Create Peering Connection`  
for `Peering connection name tag` choose `A4L-ON-PREMISES-TO-AWS`  
for `VPC (Requester)` choose `onpremVPC`  
for `VPC (Accepter)` choose `awsVPC`  
Scroll down and click `Create Peering Connection`  
Click `OK`  
Select the peering connection, then click `Actions` and then `Accept Request`  
Click `Yes, Accept`  
Click `Close`  

# Create Routes on the On-premises side
Move to the route tabes console https://console.aws.amazon.com/vpc/home?region=us-east-1#RouteTables:sort=routeTableId  
Locate the `onpremPublicRT` route table and select it using the checkbox.  
Click on the `Routes` Tab.  
You're going to add a route pointing at the AWS side networking, using the VPC Peer.  
Click `Edit Routes`  
Click `Add Route`  
For Destination enter `10.16.0.0/16`  
Click the `Target` dropdown & click `Peering Connection` and select the `A4L-ON-PREMISES-TO-AWS` then click `Save routes`  
Click `Close`  
The Onpremises network can now route to the AWS Network, but as data transfer requires bi-directional traffic flow, you need to do the same at the other side.


# STEP 2C - Create Routes on the AWS side
Move to the route tabes console https://console.aws.amazon.com/vpc/home?region=us-east-1#RouteTables:sort=routeTableId  
Locate the `awsPublicRT` route table and select it using the checkbox.  
Click on the `Routes` Tab.  
You're going to add a route pointing at the AWS side networking, using the VPC Peer.  
Click `Edit Routes`  
Click `Add Route`  
For Destination enter `192.168.10.0/24`  
Click the `Target` dropdown & click `Peering Connection` and select the `A4L-ON-PREMISES-TO-AWS` then click `Save routes`  
Click `Close`  

Move to the route tabes console https://console.aws.amazon.com/vpc/home?region=us-east-1#RouteTables:sort=routeTableId  
Locate the `awsPrivateRT` route table and select it using the checkbox.  
Click on the `Routes` Tab.  
You're going to add a route pointing at the AWS side networking, using the VPC Peer.  
Click `Edit Routes`  
Click `Add Route`  
For Destination enter `192.168.10.0/24`  
Click the `Target` dropdown & click `Peering Connection` and select the `A4L-ON-PREMISES-TO-AWS` then click `Save routes`  
Click `Close`  


# STEP 2 - FINISH   

At this point you have created the peering connection between the VPCs and the gateway objects within each VPC.  
you have also configured routing from ONPremises -> AWS and vice-versa.  
In stage 3 you will use this architecture to begin a migration.  


