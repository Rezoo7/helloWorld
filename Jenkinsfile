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
                    bat 'mvn -gs %MAVEN_GLOBAL_SETTINGS% deploy'
                }
            }
        }
        
        stage('Release') {
        when { expression { params['Perform release ?']} }
            steps {
                
            withCredentials([usernamePassword(credentialsId: 'mjidsaa', passwordVariable: 'PASSWORD_VAR', usernameVariable: 'USERNAME_VAR')]){
                bat 'git config --global user.email "maxime.guigourez@gmail.com"'
                bat 'git config --global user.name "MaximeG"'
                bat 'git branch release/'+pom.version.replace("-SNAPSHOT","")
                bat 'git push origin release/'+pom.version.replace("-SNAPSHOT","")
                bat 'mvn release:prepare -s C:/Users/Majid/.m2/settings.xml -B -Dusername=$USERNAME_VAR -Dpassword=$PASSWORD_VAR'
                bat 'mvn release:perform -s C:/Users/Majid/.m2/settings.xml -B -Dusername=$USERNAME_VAR -Dpassword=$PASSWORD_VAR'

            }
            }
        } 
        stage('Sonar') {
            steps {
                bat "mvn -s C:/Users/maxim/.m2/settings.xml sonar:sonar -Dsonar.login=admin -Dsonar.password=admin"
            }
        }
   }
}
