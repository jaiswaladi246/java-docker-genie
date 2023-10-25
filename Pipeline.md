# PIPELINE
![diagram-export-10-25-2023-8_51_03-PM](https://github.com/PranitRout07/java-docker-genie/assets/102309095/210c888f-cd9f-46eb-a7ad-b473f64d903f)
# EXPLAINATION
_First code is fetched from the github. Then builds the artifacts and run tests on the java project. After this to find any kind of bugs , duplicate codes or vulnerabilities 
code analysis is done using sonarqube. Then scans each file with help of Trivy. After this docker image is build and then pushed the docker image to the dockerhub account. Then 
the docker image is deployed in port 80. After this BUILD STATUS is sent to the gmail account._
