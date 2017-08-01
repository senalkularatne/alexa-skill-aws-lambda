# README #

This README documents steps are necessary to get the application up and running.

-----------------------

## Purpose ###

* Setup local debugging for Alexa skill
	+ No need to upload .zip files to lambda
	+ Test without talking to any Alexa devices
	+ Local access to Amazon CloudWatch
* Version 1.0

-----------------------

### Project Structure ###

* speechAssets
	+ IntentSchema.json
	+ interaction-model.json
	+ SampleUtterances.txt
* src
	+ index.js
* test
	+ context.json
	+ input.json - Used to manage how we do testing locally. Ex: Intents, language, change userID so information will persist when using DB.
	+ main.js
* credentials.csv - contains info regarding Access key ID and Secret access key for the user (admin)
* .vscode (hidden)
	+ launch.json - 

-----------------------

### Setup Instruction ###

1. Open project in Visual Studio Code
2. cd into /src and ``` npm install ```
3. cd into /test and ``` ln -s ../src/node_modules/ node_modules ``` This will create a symbolic link
4. Install [AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) using pip
5. In command line run ``` AWS configure ``` Copy info from credentials.csv
6. Start testing by adding breakpoints and hitting play. Use developer console.
	+ On index.js put a breakpoint before speechOutput to check the JSON file that is returned. Alexa would speak whatever that goes betwenn <speak>... </speak>. 

-----------------------

### Extra ###

* Code that pertains to local debugging is marked with // IMP

-----------------------

### Sources ###

* [Alexa Skill]()
* [Lambda](https://console.aws.amazon.com/lambda)
* [IAM](https://console.aws.amazon.com/iam)
* [DynamoDB]()

-----------------------

### Note to Self ###
* Create a symbolic link from the modules in /src into /test directory. Otherwise when we try to debug we won't find modules in our /test directory and it will crash. This is a better option instead of duplication the modules in two seperate directories (running npm twice). Try ``` ls -l ``` and you will see ``` node_modules -> ../src/node_modules/ ``` which means we have correctly linked the /test directory to node_modules in /src directory.

#### Configure AWS/IAM ####
* IAM - used to create group user role and attach policies
	+ Group name - localAlexaDebug
		* Attached policies - AmazonDynamoDBFullAccess & CloudWatchLogsFullAccess
	+ User name - admin
		* This user (admin) is added to the group (localAlexaDebug) so he can get access to the attaches policies locally
		* After creating user we have access to .csv file. Download this file as it has info regarding Access key ID and Secret access key
	+ Role name - localAlexaDebug (Same as Group name)
		* Role is connected to AWS Lambda
		* Attach same policies as the group
		* Click tole name and edit trust relationship. After "Service" add a comma and create a property "AWS" and __Paste the value from User ARN__ 

#### Attach Code to IAM ####
* Go to /test/main.js __Paste the Role ARN here__
* Configure AWS to be able to use the user (admin) that we created to assume the role that we added in /test/main.js
* In command line run ``` AWS configure ``` Copy info from credentials.csv

#### Setup Debug ####

1. Open project in _Visual Studio Code_ text editor. Click Debug icon and click configure button. This will automatically setup by opening launch.json file (Select environment as Node.js)
2. This will create a hidden .vscode directory in the main project directory and will  contain launch.json file
3. Override this data 











