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
                   git branch: 'master',  url: 'https://github.com/DiveshSrivastava/app_diveshsrivastava.git'
                
                    echo 'Clean solution'
                    bat 'dotnet clean'
                
                    echo 'Restoring packages ..'
                    bat 'dotnet restore'
                }
        }
        stage('Start sonarqube analysis')
        {
            steps
            {
                echo 'Starting sonar scanning ..'
                bat 'dotnet sonarscanner begin /k:"sonar-diveshsrivastava" /d:sonar.host.url="http://localhost:9000"  /d:sonar.login="sqp_1db22bdf9921f73794fb201d3ac3926457c424c3"'
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
        stage('Test case execution') 
        {
            steps 
            {
                echo 'Executing test cases..'
                bat 'dotnet test'
            }
        }
        stage('Stop sonarqube analysis')
        {
            steps
            {
                echo 'Stopping sonar scanning ..'
                bat 'dotnet sonarscanner end /d:sonar.login="sqp_1db22bdf9921f73794fb201d3ac3926457c424c3"'
            }
        }
        stage('Kubernetes deployments') 
        {
            steps 
            {
                echo 'Creating artifacts ..'
                bat ''' cd nagp-devops-us
                        docker image build -t kalpdeep/i-diveshsrivastava-master:latest . 
                        docker push kalpdeep/i-diveshsrivastava-master:latest '''
						
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