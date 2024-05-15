pipeline { 
  agent any 
  stages { 
    stage('Build') { 
      parallel { 
        stage('Build') { 
          steps { 
            sh 'echo "building the repo"'
          } 
        } 
      } 
    } 
  
    stage('Test') { 
      steps { 
        sh 'python3 test_app.py'
        input(id: "DeployGate", message: "Deploy ${params.project_name}?", ok: 'Deploy') 
      } 
    } 
  
    stage('Deploy') 
    { 
      steps { 
        echo "deploying the application"
        sh "sudo nohup python3 app.py > log.txt 2>&1 &"
      } 
    } 
    stage('E2E') 
    { 
        steps { 
          echo "e2e checking"
          sh "netstat -zv localhost 5000"
        } 
    } 

  } 
  
  post { 
        always { 
            echo 'The pipeline completed'
            junit allowEmptyResults: true, testResults:'**/test_reports/*.xml'
        } 
        success {                    
            echo "Flask Application Up and running!!"
        } 
        failure { 
            echo 'Build stage failed'
            error('Stopping early…') 
        } 
      } 
} 
