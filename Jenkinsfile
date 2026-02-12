pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/sim1574264/GC'
            }
        }

        stage('Build + SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat '''
                        mvnw.cmd -B clean install -DskipTests ^
                          org.sonarsource.scanner.maven:sonar-maven-plugin:3.11.0.3922:sonar ^
                          -Dsonar.projectKey=GC ^
                          -Dsonar.projectName="GC" ^
                          -Dsonar.host.url=http://localhost:9000
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build et analyse terminés avec succès !'
        }
        failure {
            echo 'Échec du build ou de l’analyse.'
        }
    }
}
