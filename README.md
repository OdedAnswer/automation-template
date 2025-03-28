# @pepperi-automation-testing-addons Typescript Template

A template for creating a pepperi automation test using QA team infra

* can run both on an addon which is 'deployed' on local server and on the 'real' server
* create a test file and tests serivce by NPM command - pass tested Addon UUID and wanted test name
* debug the tests in VSC 

## Installation
---
1. its recomended to open your own branch inside this repo: you CANT push to main
2. clone
3. run `npm run init` from the root

## Debugging
---
To debug your addon in `Visual Studio Code`, press `F5` or `Run->Start Debugging`.

## Sanity Testing That Everything Works
---
Once project is installed you can press `F5` (or `Run->Start Debugging`) to start local debug server, then you can use `Run Local Example Test` from postman collection included inside 'automation_assets' folder to validate project is working - this is an example test from the automation framework.

## Project structure
---
The following is an overview of the project structure. 
The node_modules folder is in use by `npm`

#### Folders
|Folder | Description |
| ---:  | :---       |
| .vscode | vscode tasks & launch |
| client-side | TODO |
| server-side | Server Side Tests And Creation Script |
| automation_assets | Postman Collection Which Can Be Used As Template To Calling Tests |
| potentialQA_SDK | all files which contain the QA infra to run tests - QA SDK; only 'generalSerivce' should be of your interest|
| script | JS script to create tests ; dont touch|
| server-side\tests.ts | the function which represents the 'run' endpoint; dont touch |
| server-side\tests\api_tests | location of all generated test files; here is where you work! |
| server-side\tests\api_tests\services | location of all generated services files; here is where you work! |


## Publishing
---
the way I currently see it - no reason to publish this as an addon might be changed

## How To Use - Your Addon Is Already Published
---
1. to create test files enter 'server-side' folder - run the CLI command `npm run create-test server-side {addonUUID} {testName}` while {addonUUID} is the tested addon UUID - and name is the created files name: please notice - you can create various tests with the same UUID but you have to use different test names
 * this will create a service file inside `server-side\tests\api_tests\services` and a test file inside `server-side\tests\api_tests` - you can search these files by searching addons UUID
 * you can alreay call these by:
 * pressing `F5` to start the automation local server 
 * once the server is up - you can call tests by sending `POST` to `http://localhost:4600/tests/run` (you can take the postman inside `automation_assets`) just notice you have to use a valid pepperi token of a user which already has the tested addon installed on it inside the `Authorization` header - the token will decied which env is running, if there are a number of tests on the same UUID - all will run one after the other in random order
 * you have to also pass `AddonUUID` and `isLocal` inside the body of this call:
    * `"AddonUUID"` - should be the UUID of the tested addon, while `"isLocal"` - is whether you want to run on local server or AWS
3. running on local server will mean you have to start your tested addon server locally on 4500 port - then calling the test with `"isLocal":"true"` inside the body
* please notice that inside the service you have TWO clients: `papiClient` - which will call the 'real' pepperi services (from the env which the token provided) dosent matter which data is passed inside `isLocal` and `routerClient` which will call local server is `isLocal` is true; by this logic: calls to pepperi should be done from `papiClient` while `routerClient` should call TESTED addon

## How To Use - Your Addon Is Not Yet Published
---
1. this means you have no UUID to pass, you can use any 36 char long string as the UUID, just notice that:
* you will have to run only on `isLocal`
* you will have to CHANGE the UUID once the addon is published and has 'real' UUID
* you will have to call the `run` endpoint using the 'fake' UUID you gave