# DEPLOYING PROMETHEUS STACK USING DOCKER COMPOSE

- In our setup, we will be using the following components.

- Prometheus: Prometheus is an open-source monitoring system designed to collect metrics from various sources, such as application and system performance data, and store them in a time-series database. It scrapes metrics from exporters, processes them, and provides this data to visualization tools like Grafana for real-time monitoring and alerting.

- Alert Manager: Alert Manager is a component of Prometheus that handles alerts based on predefined rules. It routes alerts to various notification channels, such as email, Slack, or other messaging platforms, and provides a centralized dashboard for managing and viewing alert statuses, helping teams quickly respond to potential issues.

- Node Exporter: Node Exporter is an agent for Prometheus that collects hardware and OS-level metrics from Linux systems, such as CPU usage, memory consumption, and disk I/O. These metrics are exposed at the /metrics endpoint, allowing Prometheus to scrape and monitor server performance effectively.

- Grafana: Grafana is a powerful data visualization and monitoring tool that integrates with Prometheus to create interactive and customizable dashboards. It supports various data sources and provides a user-friendly interface for visualizing complex data sets, helping teams analyze and understand system performance and trends.

- Terraform: Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp that allows developers to define and provision infrastructure using a high-level configuration language. It supports multiple cloud providers and helps automate the creation, updating, and versioning of infrastructure resources, making infrastructure management efficient and repeatable.

- Docker: Docker is a platform for developing, shipping, and running applications in containers, which are lightweight and portable units that package an application and its dependencies. Docker ensures consistency across different environments, simplifies the deployment process, and isolates applications, making it easier to manage and scale.

- Docker Compose: Docker Compose is a tool for defining and running multi-container Docker applications using a simple YAML file. It allows users to configure application services, networks, and volumes, making it easy to deploy and manage multi-container environments by handling service dependencies and orchestration.


# DOCUMENTATION


## CREATING AND SETTING UP INSTANCE


- Created an Ec2 instance(named terrafrom) with a valid VPC & EC2 FULL ACCESS IAM role attached to it.




![1.0](./img10/1.0.png)




![1.1](./img10/1.1.png)




- SSH into the instance 



- Used the following command to update and upgrade instance


<pre><code>

sudo apt upgrade -y && sudo apt upgrade -y

</code></pre>



- Installed terraform with the command below;

<pre><code>

wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update -y && sudo apt install terraform -y

</code></pre>




- Cloned the project repository to my instance


<pre><code>

git clone https://github.com/TobiOlajumoke/prometheus-observability-stack


</code></pre>



- Installed tree to check cloned project structure with the command below


<pre><code>

sudo apt install tree

</code></pre>



-  cd into the prometheus-observability-stack with the cd command;

<pre><code>

cd prometheus-observability-stack

</code></pre>



- Used the tree command to see the project structure by just typing ***tree***  and press enter



- Here is the structure 



![1.2](./img10/1.2.png)




### Understanding the project files.

- The alertmanager folder contains the alertmanager.yml file which is the configuration file. If you have details of the email, slack, etc. we can update accordingly.

- The prometheus folder contains alertrules.yml which is responsible for the alerts to be triggered from the prometheus to the alert manager. The prometheus.yml config is also mapped to the alert manager endpoint to fetch, Service discovery is used with the help of a file file_sd_configs to scrape the metrics using the targets.json file.

- terraform-aws directory allows you to manage and isolate resources effectively. modules contain the reusable terraform code. These contain the Terraform configuration files (main.tf, outputs.tf, variables.tf) for the respective modules.

- The ec2 module also includes user-data.sh script to bootstrap the EC2 instance with Docker and Docker Compose. The security group module will create all the inbound & outbound rules required.

- Prometheus-stack Contains the configuration file main.tf required for running the terraform. Vars contains an ec2.tfvars file which contains variable values specific to all the files for the terraform project.

- The Makefile is used update the provisioned AWS EC2’s public IP address within the configuration files of prometheus.yml and targets.json located in the prometheus directory.

- The docker-compose.yml file incorporates various services Prometheus, Grafana, Node exporter & Alert Manager. These services are mapped with a network named ‘monitor‘ and have an ‘always‘ restart flag as well.



## Provisioning a new instance server Using Terraform

- Cd into the terraform-aws/vars folder as seen in the project structure of prometheus-observability-stack;


<code><pre>

cd terraform-aws/vars

</pre></code>



- Used the nano command to edit the ec2.tfvars file into my aws console configuration;

<code><pre>

nano ec2.tfvars

</pre></cdoe>



![1.3](./img10/1.3.png)



***NOTE THE ATTENTION TO DETAILS*** Keyname is your exsisting keypair name already downloaded in your system. Check other details like region, Ami, subnet and vpc making sure they all align with ur console configuration.


- Now lets provision the new AWS EC2 instance & Security group using terraform.


- cd back into the prometheus-stack


<pre><code>

cd ../prometheus-stack

</code></pre>


- Used the following terraform commands;



<pre><code>

terraform fmt

</code></pre>




<pre><code>

terraform init

</code></pre>




<pre><code>

terraform validate

</code></pre>




![1.4](./img10/1.4.png)


- Executed the plan and applied the changes


<pre><code>

terraform plan --var-file=../vars/ec2.tfvars

</code></pre>


<pre><code>

terraform apply --var-file=../vars/ec2.tfvars

</code></pre>





![1.5](./img10/1.5.png)



## Successfully created a new instance in my AWS console.

- Went to my console to check




![1.6](./img10/1.6.png)



## CONNECTED TO THE NEW INSTANCE


- Ssh into the new instance


- Used the following command to update and upgrade instance


<pre><code>

sudo apt upgrade -y && sudo apt upgrade -y

</code></pre>


- Ran the following command on new instance checking the cloud-init logs to see if the user data script ran successfully.



<pre><code>

tail /var/log/cloud-init-output.log

</code></pre>


- The output is shown below. Showing Docker and Docker compose versions as highlighted in the image.


![1.7](./img10/1.7.png)



- Verified the docker and docker-compose versions

<pre><code>

sudo docker version


</code></pre>



- Or just use


<pre><code>

docker --version

</code></pre>



<pre><code>

sudo docker-compose version


</code></pre>





![1.8](./img10/1.8.png)



- I now have the new instance ready with the required utilities, I'll be deploying the Prometheus stack using docker-compose.


## Deployed Prometheus Stack Using Docker Compose


- Cloned the project code repository to this new server.


<pre><code>

git clone https://github.com/TobiOlajumoke/prometheus-observability-stack


</code></pre>




- cd into the prometheus-observability-stack


<pre><code>

cd prometheus-observability-stack


</code></pre>



- Executed the ***make*** command to update server IP in prometheus config file. Because I am running the node exporter on the same server to fetch the server metrics. I also updated the alert manager endpoint to the servers public IP address.





<pre><code>

make all


</code></pre>





![1.9](./img10/1.9.png)


## Brought up the stack using Docker Compose. It deployed Prometheus, Alert manager, Node exporter and Grafana


<pre><code>

sudo docker-compose up -d


</code></pre>





![1.10](./img10/1.10.png)



- On successful execution, the following was the output ;




![1.11](./img10/1.11.png)





- Now, with my server's IP address I can access all the apps on different ports.

### Prometheus: http://my-public-ip-address:9090
### Alert Manager: http://my-public-ip-address:9093
### Grafana: http://my-public-ip-address:3000

- Now the stack deployment is done. The rest of the configuration and testing will be done the using the GUI.






## Validated Prometheus Node Exporter Metrics

- Visited http://my-public-ip-address:9090, accessed the Prometheus dashboard as shown below.

- Validated the targets, rules and configurations as shown below. The target would be the Node exporter url.




![1.12](./img10/1.12.png)






![1.13](./img10/1.13.png)





### Validated prometheus rules and targets

- Executed a promQL statement to view node_cpu_seconds_total metrics scrapped from the node exporter.


<pre><code>

avg by (instance,mode) (irate(node_cpu_seconds_total{mode!='idle'}[1m]))

</code></pre>




![1.14](./img10/1.14.png)




- I was able to data in graph as shown below.

- Executed a promQL statement to get graph



![1.15](./img10/1.15.png)



## Configured Grafana Dashboards

- Configured Grafana dashboards for the Node Exporter metrics.

- Accessed Grafana  at: http://my-ip-address:3000

- Used admin as username and password to login to Grafana. You can update the password in the next window if required.



![1.16](./img10/1.16.png)



- I added prometheus URL as the data source from Connections→ Add new connection→ Prometheus → Add new data source.






![1.17](./img10/1.17.png)










![1.18](./img10/1.18.png)







![1.19](./img10/1.19.png)








![1.20](./img10/1.20.png)







![1.21](./img10/1.21.png)








![1.22](./img10/1.22.png)





## Configured Node Exporter Dashboard


- Grafana has many node exporter pre-built templates that gave me a ready to use dashboard for the key node exporter metrics.

- To import a dashboard, went to Dashboards –> clicked Create Dashboard –> Import Dashboard –> Typed 10180 and clicked load –> Selected Prometheus Data source –> Import



![1.23](./img10/1.23.png)








![1.24](./img10/1.24.png)









![1.25](./img10/1.25.png)








- Once the dashbaord template got imported, I was able to see all the node exporter metrics as shown below;





![1.26](./img10/1.26.png)







## Simulated & Tested Alert Manager Alerts


- Accessed the Alertmanager dashbaord on http://my-ip-address:9093




- I also checked the alert rules using the native promtool prometheus CLI. I ran promtool command from inside the prometheus container as shown below. With the commands below in the prometheus server


<pre><code>

sudo docker exec -it prometheus promtool check rules /etc/prometheus/alertrules.yml

</code></pre>



## Test: High Storage & CPU Alert

<pre><code>

dd if=/dev/zero of=testfile_16GB bs=1M count=16384; openssl speed -multi $(nproc --all) &

</code></pre>



![1.27](./img10/1.27.png)








- Checked the Alert manager UserInterface to confirm the fired alerts.




![1.28](./img10/1.28.png)







- Rolled back the changed to see the fired alerts has been resolved.


<pre><code>

rm testfile_16GB && kill $(pgrep openssl)

</code></pre>



![1.29](./img10/1.29.png)









## Cleaned up The Setup


- Tore down the setup, executed the following terraform command.




- cd into prometheus-observability-stack/terraform-aws/prometheus-stack


<pre><code>

cd prometheus-observability-stack/terraform-aws/prometheus-stack

</code></pre>



<pre><code>


terraform destroy --var-file=../vars/ec2.tfvars

</code></pre>



- Enterd ***yes** to the promt



![1.30](./img10/1.30.png)





![1.31](./img10/1.31.png)



### Terminated all instances



# THE END OF PROJECT 