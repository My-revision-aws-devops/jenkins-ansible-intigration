pipeline{
    agent{
        label "ansible"
    }
    stages{
        stage("vcs"){
            steps{
                git url: "https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git",
                    branch: "main"
            }
            post{
                success{
                    echo "========A executed successfully========"
                }
                failure{
                    echo "========A execution failed========"
                }
            }
        }
        stage("executing Ansible play book"){
            steps{
                sh "ansible-playbook -i hosts apache-playbook.yaml"
            }
            post{
                success{
                    echo "========Playbook executed successfully========"
                }
                failure{
                    echo "========PlayBook execution failed========"
                }
            }
        }
    }
    post{
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}