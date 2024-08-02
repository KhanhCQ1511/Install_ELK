# Topic: How to install Elastic Stack on Ubuntu

# Requirement:
	At least 2 CPU Cores and 4 GB of RAM for smooth performance 
	
# Step #1: Install Java for Elastic Stack on Ubuntu 24.04 LTS
	
	sudo apt update
	
	sudo apt install apt-transport-https
	
	sudo apt install openjdk-11-jdk
	
	java --version
	
	sudo nano /etc/environment
	
		JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
		
	source /etc/environment
	
	echo $JAVA_HOME
	
# Step #2: Install ElasticSearch on Ubuntu 24.04 LTS

	wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
	
	echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
	
	sudo apt update
	
	sudo apt-get install elasticsearch
	
	sudo systemctl start elasticsearch
	sudo systemctl enable elasticsearch
	
	sudo systemctl status elasticsearch
	
# Step #3: Configure ElasticSearch on Ubuntu 24.04 LTS
	
	sudo nano /etc/elasticsearch/elasticsearch.yml

 		network.host: 0.0.0.0

   		discovvery.seed_hosts: []

     		xpack.security.enabled: false
	
	sudo systemctl restart elasticsearch
	
	curl -X GET "localhost:9200"
	
# Step #4: Install Logstash on Ubuntu 24.04 LTS

	sudo apt-get install logstash -y
	
	sudo systemctl start logstash
	sudo systemctl enable logstash
	
	sudo systemctl status logstash
	
# Step #5: Install Kibana on Ubuntu 24.04 LTS

	sudo apt-get install kibana
	
	sudo systemctl start kibana
	sudo systemctl enable kibana
	
	sudo systemctl status kibana
	
# Step #6: Configure Kibana on Ubuntu 24.04 LTS

	sudo nano /etc/kibana/kibana.yml

		server.port: 5601

  		server.host: "0.0.0.0"

    		elasticsearch.hosts: ["https://localhost:9200"]
   	
	sudo systemctl restart kibana
	
# Step #7: Install Filebeat on Ubuntu 24.04 LTS
	
	sudo apt-get install filebeat
	
	sudo nano /etc/filebeat/filebeat.yml

  		Comment: #output.elasticsearch:
    				# Array of hosts to connect to.
				# hosts: ["localhost:9200"]

      		Delete comment: output.logstash:
					# The logstash hosts
					hosts: ["localhost:5044"]
 
	sudo filebeat modules enable system
	
	sudo filebeat setup --index-management -E ouput.logstash.enabled=false -E 'output.elasticsearch.hosts=["0.0.0.0:9200"]'
	
	sudo systemctl start filebeat
	sudo systemctl enable filebeat
	
	curl  -XGET  "localhost:9200/_cat/indices?v"

# Step #8: Try in browser

 	Put "localhost:5601" in browser
