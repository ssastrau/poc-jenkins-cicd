pipeline {
  agent any
  stages {
    stage('Static Code Analysis') {
      steps {
        githubChecks(name: 'Static Code Analysis') {
          echo 'Running Static Code Analysis tools...'
        }
      }
    }
    stage('Linode provisioning stage') {
      steps {
        githubChecks(name: 'Linode provisioning stage') {
          script {
            def userInput = input(
              message: 'Provision your Linode',
              parameters: [
                [$class: 'ChoiceParameterDefinition', choices: ['nano', 'micro', 'mini'].join('\n'), name: 'size', description: 'Select Linode size'],
                [$class: 'ChoiceParameterDefinition', choices: ['se-sto', 'us-ord', 'jp-osa'].join('\n'), name: 'region', description: 'Select Linode region']
              ]
            )
            echo "Provisioning Linode of size: ${userInput['size']} in region: ${userInput['region']}"
          }
        }
      }
    }
    stage('Linode Tests') {
      steps {
        githubChecks(name: 'Linode Tests') {
          echo 'Running tests on provisioned Linode...'
        }
      }
    }
    stage('App deployment stage') {
      steps {
        githubChecks(name: 'App deployment stage') {
          script {
            def deployment = input(
              message: 'Select App deployment method:',
              parameters: [
                [$class: 'ChoiceParameterDefinition', choices: ['jenkins', 'docker', 'MySQL'].join('\n'), name: 'deployment_method']
              ]
            )
            echo "Deploying the app with: ${deployment}"
          }
        }
      }
    }
    stage('Deployment Tests') {
      steps {
        githubChecks(name: 'Deployment Tests') {
          echo 'Running tests after app deployment...'
        }
      }
    }
    stage('Linode deletion stage') {
      steps {
        githubChecks(name: 'Linode deletion stage') {
          script {
            def confirm = input(
              message: 'Do you want to delete the Linode?',
              parameters: [
                [$class: 'ChoiceParameterDefinition', choices: ['Yes', 'No'].join('\n'), name: 'confirm_delete']
              ]
            )
            if (confirm == 'Yes') {
              echo 'Deleting the Linode...'
            } else {
              echo 'Deletion cancelled by user.'
            }
          }
        }
      }
    }
    stage('Report stage') {
      steps {
        githubChecks(name: 'Report stage') {
          echo 'Sending report to the team...'
        }
      }
    }
  }
}