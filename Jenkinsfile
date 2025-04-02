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
                    sh 'git clone https://github.com/mtararujs/python-greetings ${PYTHON_GREETINGS_DIR}'
                    sh 'git clone https://github.com/mtararujs/course-js-api-framework ${JS_API_FRAMEWORK_DIR}'
                }
            }
        }

        stage('Install Python Dependencies') {
            steps {
                script {
                    sh """
                        echo "Starting dependency installation..."
                        pip3 install -r ${PYTHON_GREETINGS_DIR}/requirements.txt
                    """
                }
            }
        }

        stage('Deploy to Dev') {
            steps {
                script {
                    sh """
                        pm2 delete "greetings-app-dev" || echo "Service not found, continuing..."
                        pm2 start -f ${PYTHON_GREETINGS_DIR}/app.py --name greetings-app-dev -- --port 7001
                    """
                }
            }
        }

        stage('Test on Dev') {
            steps {
                script {
                    sh """
                        cd ${JS_API_FRAMEWORK_DIR}
                        npm install
                        npm run greetings greetings_dev
                    """
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    sh """
                        pm2 delete "greetings-app-staging" || echo "Service not found, continuing..."
                        pm2 start -f ${PYTHON_GREETINGS_DIR}/app.py --name greetings-app-staging -- --port 7002
                    """
                }
            }
        }

        stage('Test on Staging') {
            steps {
                script {
                    sh """
                        cd ${JS_API_FRAMEWORK_DIR}
                        npm install
                        npm run greetings greetings_stg
                    """
                }
            }
        }

        stage('Deploy to Preprod') {
            steps {
                script {
                    sh """
                        pm2 delete "greetings-app-preprod" || echo "Service not found, continuing..."
                        pm2 start -f ${PYTHON_GREETINGS_DIR}/app.py --name greetings-app-preprod -- --port 7003
                    """
                }
            }
        }

        stage('Test on Preprod') {
            steps {
                script {
                    sh """
                        cd ${JS_API_FRAMEWORK_DIR}
                        npm install
                        npm run greetings greetings_preprod
                    """
                }
            }
        }

        stage('Deploy to Prod') {
            steps {
                script {
                    sh """
                        pm2 delete "greetings-app-prod" || echo "Service not found, continuing..."
                        pm2 start -f ${PYTHON_GREETINGS_DIR}/app.py --name greetings-app-prod -- --port 7004
                    """
                }
            }
        }

        stage('Test on Prod') {
            steps {
                script {
                    sh """
                        cd ${JS_API_FRAMEWORK_DIR}
                        npm install
                        npm run greetings greetings_prod
                    """
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh """
                        pm2 delete "greetings-app-dev" || true
                        pm2 delete "greetings-app-staging" || true
                        pm2 delete "greetings-app-preprod" || true
                        pm2 delete "greetings-app-prod" || true
                    """
                }
            }
        }
    }
}
