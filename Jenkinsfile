pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/AhmedKanoun90/DevopsProject.git' 
                
            }
        }
        stage('Clean package') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test1') { 
            steps {
                sh 'mvn test' 
            }
     }
      stage('IntegrationTest') { 
            steps {
                sh 'mvn verify -DskipTests' 
            }
     }
      stage ("SonarQube Check") {
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh 'mvn -DskipTests clean package sonar:sonar'
                    }
                   }
                    
                }
            }
     stage('Maven clean install') { 
            steps {
                sh 'mvn clean install' 
            }
     }
       
     stage("mvn Package") {
            steps {
                script {
                    sh "mvn package -DskipTests=true"
                }
            }
        }
    

       stage("Nexus Deploy") {
            steps {
                script {
                    sh "mvn clean package deploy:deploy -DgroupId=com.esprit.examen -DartifactId=tpAchatProject -Dversion=1.0 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://192.168.1.131:8081/repository/maven-releases/ -Dfile=target/tpAchatProject-1.0.jar"
                }
            }
        }
        
        stage("Build Docker image") {
            steps {
                script {
                    sh "docker build -t devopsproject/tpAchatProject-1.0 ."
                }
            }
        }
       
         
    }
}


