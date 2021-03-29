pipeline {
  agent any
  stages {
    stage('Build') {
      post {
        failure {
          emailext(subject: 'Project Integration', body: 'There was an error integrating your project!', from: 'ha_rezgui@esi.dz', to: 'hn_messaoudi@esi.dz')
        }

      }
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/**'
        archiveArtifacts 'build/test-results/**'
      }
    }

    stage('Mail Notification') {
      steps {
        emailext(subject: 'Project Integration', body: 'The project has been integtated successfully!', from: 'hn_messaoudi@esi.dz', to: 'ha_rezgui@esi.dz')


      }
    }

    stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonarqube') {
                      bat(script: 'gradle sonarqube', label: 'script')
                    }
                }
        }

    stage('Test Reporting') {
      steps {
        cucumber '**/*.json'
      }
    }

    stage('Deployment') {
      steps {
        bat 'gradle publish'
      }
    }

    stage('Slack Notifacation') {
      steps {
        slackSend(channel: '#ogl', message: 'The project is updated!')
      }
    }

  }
}