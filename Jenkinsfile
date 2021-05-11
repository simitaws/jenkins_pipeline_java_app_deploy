pipeline {
    
agent {
    docker {
        image 'theoneeagle/jenkins-docker-agent-image:1.2'
        args  '-v /var/run/docker.sock:/var/run/docker.sock -u 0:0'
    }
}
    
stages{
    
    stage ('git clone') {
        steps {
            sh 'echo %Path%'
            git branch: 'main', url: 'https://github.com/isavdeev/docker_java_build_deploy_prod.git'
        }
    }   
        
    stage ('build') {
        steps {
            sh 'mvn package'
            sh 'cp /var/lib/jenkins/workspace/homework_pipeline/target/hello-1.0.war .'
            sh 'ls -la'
        }
    }
        
    stage ('build docker image and push') {
        steps {
        sh 'docker login --username theoneeagle --password 4540ffda-0107-4954-9b8e-6363af208911'
        sh 'docker build -f Dockerfile -t theoneeagle/java-webapp:1.0 .'
        sh 'docker push theoneeagle/java-webapp:1.0'
        }
    }
        
    stage ('deploy') {
        steps {
            sshagent(['161f9c56-890f-4e7f-a03a-0b321feab323']) {
              sh '''ssh -o StrictHostKeyChecking=no root@84.252.136.114 << EOF
              docker pull theoneeagle/java-webapp:1.0
              docker run --rm --name java-webapp -d -p 8081:8080 theoneeagle/java-webapp:1.0
EOF'''
            }
        }
    }
}
}
