pipeline {
    agent any

    environment {
        PYTHON_GREETINGS_DIR = "python-greetings"
        JS_API_FRAMEWORK_DIR = "course-js-api-framework"
    }

    stages {
        stage('Clone Repositories') {
            steps {
                script {
                    powershell """
                        if (Test-Path ${PYTHON_GREETINGS_DIR}) { Remove-Item -Recurse -Force ${PYTHON_GREETINGS_DIR} }
                        if (Test-Path ${JS_API_FRAMEWORK_DIR}) { Remove-Item -Recurse -Force ${JS_API_FRAMEWORK_DIR} }
                        git clone https://github.com/mtararujs/python-greetings ${PYTHON_GREETINGS_DIR}
                        git clone https://github.com/mtararujs/course-js-api-framework ${JS_API_FRAMEWORK_DIR}
                    """
                }
            }
        }

        stage('Install Python Dependencies') {
            steps {
                script {
                    powershell """
                        Write-Host "Starting dependency installation..."
                        pip install -r ${PYTHON_GREETINGS_DIR}\\requirements.txt
                    """
                }
            }
        }

        stage('Deploy to Dev') {
            steps {
                script {
                    powershell """
                        pm2 delete "greetings-app-dev" -ErrorAction SilentlyContinue
                        pm2 start ${PYTHON_GREETINGS_DIR}\\app.py --name greetings-app-dev -- --port 7001
                    """
                }
            }
        }

        stage('Test on Dev') {
            steps {
                script {
                    powershell """
                        Set-Location ${JS_API_FRAMEWORK_DIR}
                        npm install
                        npm run greetings greetings_dev
                    """
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    powershell """
                        pm2 delete "greetings-app-staging" -ErrorAction SilentlyContinue
                        pm2 start ${PYTHON_GREETINGS_DIR}\\app.py --name greetings-app-staging -- --port 7002
                    """
                }
            }
        }

        stage('Test on Staging') {
            steps {
                script {
                    powershell """
                        Set-Location ${JS_API_FRAMEWORK_DIR}
                        npm install
                        npm run greetings greetings_stg
                    """
                }
            }
        }

        stage('Deploy to Preprod') {
            steps {
                script {
                    powershell """
                        pm2 delete "greetings-app-preprod" -ErrorAction SilentlyContinue
                        pm2 start ${PYTHON_GREETINGS_DIR}\\app.py --name greetings-app-preprod -- --port 7003
                    """
                }
            }
        }

        stage('Test on Preprod') {
            steps {
                script {
                    powershell """
                        Set-Location ${JS_API_FRAMEWORK_DIR}
                        npm install
                        npm run greetings greetings_preprod
                    """
                }
            }
        }

        stage('Deploy to Prod') {
            steps {
                script {
                    powershell """
                        pm2 delete "greetings-app-prod" -ErrorAction SilentlyContinue
                        pm2 start ${PYTHON_GREETINGS_DIR}\\app.py --name greetings-app-prod -- --port 7004
                    """
                }
            }
        }

        stage('Test on Prod') {
            steps {
                script {
                    powershell """
                        Set-Location ${JS_API_FRAMEWORK_DIR}
                        npm install
                        npm run greetings greetings_prod
                    """
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    powershell """
                        pm2 delete "greetings-app-dev" -ErrorAction SilentlyContinue
                        pm2 delete "greetings-app-staging" -ErrorAction SilentlyContinue
                        pm2 delete "greetings-app-preprod" -ErrorAction SilentlyContinue
                        pm2 delete "greetings-app-prod" -ErrorAction SilentlyContinue
                    """
                }
            }
        }
    }
}
