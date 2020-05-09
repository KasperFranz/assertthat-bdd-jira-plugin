# AssertThat BDD Jira Plugin

Jenkins plugin for interaction with [AssertThat BDD Jira plugin](https://marketplace.atlassian.com/apps/1219033/assertthat-bdd-test-management-in-jira?hosting=cloud&tab=overview).

Main features are:

- Download feature files before test run
- Filter features to download based on mode (automated/manual/both), or/and JQL
- Upload cucumber json after the run to AsserTthat BDD Jira plugin

## Create credentials

![Create credentials](docs/credentials.PNG?raw=true "Create credentials")

## Usage in Job 

Download feature files: 

![Download feature files](docs/download-features.PNG?raw=true "Download feature files")

Upload reports:

![Upload reports](docs/upload-report.PNG?raw=true "Upload reports")

## Usage in pipeline Job

```
pipeline {
    agent any 
    stages {
        stage('Features') { 
            steps {
                //Download feature files
                assertthatBddFeatures(credentialsId: '10005',  jiraServerUrl: '', jql: 'project=DEMO', mode: 'automated', outputFolder: 'features', projectId: '10005')
            }
        }
        stage('Run tests') { 
            steps {
                //Run tests here
            }
        }

    }
    post{
        always{
                //Upload test results
                assertthatBddReport(credentialsId: '10005',  jiraServerUrl: '', jsonReportFolder: 'report', jsonReportIncludePattern: '**/*.json', projectId: '10005', runName: 'Smoke test run', type: 'karate')
        }
    }

```
