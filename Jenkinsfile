pipeline{
    agent{
        label 'ansible'
    }
    stages{
        stage('checkout git'){
            steps{
                dir('vector-role'){
                    git branch: 'main', credentialsId: 'bcc2cf81k-5935-3fb5-j9a8-mvj8b4704ncm', url: 'git@github.com:GribovMaksim/vector-role.git'
                }
            }
        }
        stage('install dependencies'){
            steps{
                dir('vector-role'){
                    sh 'pip3 install -r requirements.txt'
                }
            }
        }
        stage('run molecule'){
            steps{
                dir('vector-role'){
                    sh 'molecule test'
                }
            }
        }
    }
}
