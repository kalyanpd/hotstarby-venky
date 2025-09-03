pipeline {
    agent any
	
	environment {
        DOCKERHUB_USER = 'kalyan3599'
        DOCKERHUB_REPO = 'hotstar'
        DOCKERHUB_TAG  = 'v1'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from main branch
                git branch: 'main', url: 'https://github.com/anithavalluri02/hotstarby.git'

                // Verify files
                sh 'pwd'
                sh 'ls -l'
                sh 'ls -R'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker rmi -f hotstar:v1 || true
                    docker build -t hotstar:v1 -f /var/lib/jenkins/workspace/hotstar/Dockerfile /var/lib/jenkins/workspace/hotstar
                '''
            }
        }

	stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                        docker push $DOCKERHUB_USER/$DOCKERHUB_REPO:$DOCKERHUB_TAG
                        docker logout
                    '''
                }
            }
        }
        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f con8 || true
                    docker run -d --name con8 -p 8008:8080 hotstar:v1
                '''
            }
        }

	stage('Docker Swarm Deploy') {
    steps {
        sh '''
            docker service update --image hotstar:v1 hotstarserv || \
            docker service create --name hotstarserv -p 8008:8080 --replicas=10 hotstar:v1
        '''
    
            }
        }
    }
}
