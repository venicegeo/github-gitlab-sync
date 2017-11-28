
//properties([pipelineTriggers([cron('H/3 * * * *')])])

node {
    stage('Parameterize') {
      if("${params.overwrite_parameters}" == "Yes") {
        properties([[$class: 'ParametersDefinitionProperty', parameterDefinitions: [
          [$class: 'StringParameterDefinition', name: 'github_url', defaultValue: '', description: 'Github repository URL (Use SSH Format if specifying a Github Credential Id)'],
          [$class: 'StringParameterDefinition', name: 'github_branch', defaultValue: 'master', description: 'Github branch to fetch'],
          [$class: 'StringParameterDefinition', name: 'github_credential_id', defaultValue: '', description: 'ID of ssh key in Jenkins credential store with pull permissions on Github (Leave blank if using HTTPS)'],
          [$class: 'StringParameterDefinition', name: 'gitlab_url', defaultValue: '', description: 'Gitlab repository URL (Use SSH Format)'],
          [$class: 'StringParameterDefinition', name: 'gitlab_branch', defaultValue: 'master', description: 'Gitlab branch to fetch'],
          [$class: 'StringParameterDefinition', name: 'gitlab_credential_id', defaultValue: '', description: 'ID of ssh key in Jenkins credential store with write permissions on Gitlab'],
          [$class: 'ChoiceParameterDefinition', name: 'overwrite_parameters', choices: "Yes\nNo", description: 'Set to yes to overwrite parameters'],  
        ]]])
        currentBuild.result = 'ABORTED'
        error('Parameters Reset')
      }
    }
    stage('Setup') {
        deleteDir()
    }
    stage('Clone Github Repo') {    
        if(params.github_credential_id) {
          git([
              url: params.git_url,
              branch: params.git_branch,
              credentialsId: params.git_credential_id,
          ])
        } else {
          git([
              url: params.git_url,
              branch: params.git_branch,
          ])
        }
    }
    stage('Push to Gitlab') {
        sshagent([params.gitlab_credential_id]) {
          sh """set -x
              ls
              ssh-add -l
              git branch -v
              git remote add gitlab ${params.gitlab_url}
              git checkout ${params.github_branch}
              git pull origin ${params.github_branch}
              git push gitlab ${params.gitlab_branch}
          """
        }
    }  
    stage('Cleanup') {
        deleteDir()
    }
}
