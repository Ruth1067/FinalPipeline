pipeline {
    agent {label 'verisoft-2'}

    parameters {
        string(name: 'REPO_URL', defaultValue: 'https://github.com/Ruth1067/FinalPipeline.git', description: 'Repository URL')
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch name')
    }

    environment {
        MAIN_BRANCH = 'main'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo "Starting code checkout..."

                    if (params.BRANCH_NAME == env.MAIN_BRANCH) {
                        checkout scm
                    } else {
                        git branch: "${params.BRANCH_NAME}", url: "${params.REPO_URL}"
                    }

                    echo "Code checkout completed"
                }
            }
        }

        stage('Compile') {
            steps {
                echo 'Starting compilation stage'
                timeout(time: 5, unit: 'MINUTES'){
                    sh returnStatus:true,script:'mvn compile'
                }
                echo 'Compilation stage completed successfully'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Starting test stage'
                timeout(time: 5, unit: 'MINUTES'){
                    sh returnStatus:true,script:'mvn test'
                }
                echo 'Test stage completed successfully'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }

    triggers {
        cron('30 5 * * 1\n0 14 * * *')
    }
}

