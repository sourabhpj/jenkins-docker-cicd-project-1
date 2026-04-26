pipeline{
    agent any
    
    stages{
        
        stage('Clone Code'){
            steps{
                git'https://github.com/sourabhpj/yumgit.git'
            }
        }
         stage('Build'){
             steps{
                 echo 'Building application...'
             }
         }
         stage('Test'){
             steps{
                 echo "Running tests..."
             }
         }
         stage('Docker Build'){
             steps{
                 sh 'docker build -t my-app -f Dockerfile .'
             }
         }
         stage('Deploy'){
             steps{
                 sh 'docker rm -f my-app-container || true'
                 sh 'docker rm -f my-app || true'
                 sh 'docker run -d -p 80:80 --name my-app-container my-app'
             }
         }
    }
}
