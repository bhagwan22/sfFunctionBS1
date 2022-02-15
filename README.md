# sfFunction

Salesforce Functions

Allow Written in the languages and tools of your choice, and  interesting is Salesforce Functions are 100% hosted in a Salesforce, managed elastic compute, and preconfigured to securely integrate with your org.
Salesforce Functions lets you use the Salesforce Platform for building event-driven, elastically scalable apps and experiences. Salesforce Functions is designed to boost developer productivity by reducing your infrastructure responsibilities and enabling you to build and integrate Functions-as-a-Service (FaaS) apps using the languages and tools of your choice.
In short: It can also be defined as A single purpose, on demand, short-live microservice without complex infrastructure. Also scale elastically
# Some Advantages
Use industry programming languages
Use open sources libraries like NPM and Maven
Greater CPU, Heap, and Async limits
Open and Prescriptive Developer Experience

Note: we will be needing salesforce org had function enabled : https://functions.salesforce.com/signups/ 
Or enable function
https://developer.salesforce.com/docs/platform/functions/guide/configure_your_org.html 
Remember to develop and run Salesforce Functions, install development tools. The tools vary depending on the language you use.
https://developer.salesforce.com/docs/platform/functions/guide/set-up.html 
# pre configurations
CLI
Salesforce extension pack for VS code
Now Enable sf command
node --version
npm install sfdx-cli –global
npm install @salesforce/cli –global
Note: sf plugins –core


#Steps Overview
Create supporting function for any language Java, Javascript, typescript
Functions are stored in org “compute environments”
Call Function and limit does not count in transaction limit

First come building blocks Understanding
In our example we are going to use JavaScript function
Each JavaScript function exports as async function, this is where all work is done
Syntax of defining JavaScript function:
	
module.exports = async function(event, context, logger){
	// logic here
}




Event: payload that was passed into the function
Context: allows access to an authenticated Org and the salesforce SDK
Logger: log right from your function

# Syntax for calling function from apex
And invoking a function from apex by Functions.Function class to get function reference and invoke it by passing a JSON payload.
And get response from the functions.FunctionInvocation object.


Salesforce SDK is the gateway to our salesforce data
and we interact with the Salesforce SDK with Context object. And it gives access to Data API, Org, Unit of work, User & more.
i.e in function we can access information like


Here “Data API” lets us Query, Create, Update & Delete Salesforce data and “Unit of work” object is used if we have to make multiple CRUD operation on salesforce and if any one of it is fail then whole operation need to rollback.
“Data API” Example in function we can query and return result


“Unit of work” example


Sf commands: https://github.com/forcedotcom/cli/blob/main/releasenotes/README.md 

#Now let’s get into some hands on
Steps:
Create project in VS code with sfdx: create project
Login to dev-hub org:  sf login
Login to function: sf login functions

Now go to config 🡪 project-scratch-def.json file and add function to features

Note: to run the function locally in our computer, we must have docker installed in our computer. But for this demo we will directly run function from apex.
Now create a function
With command: Sfdx: Create Function 

Write a name for your function, in our case it is queryjs(as we are creating a JavaScript function) and you can select yours

And here it is
Inside function folder our queryjs project(a node js project)
Note: this is going to be a regular node.js project . so it is going to have package json dependencies, have test, etc. So running npm install inside queryjs folder will fetch all dependencies. 



For our example we are trying to fetch 10 Accounts from our org to function and returning the result in JSON
Not it is time to start deployment
Login to function org command: sf login functions
First, we must create a “compute environment” in our org(compute environment is the space where the function is running)
Sf env create compute -o sfFunction1 -a sf_trial_env
In our case it is “sf env create compute -o sfFunction1 -a sf_trial_env”

Now we have to commit project to a git repository, in order to do a successful, deploy
Git add .
Git commit -m “you msg”

Now deploy project to org
sf deploy functions -o <orgAliesname>
in our case it is “Sf deploy functions -o sfFunction1”


Once deployment is success will get the references to call function from apex

Note: in reference  you can see the project name is added in front of function name, So it important to have alias name
No is the step we all are waiting for, calling function via apex

Yupee! And here is the account list

If you note the interesting is the function transaction does not count into salesforce transaction limit 😊 
   And here now you are ready to play around some and build a large-scale calculation




