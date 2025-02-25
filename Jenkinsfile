pipeline {
    agent any
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'declarative', description: 'Rama a compilar')
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    def repoDir = 'mi-proyecto'
                    if (fileExists(repoDir)) {
                        echo "Eliminando carpeta existente: ${repoDir}"
                        sh "rm -rf ${repoDir}"
                    }
                    echo "Clonando la rama ${params.BRANCH_NAME}"
                    sh "git clone --branch ${params.BRANCH_NAME} https://github.com/usuario/repositorio.git ${repoDir}"
                }
            }
        }
        stage('Build') {
            steps {
                dir('mi-proyecto') {
                    sh 'mvn -B -DskipTests clean package'
                }
            }
        }
        stage('Test') {
            steps {
                dir('mi-proyecto') {
                    sh 'mvn test'
                }
            }
            post {
                always {
                    junit 'mi-proyecto/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                dir('mi-proyecto') {
                    sh './jenkins/scripts/deliver.sh'
                }
            }
        }
    }
}
