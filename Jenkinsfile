pipeline {
    agent any

    environment {
        IMAGE_NAME = 'student-ui'
        CONTAINER_NAME = 'student-ui-container'
    }

    stages {
        // Stage 1: Install Git (if not already installed)
        stage('Install Git') {
            steps {
                script {
                    // Check if Git is installed, if not, install it
                    sh '''
                    if ! command -v git &> /dev/null
                    then
                        echo "Git is not installed, installing..."
                        sudo yum install -y git
                    else
                        echo "Git is already installed"
                    fi
                    '''
                }
            }
        }

        // Stage 2: Checkout Code from GitHub
        stage('Checkout Code') {
            steps {
                script {
                    // Clone the GitHub repository
                    git 'https://github.com/Pritam-Khergade/student-ui.git'
                }
            }
        }

        // Stage 3: Install Docker (for CentOS or Amazon Linux 2)
        stage('Install Docker') {
            steps {
                script {
                    sh '''
                    # Check if Docker is already installed
                    if ! command -v docker &> /dev/null
                    then
                        echo "Docker is not installed, installing..."
                        
                        # Update and install dependencies
                        sudo yum update -y
                        sudo yum install -y yum-utils device-mapper-persistent-data lvm2

                        # Install Docker based on the operating system
                        if [ -f /etc/os-release ] && grep -q "Amazon Linux 2" /etc/os-release; then
                            echo "Installing Docker on Amazon Linux 2..."
                            sudo yum install -y docker
                        elif [ -f /etc/os-release ] && grep -q "CentOS" /etc/os-release; then
                            echo "Installing Docker on CentOS..."
                            sudo yum install -y docker-ce docker-ce-cli containerd.io
                        else
                            echo "Operating System not supported for automatic Docker installation."
                            exit 1
                        fi

                        # Start and enable Docker
                        sudo systemctl start docker
                        sudo systemctl enable docker
                        echo "Docker installed and started successfully"
                    else
                        echo "Docker is already installed"
                    fi
                    '''
                }
            }
        }

        // Stage 4: Build Docker Image
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using sudo
                    sh 'sudo docker build -t $IMAGE_NAME .'
                }
            }
        }

        // Stage 5: Echo Stage (Simple echo message)
        stage('Echo Stage') {
            steps {
                echo 'This is a simple echo message after the pipeline stages.'
            }
        }
    }

    post {
        success {
            echo 'Pipeline ran successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

