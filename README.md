# DevOpsProject1

To Integrate GIT -- GITHUB -- JENKINS -- DOCKER 

 Project Statement : To Streamline the process of Development, Testing and Deployment of a website on Apache Webserver 
                     using GIT,GITHUB, JENKINS and DOCKER. 
                     
 Assumptions :
 
 * GIT installed on Windows machine (Developer machine).
 * GITHUB Account.
 * Virtual Machine (RHEL8/CENTOS8) up and running.
 * Jenkins and Docker Installed and Configured on VM.
 
  Overview of WorkFlow : 
  
              Developer commits code in his windows machine using GIT then the hook (post-commit*) configured in GIT 
              should push the code to the GITHUB in dev1 branch. 
              
              This should trigger JOB2 which is configured to Poll SCM dev1 branch and hence downloads the code and launches 
              webserver TestServer using docker httpd image.
              
              After Completion of JOB2, JOB3 gets trigerred due to dependency in place, JOB3 executes curl command to test the
              Website response and if status is good then proceeds with merge of dev1 branch with master branch.
              
              Once this completes as master branch has changed this should trigger JOB1 which is configured to Poll SCM
              master branch, downloads the code from GITHUB and launches ProdWebServer using Docker httpd image.
              
              Using ngrok public URL is generated to access it over the internet. 
           
              
              
 
 Step 1: Create Local Repo simplewebapp.
 
        git init simplewebapp
        
 Step 2: Create HTML page say index.html, add it and commit it.
 
        git add index.html
        
        git commit -m "First commit"
        
Step 3: If required make changes to index.html and commit every change to create more versions.

Step 4: Create GITHUB Repo with simplewebapp (do not initialize).

Step 5: Add remote URL as Origin and push the code from git to github.

        git remote add origin https://github.com/VinayKumarNC2019/simplewebapp.git 
        
Step 6: Create Jenkins Item : JOB1  and configure with below details

        * Source Code Management Section:
        
           - Choose git
           - Repositories --> Repository URL https://github.com/VinayKumarNC2019/simplewebapp.git
           - Branch specifier --> */master
          
        * Build Triggers Section:
        
           - Choose PollSCM
           - Schedule  * * * * *  (Which means every minute check Github code for any changes)
           
        * Build Section :
        
           - Choose Execute shell 
           
             copy the  downloaded code to /var/master directory.
             
             sudo cp * /var/master
             
           - Choose Execute shell 
           
              Execute Docker command 
        
              count=$(sudo docker inspect ProdServer | wc -l)  #Just to test Webserver is running.
        
              [[ $count -gt 1 ]] && sudo docker rm -f ProdServer  # if it's running then remove the container

              sudo docker run -d -i -t -p 80:80 -v /var/master:/usr/local/apache2/htdocs --name ProdServer httpd

        * Launch ngrok to get public URL and access from Windows/Mobile.
        
          ./ngrok http 80
        
        
Step 8:   Start working on new features, Developer creates new branch dev1 from the master. (checkout to dev1)

           git checkout -b dev1  # This command both creates the branch and switches to dev1 branch
           
Step 9:   Add additional code and commit.

          git add index.html
          
          git commit -m "Added background color and changed Styling"
          

Step 10:  To create new branch dev1 in GITHUB and to push the code use below command.

          git push --set-upstream origin dev1 

Step 11:  Create Jenkins Item : JOB2  and configure with below details

        * Source Code Management Section:
        
           - Choose git
           - Repositories --> Repository URL https://github.com/VinayKumarNC2019/simplewebapp.git
           - Branch specifier --> */dev1
          
        * Build Triggers Section:
        
           - Choose PollSCM
           - Schedule  * * * * *  (Which means every minute check Github code for any changes)
           
        * Build Section :
        
           - Choose Execute shell 
           
             copy the  downloaded code to /var/master directory.
             
             sudo cp * /var/master/dev1
             
           - Choose Execute shell 
           
              Execute Docker command 
        
              count=$(sudo docker inspect TestServer | wc -l)  #Just to test Webserver is running.
        
              [[ $count -gt 1 ]] && sudo docker rm -f TestServer  # if it's running then remove the container

              sudo docker run -d -i -t -p 90:80 -v /var/master/dev1:/usr/local/apache2/htdocs --name TestServer httpd
           
           
Step 12:  Create Jenkins JOB3

         * Source Code Management Section:
        
           - Choose git
           - Repositories --> Repository URL https://github.com/VinayKumarNC2019/simplewebapp.git
           - Choose Credentials of GITHUB account (Add it in Credentials in dashboard)
           - Branch specifier --> */dev1

         * Build Triggers Section
         
           - Choose "Build after other projects are built"
           - Projects to watch JOB2 
           - Choose "Trigger only if build is stable"
            
         * Build Section:
        
           -  Execute Curl Command to test the response code of Test Webserver webapp. 
              (This is not the best way, will add selenium code if time permits )
        
           -  response_code=$(curl -Is http://192.168.0.102:3000/ | head -1 | awk '{print $2}')

              [[ $response_code -eq 200 ]] && echo "Trigger Merge"
              
              # If Response Code is 200 then it means Website is up and running 
              and hence proceed to Merge dev1 branch in GitHub master branch.
                    
         * Post-build Actions: 
         
           - Choose Git Publisher
              - Choose "Push Only If Build Succeeds"
              - Choose "Merge Results"
              - Branches 
                  - Branch to push : master
                  - Target remote name : origin
                     
          
  Improvements:
  
                * Testing the Website should be performed using Selenium or any other testing framework.
                
                * Instead of using ngrok, leverage any cloud provider VM for public IP.
                
                * Purchase Domain name for the website and map it with the IP of Hosted Machine or Load Balancer machine.
                
                * Instead of using volume for code deployment, we can build the Docker image using Docker File and push it to
                  registry and just launch it. 
               

 
 
