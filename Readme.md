### Manual Steps to build the application

* Install docker 
* To build the docker image run the below command
```
docker image build -t shaikkhajaibrahim/jenkinsdec23workshop .
```
* Run the sonar qube locally or use sonar cloud
```
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```
* Username and password is admin for sonarqube
* To install Trivy:
        ```
        sudo apt-get install wget apt-transport-https gnupg lsb-release
        wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
        echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
        sudo apt-get update
        sudo apt-get install trivy        
        ```

