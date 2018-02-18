node {
    def myImage

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        dir('.') {
            git url: 'https://github.com/VireshDoshi/jen-build-node-app-aws.git'
        }
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        myImage = docker.build("vireshdoshi/jen-build-node-app-aws:${env.BUILD_NUMBER}")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image. */

        myImage.inside {
            sh 'echo "Tests passed"'
            sh 'ls -ltr'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag. */
            withCredentials([usernamePassword( credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        
          docker.withRegistry('', 'docker-hub-credentials') {
          sh "docker login -u ${USERNAME} -p ${PASSWORD}"
           myImage.push("${env.BUILD_NUMBER}")
           myImage.push("latest")
         }
            
        }
    }
}
