# Introduction 

A postman/newman collection to create Trello board
# Getting Started
TODO:
1.	Installation process
2.	Software dependencies
3.	Latest releases
4.	API references

# Build and Test

To edit and run locally:
- Install postman (https://www.postman.com/downloads/)
- Install newman to run the collection
- Install newman reporter html and htmlextra

## Install newman and html reporter
See https://adevait.com/qa/how-to-create-elegant-html-reports-in-postman.

- Note: Nodejs should be installed to make use of npm.
- npm install -g newman
- npm install -g newman-reporter-html
- npm install -g newman-reporter-htmlextra

### Newman
To run a collection using newman:
```bash
newman run knab_create_trello.postman_collection.json -e Trello_Board.postman_environment.json --working-dir ./testdata -r htmlextra --reporter-htmlextra-hideResponseBody "Get report"
```
- -e : environment 
- -d : path to iteration data
- --working-dir : path to the file locations (images)
- --verbose : hm, i didn't see anyting ..
- --reporter-htmlextra-hideResponseBody : to exlude big reponses from the final report
- --suppress-exit-code : to other tests wil run after one failure

Default the html report is saved in the subfolder `newman`. You can change that using `--reporter-htmlextra-export <path>`.

See: https://github.com/DannyDainton/newman-reporter-htmlextra/blob/main/README.md.

### Postman

First import the collection `knab_create_trello.postman_collection.json` and the environment `Trello_Board.postman_environment.json` (or another).

You can run the requests separately or as a collection.
To run it as a collection using postman, click on the ... right next to the collection name and select `run collection`.
Note: *Files to upload should be in the working directory of postman to run collection locally on pc.*
Default the working directory is `~/Postman/files`.
Easy Tip: copy all testdata files to this location,
or change it to the testdata folder.

Pro Tip: formatting javascript code press `Ctrl+b`.

### Azure pipeline

Postman api call environment.get('xxx') is only getting the variable from the environment.json file. Not the environment variable from the OS. Use `--env-var "key=value"` to pass environment variables.

The environment file `Trello_Board.postman_environment.json` contains the base variables, they are `Production`. The rest are provided via the command line.

```bash
newman run knab_create_trello.postman_collection.json -e Trello_Board.postman_environment.json --working-dir ./testdata -r htmlextra --reporter-htmlextra-hideResponseBody "Get report" --suppress-exit-code 
```
### Azure pipeline (Env. Variables if required to run in Pipelines , Please first try to run it as Newman in cli before trying to run in pipelines)
##-env-var "auth_url=$(auth_url)" --env-var "api_url=$(api_url)" --env-var "client_id=$(client_id)" --env-var "client_secret=$(client_secret)"

To export 2 different reports, because junit format is better integrated in the Azure pipeline.

```bash
newman run knab_create_trello.postman_collection.json -e Trello_Board.postman_environment.json --working-dir ./testdata -r "htmlextra,cli,junit" --reporter-htmlextra-hideResponseBody "Get report" --reporter-junit-export newman/TEST-junitreport.xml --suppress-exit-code  
```
# Contribute

### Update or add tests using postman client:

- Import the collection and environment in postman.
- Review and change/add requests and tests. 
- Test run with collection runner, select the right iteration-data
- Export collection and environment (if changed).
- Run a test with newman locally.



### Trello API Powerup.

- Trello Api to create board has been made possible by Powerup.
- Launch Powerup to Create API key and then a Token (https://developer.atlassian.com/cloud/trello/guides/power-ups/your-first-power-up/)
- Send POST with key and token to create a board.
-The Request should look like as below 
-POST https://api.trello.com/1/boards/?name=NewBoard_Bik2&key=f5606f0b8bd139818b42da7d8b1e1d1b&token=ATTA49668787946b28bbb174aa89cf1d939e7e5fece2400c504759c8f9e9c780789cEACCB900


```