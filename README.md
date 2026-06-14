# Jenkins + Git Integration

## Prerequisites

* Git installed
* Jenkins running
* GitHub repository

## Configure Jenkins

1. Open Jenkins: `http://localhost:8080`
2. Create **New Item** → **Pipeline**
3. Under **Pipeline**:

   * Definition: **Pipeline script from SCM**
   * SCM: **Git**
   * Repository URL:

     ```
     https://github.com/<username>/<repo>.git
     ```
   * Branch:

     ```
     */main
     ```
4. Save.

## Create Jenkinsfile

Add a `Jenkinsfile` to your Git repository:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building application'
            }
        }
    }
}
```

## Push Code

```bash
git add .
git commit -m "Add Jenkinsfile"
git push origin main
```

## Run Pipeline

In Jenkins:

* Open the pipeline job
* Click **Build Now**
* Check **Console Output**

## Outcome

Jenkins pulls code from Git and executes the pipeline automatically.
