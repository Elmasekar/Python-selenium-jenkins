# How to integrate Jenkins pipeline for Python-selenium on [LambdaTest](https://www.lambdatest.com/?utm_source=github&utm_medium=repo&utm_campaign=Python-selenium-jenkins)

Jenkins Pipeline is also referred to as "Pipeline" offers a suite of plugins to help integrate your continuous delivery pipeline into Jenkins. Jenkins Pipeline does so with the help of Pipeline DSL(Domain Specific Language) syntax that facilitates easy modelling of even the most complex delivery pipeline. 

You can easily create a Jenkins pipeline for Python-selenium automation tests on LambdaTest using the following steps. You can refer to sample test repo [here](https://github.com/LambdaTest/python-selenium-sample).

## Prerequisites For Configuring Jenkins Pipeline With LambdaTest

1.  Jenkins 2.X or greater version.
2.  A Jenkins User with root access.
3.  Ensure you have the Pipeline plugin, although, it is displayed under the "suggested plugins" during the post-installation setup of Jenkins.
4.  **LambdaTest Authentication Credentials**
Be aware of your LambdaTest authentication credentials i.e. your LambdaTest username, access key and HubURL. You need to set them up as your environment variables. You can retrieve them from your  **[LambdaTest automation dashboard](https://automation.lambdatest.com/)**  by clicking on the key icon near the help button.

-   For Linux/Mac:
    
    ```
    $ export LT_USERNAME= {YOUR_LAMBDATEST_USERNAME}$ export LT_ACCESS_KEY= {YOUR_LAMBDATEST_ACCESS_KEY}
    ```
    
-   For Windows:
    
    ```
    $ set LT_USERNAME= {YOUR_LAMBDATEST_USERNAME}$ set LT_ACCESS_KEY= {YOUR_LAMBDATEST_ACCESS_KEY}
    ```
## Setting Up Jenkins Pipeline

Find the code for setting up a pipeline for the sample Python-selenium repo.
```bash
pipline 
{
    withEnv(["LT_USERNAME=Your LambdaTest UserName",
    "LT_ACCESS_KEY=Your LambdaTest Access Key",
    "LT_TUNNEL=true"]){

    echo env.LT_USERNAME
    echo env.LT_ACCESS_KEY 

    stages{
        stage('setup') { 

            // Get some code from a GitHub repository
            try{
            git 'https://github.com/LambdaTest/python-selenium-sample'

            //Download Tunnel Binary
            sh "wget https://s3.amazonaws.com/lambda-tunnel/LT_Linux.zip"

            //Required if unzip is not installed
            sh 'sudo apt-get install --no-act unzip'
            sh 'unzip -o LT_Linux.zip'

            //Starting Tunnel Process 
            sh "./LT -user ${env.LT_USERNAME} -key ${env.LT_ACCESS_KEY} &"
            sh  "rm -rf LT_Linux.zip"
            }
            catch (err){
            echo err
        }

        }
        stage('build') {
            // Installing Dependencies
            sh 'pip install -r requirements.txt'
            }

        stage('test') {
                try{
                sh 'python lambdatest.py'
                }
                catch (err){
                echo err
                }  
        }
        stage('end') {  
            echo "Success" 
            }
        }
    }
}

```
You can now add this script when creating the pipeline by using the following steps:

1. Create new pipleline
2. Scroll down to Advanced Project Options.
3.  Paste the Code in the code pane or fetch it via SCM & hit the **Save** button.

**Note:**  To run on the tunnel, Either you can use LT_TUNNEL Environment variable to set the tunnelling capability or you can pass in the code. Instructions on the tunnel are are available in the sample repo readme.



# Links:

[LambdaTest Community](http://community.lambdatest.com/)

