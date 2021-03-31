pipeline {  
    agent any
    
     tools { 
        maven 'mvn3.6.3'
    }   

    stages{
        stage ('clean and clone') {
            steps {
                cleanWs()
                echo 'finish cleanws'
                git branch: 'main', url: 'https://github.com/yaniva/jenkins.git'
                echo 'Finishing clone'
                script{
                    env.IMAGE = readMavenPom().getArtifactId()
                    env.VERSION = readMavenPom().getVersion()
                    env.DOCKER_IMAGE_JAVA_OPS_PARAM = readMavenPom().getProperties()["docker.image.java.ops"]
                }
                echo 'Finishing env setup'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
                echo $IMAGE
                echo $VERSION
                echo $DOCKER_IMAGE_JAVA_OPS_PARAM
                docker version
                '''
            }
        }
    }
}
