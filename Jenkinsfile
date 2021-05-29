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
                    echo tag version: ${TAG_VERSION}                   
                    echo 'commit  + tag ...'
                '''  
                script{
                    env.NEXT_VERSION = bumpVersion(env.NEXT_VERSION)
                }
                sh'''
                    echo next version ${NEXT_VERSION}                   
                    echo 'commit next snapshot ...'
                '''  
                
            }        
        }
    }
}

def bumpVersion(version){
    def versionParts = version.tokenize('.')        
    major = versionParts[0].toInteger()
    minor = versionParts[1].toInteger()
    patch = versionParts[2].toInteger()
    return "${major}.${minor}.${patch+1}-SNAPSHOT"
}
