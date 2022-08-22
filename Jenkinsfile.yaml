pipeline 
{
    agent any

    stages 
    {
        stage('Nuget restore') 
        {
            steps 
                {
                   echo 'Cloning repository ..'
                   git branch: 'develop', url: 'https://github.com/DiveshSrivastava/app_diveshsrivastava.git'
                
                    echo 'Restoring packages ..'
                    bat 'dotnet restore'
                }
        }
        stage('Code build') 
        {
            steps 
            {
                echo 'Building solution ..'
                bat 'dotnet build'
            }
        }
        stage('Release artifacts') 
        {
            steps 
            {
                echo 'Publishing artifacts ..'
                bat ''' cd nagp-devops-us
                        docker image build -t kalpdeep/i-diveshsrivastava-develop:latest .
                        docker push kalpdeep/i-diveshsrivastava-develop:latest '''
            }
        }
        
        stage('Kubernetes deployments') 
        {
            steps 
            {
                echo 'K8s deployment  ..'
                bat ''' cd K8s
                        kubectl create -f namespace.yaml 
                        kubectl apply -f configmap.yaml 
                        kubectl apply -f secrets.yaml 
                        kubectl apply -f service.yaml 
                        kubectl apply -f deployment.yaml '''
            }
        }
    }
}