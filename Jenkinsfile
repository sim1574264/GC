pipeline {
    agent any

    triggers { githubPush() }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/sim1574264/cargo-tracker-UM6P1.git'
            }
        }

        stage('Build + SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat """
                        mvn -B clean verify ^
                          org.sonarsource.scanner.maven:sonar-maven-plugin:3.11.0.3922:sonar ^
                          -Dsonar.host.url=https://sonarcloud.io ^
                          -Dsonar.organization=minssin
 ^
                          -Dsonar.projectKey=minssin-GC ^
                          -Dsonar.projectName="GC"
                    """
                }
            }
        }

        // Optionnel: faire échouer le pipeline si Quality Gate KO
        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success { echo 'Build et analyse SonarCloud OK !' }
        failure { echo 'Échec du build / tests / analyse.' }
    }
}
