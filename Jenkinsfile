pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'git@github.com:Priscilla0506/maven-project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to Cloud') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'aws-key', keyFileVariable: 'KEYFILE', usernameVariable: 'USER')]) {
                    sh '''
                    scp -o StrictHostKeyChecking=no -i $KEYFILE target/*.jar $USER@13.235.21.54:/home/ec2-user/app.jar
                    ssh -o StrictHostKeyChecking=no -i $KEYFILE $USER@13.235.21.54 "nohup java -jar /home/ec2-user/app.jar > output.log 2>&1 &"
                    '''
                }
            }
        }
    }
}    }
}
