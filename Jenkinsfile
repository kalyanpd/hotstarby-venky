pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from main branch
                git branch: 'main', url: 'https://github.com/kalyanpd/hotstarby-venky.git'

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
                    docker rmi -f hotstor:v1 || true
<<<<<<< HEAD
                    docker build -t hotstor:v1 -f /var/lib/jenkins/workspace/hoststar_by_venky/Dockerfile /var/lib/jenkins/workspace/hotstor
=======
                    docker build -t hotstor:v1 -f /var/lib/jenkins/workspace/hotstor/Dockerfile /var/lib/jenkins/workspace/hotstor
>>>>>>> 68e23edc88186f36288b1e948648a484ac682288
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f con8 || true
<<<<<<< HEAD
                    docker run -d --name con8 -p 8083:8080 hotstor:v1
=======
                    docker run -d --name con8 -p 8008:8080 hotstor:v1
>>>>>>> 68e23edc88186f36288b1e948648a484ac682288
                '''
            }
        }

        stage('Docker Swarm Deploy') {
            steps {
                sh '''
                    docker service update --image hotstor:v1 hotstarserv || \
                    docker service create --name hotstarserv -p 8009:8080 --replicas=10 hotstor:v1
                '''
            }
        }
    }
}
