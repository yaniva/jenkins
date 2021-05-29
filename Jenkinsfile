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
       stage('bump version') {
            steps {
                
                sh '''
                mvn versions:set versions:commit -DremoveSnapshot
                '''
                
                script{
                    env.TAG_VERSION = sh(script: "mvn help:evaluate -Dexpression=project.version -q -DforceStdout", returnStdout: true).trim()
                }
                
               sh'''
                    echo ${TAG_VERSION}
                    git commit -am "committing release version" 
                    git push
                    git tag -a ${TAG_VERSION} -m "tagging release version" 
                '''                
            }        
        }
    }
}
