pipeline {
    agent any

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '8'))
        disableConcurrentBuilds()
    }


    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[credentialsId: 'PRUDHVI-GIT', url: 'https://github.com/Prudhvi0103/Project-Prime-O.git']]
                )
            }        }

        stage('Validate HTML') {
            steps {
                sh '''
                    # tidy install cheyali agent meeda if not present: sudo yum install tidy -y or apt install tidy
                    tidy -q -e index.html || true   # errors print chestundi, build fail avvadhu
                '''
            }
        }

        stage('Archive Static Files') {
            steps {
                archiveArtifacts(
                    artifacts: 'index.html, style.css',
                    allowEmptyArchive: false,
                    fingerprint: true
                )
            }
        }

        // Optional: Uncomment if you want auto-deploy to GitHub Pages
        /*
        stage('Deploy to GitHub Pages') {
            when { branch 'main' }
            steps {
                sshagent(credentials: ['github-ssh-deploy-key']) {  // Add SSH key credential in Jenkins
                    sh '''
                        git config --global user.name "Jenkins CI"
                        git config --global user.email "jenkins@example.com"

                        # gh-pages branch setup if not exists
                        git fetch origin || true
                        git checkout main

                        rm -rf ../gh-pages-temp || true
                        git worktree add ../gh-pages-temp gh-pages || git worktree add ../gh-pages-temp -f gh-pages

                        rsync -av --delete \
                            --exclude='.git' \
                            --exclude='.gitignore' \
                            --exclude='Jenkinsfile' \
                            ./ ../gh-pages-temp/

                        cd ../gh-pages-temp
                        git add -A .
                        git commit -m "Auto-deploy from Jenkins build ${BUILD_NUMBER} (${GIT_COMMIT})" || echo "No changes to commit"
                        git push origin gh-pages

                        git worktree remove ../gh-pages-temp -f
                    '''
                }
            }
        }
        */
    }

    post {
        always {
            cleanWs()  // workspace clean cheyadam good practice
        }
        success {
            echo "🎉 JioSaavn clone pipeline success! Files archived."
            // If deploy uncomment cheste: echo "Live at: https://prudhvi0103.github.io/Project-Prime-O/"
        }
        failure {
            echo "❌ Pipeline fail ayindi ra mawa... check console output."
        }
    }
}
