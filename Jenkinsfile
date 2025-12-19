pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh '''
                echo "This is a multi-line shell script"
                echo "It can contain multiple commands"
                '''
            }
        }
    }
}
