pipeline {
    agent any

    environment {
        PYTHON_GREETINGS_DIR = "python-greetings"
        JS_API_FRAMEWORK_DIR = "course-js-api-framework"
    }

    stages {
        stage('Clone Repositories') {
            steps {
                bat """
                    if exist ${PYTHON_GREETINGS_DIR} rmdir /s /q ${PYTHON_GREETINGS_DIR}
                    if exist ${JS_API_FRAMEWORK_DIR} rmdir /s /q ${JS_API_FRAMEWORK_DIR}
                    git clone https://github.com/mtararujs/python-greetings ${PYTHON_GREETINGS_DIR}
                    git clone https://github.com/mtararujs/course-js-api-framework ${JS_API_FRAMEWORK_DIR}
                """
            }
        }

        stage('Install Python Dependencies') {
            steps {
                bat """
                    echo Starting dependency installation...
                    python -m pip install -r .\\${PYTHON_GREETINGS_DIR}\\requirements.txt
                """
            }
        }

        stage('Deploy to Dev') {
            steps {
                bat """
                    npx pm2 delete greetings-app-dev || exit 0
                    npx pm2 start ${PYTHON_GREETINGS_DIR}\\app.py --name greetings-app-dev -- --port 7001
                """
            }
        }

        stage('Test on Dev') {
            steps {
                bat """
                    cd ${JS_API_FRAMEWORK_DIR}
                    npm install
                    npm run greetings greetings_dev
                """
            }
        }

        stage('Deploy to Staging') {
            steps {
                bat """
                    pm2 delete greetings-app-staging || echo Service not found, continuing...
                    pm2 start ${PYTHON_GREETINGS_DIR}\\app.py --name greetings-app-staging -- --port 7002
                """
            }
        }

        stage('Test on Staging') {
            steps {
                bat """
                    cd ${JS_API_FRAMEWORK_DIR}
                    npm install
                    npm run greetings greetings_stg
                """
            }
        }

        stage('Deploy to Preprod') {
            steps {
                bat """
                    pm2 delete greetings-app-preprod || echo Service not found, continuing...
                    pm2 start ${PYTHON_GREETINGS_DIR}\\app.py --name greetings-app-preprod -- --port 7003
                """
            }
        }

        stage('Test on Preprod') {
            steps {
                bat """
                    cd ${JS_API_FRAMEWORK_DIR}
                    npm install
                    npm run greetings greetings_preprod
                """
            }
        }

        stage('Deploy to Prod') {
            steps {
                bat """
                    pm2 delete greetings-app-prod || echo Service not found, continuing...
                    pm2 start ${PYTHON_GREETINGS_DIR}\\app.py --name greetings-app-prod -- --port 7004
                """
            }
        }

        stage('Test on Prod') {
            steps {
                bat """
                    cd ${JS_API_FRAMEWORK_DIR}
                    npm install
                    npm run greetings greetings_prod
                """
            }
        }

        stage('Cleanup') {
            steps {
                bat """
                    pm2 delete greetings-app-dev || echo Service not found, continuing...
                    pm2 delete greetings-app-staging || echo Service not found, continuing...
                    pm2 delete greetings-app-preprod || echo Service not found, continuing...
                    pm2 delete greetings-app-prod || echo Service not found, continuing...
                """
            }
        }
    }
}
