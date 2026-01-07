pipeline {
  agent any
  stages {
    stage('Info') {
      steps {
        script {
          if (env.CHANGE_ID) {
            echo "This is a PR build: #${env.CHANGE_ID}, source branch: ${env.CHANGE_BRANCH}, target branch: ${env.CHANGE_TARGET}"
          } else {
            echo "This is a branch build: ${env.BRANCH_NAME}"
          }
        }
      }
    }
    stage('Static Code Analysis') {
      steps {
        echo 'Running Static Code Analysis tools...'
      }
    }
    stage('Linode provisioning stage') {
      steps {
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
    stage('Linode Tests') {
      steps {
        echo 'Running tests on provisioned Linode...'
      }
    }
    stage('App deployment stage') {
      steps {
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
    stage('Deployment Tests') {
      steps {
        echo 'Running tests after app deployment...'
      }
    }
    stage('Linode deletion stage') {
      steps {
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
    stage('Report stage') {
      steps {
        echo 'Sending report to the team...'
      }
    }
  }
}