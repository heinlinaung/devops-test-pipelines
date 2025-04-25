## TaskB Jenkenfile
pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/heinlinaung/devops-test-repo-A.git'
        REPO_BRANCH = 'master'
        DOXYFILE = 'Doxyfile'
    }

    stages {
        stage('Checkout RepoA') {
            steps {
                git url: "${env.REPO_URL}", branch: "${env.REPO_BRANCH}"
            }
        }

        stage('Install Doxygen') {
            steps {
                sh '''
                    if ! command -v doxygen &> /dev/null; then
                        brew install doxygen
                    fi
                '''
            }
        }

        stage('Generate Doxygen Config') {
            steps {
                sh "doxygen -g ${DOXYFILE}"
            }
        }

        stage('Adjust Config') {
            steps {
                script {
                    sh """
                        sed -i 's|^INPUT.*|INPUT = src|' ${DOXYFILE}
                        sed -i 's|^GENERATE_HTML.*|GENERATE_HTML = YES|' ${DOXYFILE}
                        sed -i 's|^GENERATE_LATEX.*|GENERATE_LATEX = NO|' ${DOXYFILE}
                    """
                }
            }
        }

        stage('Run Doxygen') {
            steps {
                sh "doxygen ${DOXYFILE}"
            }
        }

        stage('Package HTML Docs') {
            steps {
                sh 'tar -czf doc.tar.gz html/'
                archiveArtifacts artifacts: 'doc.tar.gz'
            }
        }
    }
}
