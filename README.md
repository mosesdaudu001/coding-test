# POC for coding-test

1. Clone the CVAT repository to access the necessary files.
	```
	git clone https://github.com/opencv/cvat.git
	```
 
2. Navigate to the CVAT directory
	```
	cd cvat
	```
   
3. Start CVAT together with the plugin use for AI automatic annotation assistant.
	
	```
	docker compose -f docker-compose.yml -f components/serverless/docker-compose.serverless.yml up -d
	```
4. Create an account
	```
	docker exec -it cvat_server bash -ic 'python3 ~/manage.py createsuperuser'
	```

5. Install `nuctl`
   
	```
	wget https://github.com/nuclio/nuclio/releases/download/1.8.14/nuctl-1.8.14-linux-amd64
	```
	
6. After downloading the nuclio, give it a proper permission and do a softlink.
   
	```
	sudo chmod +x nuctl-1.8.14-linux-amd64
	sudo ln -sf $(pwd)/nuctl-1.8.14-linux-amd64 /usr/local/bin/nuctl
	```

7. Add required configurations for deploying the model using Nuclio. First you need to copy the custom model to the cloned cvat repository, which should have a location in the format of "cvat/serverless/pytorch/ultralytics/ultralytics/nuclio". Then add and modify the files: "fucntion.yaml" and "main.py"

8. Build the docker image and run the container. After it is done, you can use the model right away in the CVAT.
	```
	./serverless/deploy_cpu.sh cvat/serverless/pytorch/ultralytics/ultralytics/nuclio
	```

## File Structure

- `function.yaml`: Declare the model so it can be understand by CVAT. It includes setup the docker environment.

- `main.py`: Contain the handle function that will serve as the endpoint used by CVAT to run detection.
