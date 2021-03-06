# ABX-vRO-Tagging Sample

This an example of how Tags could be further manipulated with Extensibility, either way via ABX or vRO

Keep in mind 90 % of the time Metadata Tagging can be handled by Cloud Assembly Blueprint but for those corner cases, you could try this approach

Besides, this is an example that could be used as reference for accessing any data generated by vRA at any stage and being passed by Extensibility framework

# Requirements
    vRA 8.X or vRA Cloud with ABX and vRO 8.X 
    
Using the same basic Blueprint ( adding an On-Demand Network and VM )
I created a custom property   

    advanced-tag:
        type: string
        enum:
          - vRO
          - ABX
        default: vRO
        title: Extensibility Engine
        description: Controls Extensibility Engine for flows execution

That I will use as criteria for executing extensibility via ABX or vRO 

I have created the ABX "Set VM and Tags Properties" workflow 

![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/setvmtagesproflow.png)
 
 You can see it calls two specific ABX Actions:
 
"Set VM Tags", which it is a NodeJS based Script, 
it will ingest two properties "metadata" & "tags" coming from vRA when we call this ABX Flow via a Subscription for the Event Topic: Compute Allocation 
 
 ![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/setvmtag.png)
 
 and "Set VM Custom Properties", which it is a Python based Script,
 
 ![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/setvmtagespro.png)
 
 Please note, each "Event Topic:" will expose specific data associated to that stage ( and the types too )
 in this example I am using the Event Topic: Compute Allocation, which allows me to access this data at vRA for my deployment
 
 ![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/etcomputeallocation.png)
  
  Now, I have created a subscription for ABX execution with a condition for triggering the extensibility execution using as criteria my Custom Property from my Blueprint 
  
    event.data.customProperties['enable_ext'] == "ABX"

and then call my ABX Flow "Set VM and Tags Properties" workflow if that property is met ( Basic If-Then-Else )
  
 ![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/subscriptionabx.png)
 
if I instance my Blueprint and select "ABX"
 
 ![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/DeploymentInput.png)
 
This will meet my subscription criteria and will execute the ABX "Set VM and Tags Properties" workflow and associated ABX Flows
 
 ![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/ActionRunsABX.png)
 
 Looking into details 
 
  ![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/8a7480af713df66401715552c0110005.png)
 
 Among all the "input" data available per the Event Topic Schema, you can see the "tags" generated at the blueprint
 
![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/taginput.png)
 
But also note the output, as you can see how the metadata was used, applied in the logic from the script and sent back to vRA to continue the deployment processing 

![detailsAction](https://github.com/moffzilla/abx-vro-sample/blob/master/media/tagoutput.png)


