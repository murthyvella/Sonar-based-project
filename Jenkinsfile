pipeline{
    agent any
    
    environment{
         SCANNER_HOME = 'sonar-scanner'
    }
    tools{
        maven 'maven3'
    }
    stages{
        stage('codecheckout'){
            steps{
                git branch: 'main', url: 'https://github.com/murthyvella/Sonar-based-project.git'
            }
        }
        stage('buildcode'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('analysis'){
            steps{
                sh '''
                 mvn clean verify sonar:sonar \
                 -Dsonar.projectKey=sonar-project \
                 -Dsonar.host.url=http://54.167.63.76 \
                 -Dsonar.login=sqp_113c73e5b7ac461809cc131f694fc55dfbb0a825
                
                '''
            }
        }
        stage('build the code'){
            steps{
                sh 'mvn package'
            }
        }
        stage('build the image'){
            steps{
                sh 'docker build -t spotifyapp:latest .'
                
            }
        }
        stage('tag and pushcode'){
            steps{
                  sh  'docker tag spotifyapp:latest murthyvella/spotifyapp:latest'
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
                  sh 'docker login -u ${dockeruser} -p ${dockerpass} '
                  sh 'docker push murthyvella/spotifyapp:latest '
              }
            }
        }
        stage('application live'){
            steps{
                sh 'docker run -d --name spotify-app -p 5555:5555 murthyvella/spotifyapp:latest'
            }
        }
    }
}
