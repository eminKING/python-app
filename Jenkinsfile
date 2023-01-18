pipeline {
           agent any


         stages { stage("TEST") {
             steps {
                  echo "---- Test ----"
           }
        }
        stage("Build") {
             steps {
                  echo "---- Build ----" 
                  sh "cd simple_python_app && docker build -t emin123456789/pipline:$BUILD_ID ."
           }
        }
        stage("Push DockerHUB") { steps { echo "---- Push DockerHUB ---- " withCredentials([usernamePassword(credentialsId: 'dockerhub-emin', 
                passwordVariable: 'password', usernameVariable: 'emin')]) { sh "docker login -u $emin -p $password" sh "docker push 
                emin123456789/pipline:$BUILD_ID"
                }
            }
        }
        stage("Deploy") { steps { echo "---- Deploy ----" sh "docker run -d --name flask-app-$BUILD_ID -p 80$BUILD_ID:8080 
                emin123456789/pipline:$BUILD_ID"
            }
        }
    }
}
