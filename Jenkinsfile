pipeline{
    agent any
        
    stages{
        stage("Install NPM dependencies"){
            steps{
                sh 'npm install'
            }
            post{
                success{
                    echo "========Install NPM executed successfully========"
                }
                failure{
                    echo "========Install NPM execution failed========"
                }
            }
        }
        stage("Run tests"){
            steps{
                sh 'npm test'
            }
            post{
                success{
                    echo "========Run tests executed successfully========"
                }
                failure{
                    echo "========Run tests execution failed========"
                }
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: '38b3acea-775d-4c9d-901c-869bd296548e',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        docker build -t yoanab/sregci2 .
                        echo "$DOCKER_PASS" | docker login --username "$DOCKER_USER" --password-stdin
                        docker push yoanab/sregci2
                    '''
                }
            }
            post{
                success{
                    echo "========Build and Push Docker Image executed successfully========"
                }
                failure{
                    echo "========Build and Push Docker Image execution failed========"
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    docker pull yoanab/sregci2
                    HOST_PORT=3031 docker compose -f docker-compose.yml up -d
                '''  
            }
            post{
                success{
                    echo "========Deploy executed successfully========"
                }
                failure{
                    echo "========Deploy execution failed========"
                }
            }
        }
    }
    post{
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}