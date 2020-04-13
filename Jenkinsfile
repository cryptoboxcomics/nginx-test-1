pipeline {
    agent {
        label "generic"
    } //agent
    stages {

           stage ("Set up") {
               steps {
                    sh """
                     python3 -m venv env
                    source ./env/bin/activate 
                    python -m pip install molecule
                    python -m pip install docker
                """
               }//steps
           }//stage
            stage ("starting deamon and enable") {
               steps {
                   sh """
                   sudo systemctl start docker
                   sudo systemctl enable docker
                   sudo systemctl restart docker
                   """
           
            } //steps
        } //stage


        stage("Create docker image for testing") {
            steps {
                sh """
                    LC_ALL=en_US
                    export LC_ALL
                    source ./env/bin/activate 
                    python3 -m molecule create
                """
            } //steps
        } //stage
        stage("Apply Ansible role to docker image") {
            steps {
                sh """
                    LC_ALL=en_US
                    export LC_ALL
                    source ./env/bin/activate 
                    python3 -m molecule converge
                """
            } //steps
        } //stage
        stage("Check idempotency") {
            steps {
                sh """
                    source ./env/bin/activate 
                    python3 -m molecule idempotence
                """
            } //steps
        } //stage
        stage("Cleanup molecule") {
            steps {
                sh """
                    source ./env/bin/activate 
                    python3 -m molecule cleanup
                """
            } //steps
        } //stage
        stage("Destroy molecule instance") {
            steps {
                sh """
                    source ./env/bin/activate 
                    python3 -m molecule destroy
                """
            } //steps
        } //stage
    } //stages
    post {
        always {
            sh """
                rm -rf env
            """
        }
    }
} //pipeline
