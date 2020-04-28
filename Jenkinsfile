



import java.text.SimpleDateFormat
def Version = null
def FileName = null
def buildagent = 'master'
def enableDeploy = 'true'
// Build Properties (num builds to keep, polling, blocking, etc)
def projectProperties = [
    [$class: 'BuildDiscarderProperty',strategy: [$class: 'LogRotator', artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '15']],
    disableConcurrentBuilds(),
    ]

properties(projectProperties)

try
{
    timestamps
    {
        node(buildagent)
        {
            // stage('Set Version')
            // {
            //     //Setting the version based on the branch type
            //     def dateFormat = new SimpleDateFormat("MM.dd")
            //     def date = new Date()
            //     def dateString = dateFormat.format(date)
            //     def trimCharacters = null
            //     def versionString = null
            //     def prefix = ''
            //     currentBuild.result = 'SUCCESS'

            //     switch(BRANCH_NAME)
            //     {
            //         case ~/develop/:
            //             trimCharacters = 'develop/'
            //             versionString = dateString+'.${BUILD_NUMBER}'
            //             prefix = BRANCH_NAME
            //             break;
            //         case ~/devops/:
            //             trimCharacters = 'devops/'
            //             versionString = dateString+'.${BUILD_NUMBER}'
            //             prefix = BRANCH_NAME
            //             break;
            //         default:
            //             trimCharacters = ''
            //             versionString = '${BUILD_MONTH}.${BUILD_DAY}.${BUILD_NUMBER}'
            //             prefix = ""
            //             break;
            //     }
            //     //prefix = powershell returnStdout: true, script: "\"${prefix}\".trim('${trimCharacters}/')"
            //     //prefix = prefix.trim()
            //     Version = VersionNumber versionNumberString: "$versionString", versionPrefix: "$prefix."
            //     FileName = VersionNumber versionNumberString: "$versionString", versionPrefix: ""
            //     //The display name needs to always be the version, the deployment jobs depend on this, so don't change it.
            //     currentBuild.displayName = Version
            // }

            // //pull the source code from git tfs.
            stage('check out')
            {
            //     // delete the existing source code to cleanup the workspace.
            //     deleteDir()

                // checkout the code from git tfs to the workspace.
                checkout scm
            }
            stage('Docker Building')
            {
                //   dir('C://Program Files (x86)//Jenkins//workspace//Empmgmt_CI')
                //  {
                     bat 'docker build -t venky1822/angular8-example-app:7.0.2 .'
                //  }
            }


            stage('Docker login')
            {
                withCredentials([string(credentialsId: 'dkrpwd', variable: 'DkrPwd')]) {
                    bat "docker login -u venky1822 -p Venky1822"
                }
                bat 'docker push venky1822/angular8-example-app:7.0.2'
            }

            stage('docker run')
            {
                // def dockrRun = 'docker run -p 8080:80 -d --name my-app venky1822/angular8-example-app:7.0.0'
                // sshagent(['ec2']) {
                bat 'docker stop newcontainer1'
                bat 'docker rm newcontainer1'
                bat "docker run  --name newcontainer1 -d -P venky1822/angular8-example-app:7.0.2"
                // }
            }


            // stage('docker run')
            // {
            //     bat 'docker run -p 8083:80 venky1822/angular8-example-app:7.0.0'
            // }
            // stage('docker stop')
            // {
            //     bat 'docker stop venky1822/angular8-example-app:7.0.0'
            // }
            stage('port num')
            {
                def port= bat 'docker port newcontainer1'
            }

//






            // stage('Running Docker')
            // {
            //     // dir('C://Program Files (x86)//Jenkins//workspace//Empmgmt_CI//node_modules//.bin')
            //     // {
            //         bat 'docker run -it -p 8083:80 angular8-example-app'
            //     // }
            // }

            // def zipFileName="$FileName"+".zip"
            // def MSTestParam = tool 'VSTestTools_MSTest'
            // // stage('Unit Tests')
            // // {

            // // }
            // stage('zipping artifacts')
            // {
            //     //zipping artifacts
            //     zip dir: 'EFileDiagnostics/Service/src/TRTA.Diagnostics.RuleEngine.Service/bin/Debug/netcoreapp2.1/', glob: '', zipFile: zipFileName

            //     // // create a Version folder
            //     // powershell "New-Item -Path $WORKSPACE\\$Version -ItemType directory"

            //     // //pushing zip into Version folder
            //     // powershell "Copy-Item -Path $WORKSPACE\\*.zip $WORKSPACE\\$Version"
            // }

            // stage('Push to TRArtifactory')
            // {
            //     artifactory.pushPackageToTRArtifactory("$Version", "generic-local/CTT/GST/BRMS/$BRANCH_NAME", "$BRANCH_NAME", "*.zip")
            // }
        }
    }
}
catch (e){
    echo e.getMessage()
    currentBuild.result = 'FAILURE'
    throw e
}
