// Evan Price, 2021-12-08, pipeline for EntSysInt final

pipeline {
  agent any
  parameters {
    string(name: 'TARGET', defaultValue: '', description: "Run Target Scripts")
  }
    stage ('Build') {
      steps {
        echo 'Running The Requirements...'
        sh 'pip install -r requirements.txt'
        echo 'Requirements Complete!'
      }
      post {
        always {
          echo 'A00855286 Group 42'
        }
      }
    }
    stage ('Code Quality') {
      steps {
        sh 'pylint-fail-under --fail_under 7.0 *.py'
      }
    }
    stage ('Code Quantity') {
      steps {
        script {
          def files = findFiles(glob: '*.py')
          for (file in files) {
            sh "wc -l ${file}"
          }
        }
      }
    }
    stage ('Run Scripts') {
      when {
        expression {
          params.TARGET == "run"
        }
      }
      steps {
        python 'main.py phone text output'
        python 'main.py tablet csv output'
        python 'main.py laptop json output'
      }
    }
    stage ('Zip') {
      steps {
        sh 'zip app.zip *.py'
      }
      post {
        always {
          archiveArtifacts artifacts: 'app.zip', fingerprint: true
        }
      }
    }
  }
}
