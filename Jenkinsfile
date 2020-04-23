node {

    stage('Git pull') {
       // git branch: 'jenkins2-course', 
         //   url: 'https://github.com/AKoponen/solitaire-systemjs-course'
         checkout scm
    }
    stage('Npm install') {
        nodejs('nodeJS') {
            sh 'npm install'
        }
    }
   /* stage('Stash') {
        // stash code & dependencies to expedite subsequent testing
        // and ensure same code & dependencies are used throughout the pipeline
        // stash is a temporary archive
        stash name: 'everything', 
              excludes: 'test-results/**', 
              includes: '**'
    }*/
    /*stage('Run tests') {
        // test with PhantomJS for "fast" "generic" results
        // on windows use: bat 'npm run test-single-run -- --browsers PhantomJS'
        nodejs('nodeJS') {
            sh 'npm run test-single-run -- --browsers Chrome'
        }     
    }*/
    
}

stage ('Deploy') {
    node {
        // write build number to index page so we can see this update
        sh "echo '<h1>${env.BUILD_DISPLAY_NAME}</h1>' >> app/index.html"
        // deploy to a docker container mapped to port 3000
        sh 'docker-compose up -d --build'
        
        notify 'Solitaire deployed'
    }
}
def notify(status){
    emailext (
      to: "mainokset1234@gmail.com",
      subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
    )
}
