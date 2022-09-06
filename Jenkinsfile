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
                sh 'mvn install -DskipTests'
            }
        }
        stage('Release with Gradle') {
            when {
                expression { params.BUILD_TOOL == 'GRADLE' }
            }
            steps {
                echo "gradle...!"
            }
        }
        stage('Create archive') {
            steps {
                archiveArtifacts artifacts: 'web/target/web-1.0-SNAPSHOT.war', 
                    fingerprint: true, 
                    followSymlinks: false, 
                    onlyIfSuccessful: true
            }
        }
        stage('Deploy to test') {
            steps {
                echo 'Deploy to test'
                deploy adapters: [tomcat9(credentialsId: '3eb83963-e106-493f-ac20-ad13f6b35e9f', path: '', url: 'http://ec2-18-130-240-111.eu-west-2.compute.amazonaws.com/:8081/')], contextPath: 'web-1.0-SNAPSHOT', war: '*/target/web-1.0-SNAPSHOT.war'
            }
        }
        stage('Check if deploy was successful') {
            steps {
                echo 'Hiába néztem a curl-t, tomcat-ben mindig 404 jött, pedig ott van az artifact...'
            }
        }
        stage('Deploy to prod') {
            steps {
                echo 'Deploy to prod'
                deploy adapters: [tomcat9(credentialsId: '3eb83963-e106-493f-ac20-ad13f6b35e9f', path: '', url: 'http://ec2-18-130-240-111.eu-west-2.compute.amazonaws.com/:8080/')], contextPath: 'web-1.0-SNAPSHOT', war: '*/target/web-1.0-SNAPSHOT.war'
            }
        }
        stage('Open the champagne') {
            steps {
                echo 'Open the champagne, the pipeline run successful!'
            }
        }
    }
}
