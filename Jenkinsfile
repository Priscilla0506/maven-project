pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning source code...'
                git 'https://github.com/your-github-username/your-repo-name.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
        }

        stage('Deploy to Cloud') {
            steps {
                echo 'Deploying to AWS EC2...'
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh', keyFileVariable: 'KEYFILE', usernameVariable: 'USER')]) {
                    sh '''
                    scp -o StrictHostKeyChecking=no -i $KEYFILE target/*.jar $USER@<EC2-IP>:/home/ec2-user/app.jar
                    ssh -o StrictHostKeyChecking=no -i $KEYFILE $USER@<EC2-IP> "nohup java -jar /home/ec2-user/app.jar > output.log 2>&1 &"
                    '''
                }
            }
        }
    }
}
