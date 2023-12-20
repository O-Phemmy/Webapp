pipeline{
    agent any 
    options{
        buildDiscarder(logRotator(numToKeepStr:'5'))
    }
    environment{
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages{
        stage('Build docker image'){
            steps{
                echo "Building the docker image from Dockerfile in src directory"
                sh 'docker build -t fjbodun/jenkins_demo .'
            }
        }
        stage('Login into the dockerhub account'){
            steps{
                echo "Logging into dockerhub account for pushing the image"
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }

        }
        stage('Push docker image on dockerhub online'){
            steps{
                echo "Execute the pushing command for image onto dockerhub"
                sh "docker push fjbodun/jenkins_demo"
            }
        }
        stage('Run the docker image on the EC2 instance'){
            steps{
                echo "Running the docker image on EC2 instance"
                sh "docker run -d -p 9900:3000 fjbodun/jenkins_demo"
            }
        }
    }
}
