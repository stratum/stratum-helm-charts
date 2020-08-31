pipeline {
    agent {
        docker { 
            image 'ubuntu:18.04' 
            args '-u root:sudo'
        }
    }
    stages {
        stage('Install tools') {
            steps {
                sh '''
                set -x
                ls
                cat Jenkinsfile
                '''
            }
        }
        stage('Publish') {
            when {
               branch 'master'
            }
            steps {
                sh '''
                set -x
                echo 'publish'
                '''
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
    }    
}
