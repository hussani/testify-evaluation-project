install:
	sudo apt-get update -y 
	sudo apt-get install -y python3 python3-pip docker.io unzip
	sudo apt-get upgrade -y ca-certificates
	sudo service docker start
	sudo usermod -aG docker $USER

setup_project:
	sudo mkdir /app
	sudo chown ubuntu:adm /app
	wget https://github.com/hussani/runner/archive/refs/heads/fix-concurrency-files.zip
	unzip fix-concurrency-files
	mv runner-fix-concurrency-files/* /app
	cd /app
	sudo chmod +x *
	pip3 install -r requirements.txt
