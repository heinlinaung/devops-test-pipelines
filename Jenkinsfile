// TaskB Jenkinsfile
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

        stage('Generate Doxygen Config') {
            steps {
                sh "doxygen -g ${DOXYFILE}"
            }
        }

        stage('Adjust Config') {
            steps {
                sh """
                    sed -i.bak 's|^INPUT.*|INPUT = src|' ${DOXYFILE}
                    sed -i.bak 's|^INPUT.*|RECURSIVE = YES|' ${DOXYFILE}
                    sed -i.bak 's|^GENERATE_HTML.*|GENERATE_HTML = YES|' ${DOXYFILE}
                    sed -i.bak 's|^GENERATE_LATEX.*|GENERATE_LATEX = NO|' ${DOXYFILE}
                """
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
