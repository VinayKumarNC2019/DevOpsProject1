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
Step 7:   Above steps is for Creating Stable Production Server Webapp accessible to the clients.
---------------------------------------------------------------------------------------------------------------
Step 8:   Start working on new features, Developer creates new branch from the master dev1. (checkout to dev1)
Step 9:   Add additional code and commit.
Step 10:  git push to GitHub (This should create new branch in GitHub)
Step 11:  Create Jenkins JOB2
           * Configure POLLSCM dev1 branch
           * copy the code to /var/dev1
           * Execute Docker command to launch Webserver with bind mounting from /var/dev1 to /var/www/html (This represents Test Server)
           * If docker is running then remove and launch fresh.
Step 12:  Create Jenkins JOB3
           * Place dependency on JOB2 (Chaining)
           * Execute Curl Command to test the response code of Test Webserver webapp. (This is not the right way, we may have to use robust testing may be using selenium etc)
           * If Response Code is 200 then Merge dev1 branch in GitHub with Master else do nothing. (Use Git Publisher in Post build Action)
               * Add Behaviour (SCM section)
               * Git Publisher (Post build section)

 
 
