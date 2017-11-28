properties([[$class: 'ParametersDefinitionProperty', parameterDefinitions: [
  [$class: 'StringParameterDefinition', name: 'git_url', defaultValue: '', description: 'Github repository URL (Use SSH Format)'],
  [$class: 'StringParameterDefinition', name: 'git_branch', defaultValue: 'master', description: 'Git branch to fetch'],
  [$class: 'StringParameterDefinition', name: 'git_credential_id', defaultValue: '', description: 'ID of ssh key in Jenkins credential store'],
  [$class: 'StringParameterDefinition', name: 'gitlab_project', defaultValue: '', description: 'Gitlab repository URL (Use SSH Format)'],  
]]])
//properties([pipelineTriggers([cron('H/3 * * * *')])])

node {
    stage('Setup') {
        deleteDir()
    }
    stage('Clone Github Repo') {    
        git([
            url: params.git_url,
            branch: params.git_branch,
            credentialsId: params.git_credential_id,
        ])
    }
    stage('Push to Gitlab') {
        sshagent([params.git_credential_id]) {
          sh """set -x
              ls
              ssh-add -l
              git branch -v
              git remote add gitlab ${params.gitlab_project}
              git checkout master
              git pull origin master
              git push gitlab master 
          """
        }
    }  
    stage('Cleanup') {
        deleteDir()
    }
}
