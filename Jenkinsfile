pipeline {  

    environment {
          PATH_2_POM = './gri-parent/importers/alpine-importer/alpine-importer-service'
          IMAGE = readMavenPom().getArtifactId()
          VERSION = readMavenPom().getVersion()
          DOCKER_IMAGE_JAVA_OPS_PARAM = readMavenPom().properties["docker.image.java.ops"]
    }

    stages{
        stage ('clean and clone') {
            steps {
                cleanWs()
                git branch: 'master',                    
                    url: 'git@github.com:yaniva/jenkins.git'
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
