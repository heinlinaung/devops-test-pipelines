// TaskB and TaskC Jenkinsfile
pipeline {
    agent any

    environment {
        REPO_A_URL = 'https://github.com/heinlinaung/devops-test-repo-A.git'
        REPO_A_BRANCH = 'master'
        REPO_C_URL = 'https://github.com/heinlinaung/devops-test-repo-C.git'
        REPO_C_BRANCH = 'main'
        DOXYFILE = 'Doxyfile'
        LOG_FILE_NAME = 'warnings.log'
    }

    stages {
        stage('Checkout RepoA') {
            steps {
                git url: "${env.REPO_A_URL}", branch: "${env.REPO_A_BRANCH}"
            }
        }

        stage('Generate Doxygen Config') {
            steps {
                sh "doxygen -g ${DOXYFILE}"
            }
        }

        stage('Adjust Config') {
            steps {
                sh """
                    sed -i.bak 's|^INPUT.*|INPUT = src|' ${DOXYFILE}
                    sed -i.bak 's|^RECURSIVE.*|RECURSIVE = YES|' ${DOXYFILE}
                    sed -i.bak 's|^GENERATE_HTML.*|GENERATE_HTML = YES|' ${DOXYFILE}
                    sed -i.bak 's|^GENERATE_LATEX.*|GENERATE_LATEX = NO|' ${DOXYFILE}
                    sed -i.bak 's|^WARN_LOGFILE.*|WARN_LOGFILE = ${LOG_FILE_NAME}|' ${DOXYFILE}
                """
            }
        }

        stage('Run Doxygen') {
            steps {
                sh "doxygen ${DOXYFILE}"
            }
        }

        // --- TaskC ---
        stage('Clone RepoC (Python)') {
            steps {
                dir('repoC') {
                    git url: "${env.REPO_C_URL}", branch: "${env.REPO_C_BRANCH}"
                }
            }
        }

        stage('Run log_parser.py') {
            steps {
                dir('repoC') {
                    sh "python3 check.py ../${LOG_FILE_NAME}"
                    archiveArtifacts artifacts: 'output.csv'
                }
            }
        }
    }
}
