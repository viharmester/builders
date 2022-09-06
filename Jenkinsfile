pipeline {
    agent any

    parameters {
        choice(name: 'BUILD_TOOL', choices: ['MAVEN', 'GRADLE'], description: 'The build tool')
    }
    
    stages {
        stage('Release with Maven') {
            when {
                expression { params.BUILD_TOOL == 'MAVEN' }
            }
            steps {
                echo "maven...!"
                sh 'mvn clean package'
            }
        }
        stage('Release with Gradle') {
            when {
                expression { params.BUILD_TOOL == 'GRADLE' }
            }
            steps {
                echo "gradle...!"
                sh './gradlew --version'
            }
        }
        stage('Deploy to test') {
            steps {
                echo 'Deploy to test'
            }
        }
        stage('Deploy to prod') {
            steps {
                echo 'Deploy to prod'
            }
        }
    }
}
