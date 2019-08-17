pipeline {
     agent any
     
    parameters{
     string(name:'APPLICATION_PATH',defaultValue:'webapi.sln')
 
     string(name: 'NUGET_REPO', defaultValue: 'https://api.nuget.org/v3/index.json')
     string(name: 'GIT_REPO_PATH', defaultValue: 'https://github.com/tavisca-vjola/web_api.git')
     string(name:'IMAGE_NAME',defaultValue:'sample-webapi',description: 'Enter the image name')
      string(name: 'DOCKER_LOGIN', defaultValue: 'vjmsai', description: 'Enter Login')
       string(name: 'DOCKER_PASSWORD', defaultValue: 'vjmsai', description: 'Enter Password')
       string(name: 'TAG_NAME', defaultValue: 'version', description: 'enter tag name')
         string(name:"DOCKER_REPO_NAME",defaultValue:"webapi")
              
         
         
   
     choice(name: 'JOB', choices:  ['Test' , 'Build', 'Publish'])
    }
    
    
   
       stages {
        
        stage('Build') {
            
            
            steps {
              
                 powershell(script: 'dotnet build ${env:APPLICATION_PATH} -p:Configuration=release -v:n')
                
               
            }
        }
        stage('Test') {
             when
            {
                expression { params.JOB == 'Test'}
            }
            
            
            steps {
                 
                powershell(script: 'dotnet test')
            }
        }
        stage('Publish')
        {
          steps{
                 powershell(script: 'dotnet publish') 
             
            }
            
        }
        //   stage('Archive')
        //{
          //  steps
           // {
            // powershell(script: 'compress-archive webapi/artifacts publish.zip -Update')
             //   archiveArtifacts artifacts: 'publish.zip' 
            //}
        //}
         stage('Docker Creation')
        {
           powershell(script:'docker build -t ${env:IMAGE_NAME} .')
           powershell(script:'docker login -u ${env:DOCKER_LOGIN} -p ${env:DOCKER_PASSWORD}')
           powershell(script:'docker tag ${env:IMAGE_NAME}:latest ${env:DOCKER_LOGIN}/${env:}:${env:TAG_NAME}')
                
        }
        stage(' Docker Image Pushing')
        {
            steps 
            {
              
                powershell(script:'docker push ${env:DOCKER_LOGIN}/${env:DOCKER_REPO_NAME}:${env:TAG_NAME}')
            }
        }
            
           
    }
    
}
