# DevOpsProject1

To Integrate GIT -- GITHUB -- JENKINS -- DOCKER 

 Project Statement : To Streamline the process of Development, Testing and Deployment of a website on Apache Webserver using GIT, GITHUB, JENKINS and DOCKER. 
 
 Assumptions :
 
 * GIT installed on Windows machine (Developer machine).
 * GITHUB Account.
 * Virtual Machine (RHEL8/CENTOS8) up and running.
 * Jenkins and Docker Installed and Configured on VM.
 
 Step 1: Create Local Repo simplewebapp.
 
        git init simplewebapp
        
 Step 2: Create HTML page say index.html, add it and commit it.
 
        git add index.html
        
        git commit -m "First commit"
        
Step 3: If required make changes to index.html and commit every change to create more versions.

Step 4: Create GITHUB Repo with simplewebapp (do not initialize).

Step 5: Add remote URL as Origin and push the code from git to github.

        git remote add origin https://github.com/VinayKumarNC2019/simplewebapp.git 
        
Step 6: Create Jenkins Item : JOB1

        * Configure it to PollSCM Master branch
        * In build section execute shell copy command to copy the code downloaded to /var/master
        * Execute Docker command with mounting /var/master to /var/www/html, also expose the port to access.
        * Launch ngrok to get public URL and access from Windows/Mobile.
        
Step 8:   Start working on new features, Developer creates new branch from the master dev1. (checkout to dev1)

           git checkout -b dev1
           
Step 9:   Add additional code and commit.

          git add index.html
          
          git commit -m "Added background color and changed Styling"
          

Step 10:  To create this new branch in GITHUB, we can switch to master branch 

          git push --all  (This should create new branch dev1 in GitHub)


Step 11:  Create Jenkins JOB2

           * Configure the job to Poll SCM dev1 branch
           * copy the downloaded code from WorkSpace to /var/master/dev1
           * Execute Docker command to launch Webserver with bind mounting from /var/master/dev1 to /var/www/html (This represents Test Server)
           
           
           * If docker is running then remove and launch fresh.
           
           
Step 12:  Create Jenkins JOB3

           * Place dependency on JOB2 (Chaining)
           * Execute Curl Command to test the response code of Test Webserver webapp. (This is not the best way)
           
           curl -Is http://192.168.0.102:3000  | head -1 | awk '{print $2}'
           
           * If Response Code is 200 then it means Website is up and running and hence proceed to Merge dev1 branch in GitHub with                    Master branch.
           
               * Add Behaviour in (SCM section)
               * Post build section use Git Publisher to merge the dev1 branch to master branch.
               

 
 
