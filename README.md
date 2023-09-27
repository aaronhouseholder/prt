# Installing PRT

This guide is specifically created for those who are looking to install [petitRADTRANS](https://petitradtrans.readthedocs.io/en/latest/) using Docker Desktop on a Mac with Apple silicon. The steps make use of Docker to ensure compatibility and reproducibility across different environments. 

# Prerequisites
Docker Desktop for Mac (Apple Silicon): Make sure you have Docker Desktop installed on your machine. If not, follow the installation guide for Mac with Apple silicon [here](https://docs.docker.com/desktop/install/mac-install/). You must have it downloaded and open for the steps below to work. 

Download the opacity and input data (12.1 GB) from petitRADTRANS. Make a folder titled `input_data` on your Desktop with the four different folders (e.g., `abundance_files`). In principle, you can put this folder anywhere. You will just have to update the path in `compose.yaml`

# Steps: 

Step 1: Clone the repo to your desktop: `cd Desktop` and `git clone https://github.com/aaronhouseholder/prt`

Step 2: Navigate to the Cloned Directory: `cd prt`

Step 3: Build the Docker Image: `docker build -f docker/Dockerfile --tag prt .`

`--tag prt` assigns a name (in this case prt) to your image, making it easier to reference later. Without it, Docker will assign a default hash as the name.

Step 4: Start the Docker Container: `docker compose up -d`

Step 5: Enter the Running Container: `docker compose exec dev bash`
This opens a bash shell inside the running container.

Step 6: You're all set! You can now test the installation:
```
cd prt
python
from petitRADTRANS import Radtrans
atmosphere = Radtrans(line_species = ['CH4'])
```

This should produce

```
Read line opacities of CH4...
Done.
```

To start a Jupyter notebook session inside the Docker container, execute:
`sh run_jupyter.sh`

Once you run the above command, Jupyter will provide you with a URL. Copy and paste this URL (e.g., http://127.0.0.1:8889/?token=e9e2004c9ce2ab68865acbff26892f1c9dfd6577e84dd52c) into your preferred browser (e.g. Chrome, Safari) to access the Jupyter notebook environment (if it doesn't appear automatically).

# Additional Commands
Once you have petitRADTRANS running inside the Docker container, there are a few more Docker commands that can be useful in managing your container and services:

Stopping Containers:
`docker compose stop`

This command will stop the running containers without removing them. The containers' state is preserved, allowing you to start them again without reinitializing anything. Using this method is recommended because it allows you to pause your work temporarily and resume later, without leaving containers running indefinitely. 

Starting Containers:
`docker compose start`

If you've previously stopped containers using the stop command, you can start them again with this command. It will resume the containers from where they were left off.

Shutting Down Containers 
`docker compose down`

This command stops the running containers and also removes the containers, networks, and volumes defined in the `compose.yaml` file. It's a way to clean up resources. If you use this command, you'll need to use the `docker compose up -d` command again to reinitialize and start the containers.

The Dockerfile also installs `MultiNest` and `species`, so you should (hopefully) be able to run petitRADTRANS without any issues! If you need to include additional Python packages in your Docker environment, simply add the package name to the section of the Dockerfile where Python dependencies are listed and installed. Ensure you rebuild the Docker image after making changes to the Dockerfile for the new packages to be included.

# Common Issues

If you encounter the `Killed` message when running some of the retrieval examples, it's likely due to memory limitations in Docker Desktop. To fix this, follow these steps: 

navigate to the Docker Desktop dashboard > click on the gear icon on top right > click on resources > crank the memory up! 

It's worth noting that some of these retrievals demand significant computational power. It might be best to avoid running them on a laptop.
