pipeline {
    agent any

    parameters {
        string(name: 'REPO_URL', defaultValue: 'https://github.com/Ruth1067/FinalPipeline.git', description: 'Repository URL')
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch name')
    }

    environment {
        MAIN_BRANCH = 'main'  // משתנה סביבה שמייצג את הבראנצ' המרכזי
    }

    triggers {
        cron('TZ=Asia/Jerusalem 30 5 * * 1\nTZ=Asia/Jerusalem 0 14 * * 3')
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo "Starting code checkout..."

                    // אם הבראנצ' מהפרמטרים תואם את ברירת המחדל – נשתמש ב-checkout scm
                    if (env.BRANCH_NAME == env.MAIN_BRANCH) {
                        checkout scm
                    } else {
                        // אחרת – נוריד ידנית לפי ה-URL והבראנצ'
                        git branch: params.BRANCH_NAME, url: params.REPO_URL
                    }

                    echo "Code checkout completed"
                }
            }
        }

        stage('Compile') {
            options {
                timeout(time: 5, unit: 'MINUTES')
            }
            steps {
                echo 'Starting compilation stage'
                sh 'mvn compile'
                echo 'Compilation stage completed successfully'
            }
        }

        stage('Run Tests') {
            options {
                timeout(time: 5, unit: 'MINUTES')
            }
            steps {
                echo 'Starting test stage'
                sh 'mvn test'
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
}
