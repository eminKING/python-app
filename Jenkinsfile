pipeline {
    agent any
    environment {
     ANSIBLE_PRIVATE_KEY=credentials('ssh-key')   
    }
    triggergs {
     pollSCM '* * * * *'
    }
    stages {
        stage('Build and Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-emin', passwordVariable: 'psswd', usernameVariable: 'emin')]) {
                 sh "docker build -t emin123456789/python:v$BUILD_ID ."
                 sh "docker push emin123456789/python:v$BUILD_ID"   
                }
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-emin', passwordVariable: 'psswd', usernameVariable: 'emin')]) {
                 sh "ansible-playbook playbook.yml -u ansible --private-key=ANSIBLE_PRIVATE_KEY -e username=$emin -e password=$psswd -e BUILD_ID=$BUILD_ID --become -i inventory"   
                }
            }
        }
    }
}
