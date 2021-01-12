pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'jdk11'
    }
    parameters {
        booleanParam(name: "Perform release ?", description: '', defaultValue: false)
    }
    stages {
        stage('Initialize'){
            steps {
                bat '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    '''
            }
        }
            stage('Build') {
                steps {
                    bat 'mvn compile'
                }
            }
            stage('Test') {
                steps {
                    bat 'mvn test'
                }
            }
        stage('Deploy') {
            steps {
                configFileProvider([configFile(fileId: '49dd48d9-6105-40d5-bc6f-d3db4782035d', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                    sh 'mvn -gs %MAVEN_GLOBAL_SETTINGS% deploy'
                }
            }
        }
    }
}
