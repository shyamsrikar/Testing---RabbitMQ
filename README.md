# Testing - RabbitMQ

## What is RabbitMQ?

RabbitMQ is an open-source message broker — a software that helps applications, services, and systems communicate with each other by sending and receiving messages through a centralized system.

### 🧠 In Simple Terms:

RabbitMQ acts like a "post office" between your applications:

- One application (producer) sends a message to RabbitMQ.

- Another application (consumer) receives it from RabbitMQ.
  
![Screenshot from 2025-06-22 10-27-02](https://github.com/user-attachments/assets/6b63eee9-3131-427b-812e-90e4a0f302db)




## Before installing RabbitMQ, you need a server to run it. Here’s how to set up an Ubuntu server on AWS:

## 📦 PART-I

- 1) Login to AWS Console and go to EC2 Dashboard.
     
- 2) Click "Launch Instance".

![Screenshot from 2025-06-22 09-46-48](https://github.com/user-attachments/assets/c8fbf5cd-0b88-4411-ba28-c20de704a8e8)
  
- 3)  Choose AMI: Select Ubuntu Server 24.04 LTS (HVM), SSD Volume Type.

 ![Screenshot from 2025-06-22 09-47-15](https://github.com/user-attachments/assets/b036215d-e456-4e50-b380-49bb7aba08b4)

- 4) Instance Type: Choose t2.micro (free tier) or a higher type if needed.

- 5) Key Pair: Select or create a key pair to access your server.

  ![Screenshot from 2025-06-22 09-47-50](https://github.com/user-attachments/assets/c42e5577-3a9a-40b0-83c8-4a2ff7ad5204)
  
- 6) Network Settings:
  7) 
  SSH (TCP 22)
  Custom TCP Rule (5672): Source → 0.0.0.0/0 (for RabbitMQ AMQP access)
  Custom TCP Rule (15672): Source → 0.0.0.0/0 (for RabbitMQ Web UI)

  ![Screenshot from 2025-06-22 09-48-46](https://github.com/user-attachments/assets/25369730-0545-43ea-a469-7c06fe1f5803)
  
- 7) Launch Instance.
     
- 8) After it’s running, connect to it using:
     
  ![Screenshot from 2025-06-22 09-49-48](https://github.com/user-attachments/assets/1b7cb64e-5438-4037-86b7-c15d90c60977)

  
 After Connecting:

 ![Screenshot from 2025-06-22 09-50-22](https://github.com/user-attachments/assets/4d87f99d-d6ca-42c4-9969-bfb2e4c3d71d)


 ## 📦 PART-II

 # 🛠️ Installation Steps for RabbitMQ on Ubuntu Server After Connecting to the Instance Using Docker and Docker-Compose:

 ## To install RabbitMQ on Ubuntu, follow these steps:

 ### ✅ Step 1: Update System and Install Docker

 Run a command to update the list of software packages and install docker

 ```
  sudo apt update
  sudo apt install docker.io -y
```

Start and enable Docker to run on boot:

```
 sudo systemctl start docker
 sudo systemctl enable docker
```

### ✅ Step 2: check Wheather the Docker is Installed or not

```
 docker --version

```
### ✅ Step 3: Install Docker Compose

Docker Compose is used to manage multi-container Docker applications.

```
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \
-o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

✅ You should see the Docker Compose version printed — confirmation it's installed.

## 📦 Part-III

## Run RabbitMQ with Docker Compose

### ✅ Step 4: Create a Docker Compose File

Create a project directory for RabbitMQ:

```
mkdir rabbitmq && cd rabbitmq
```

Now create a docker-compose.yml file:

```
nano docker-compose.yml
```

Paste the following configuration into the file:

```
version: '3.8'

services:
  rabbitmq:
    image: rabbitmq:3-management
    container_name: rabbitmq
    ports:
      - "5672:5672"    # AMQP port
      - "15672:15672"  # Management UI
    environment:
      RABBITMQ_DEFAULT_USER: admin
      RABBITMQ_DEFAULT_PASS: StrongPassword123
    restart: always
```

Save and exit the file: Ctrl + O, Enter, then Ctrl + X.

### ✅ Step 5: Start RabbitMQ

Launch RabbitMQ using Docker Compose:

```
sudo docker-compose up -d
```

![Screenshot from 2025-06-22 09-54-03](https://github.com/user-attachments/assets/3af43321-7a85-4660-a641-c9b1d430d37d)


Verify that the container is running:

```
sudo docker ps
```

✅ You should see a container named rabbitmq running on ports 5672 and 15672.

## 📦 Part-IV: Access RabbitMQ via Public IP

### ✅ Step 6: Open RabbitMQ Management UI

Open a web browser and navigate to:

```
http://<EC2_PUBLIC_IP>:15672
```

#### Login with:

- Username: admin
- Password: StrongPassword123

![Screenshot from 2025-06-22 09-55-35](https://github.com/user-attachments/assets/3dbc5204-31d7-45e9-9484-6fb9ee81b77b)


✅ You should now see the RabbitMQ Management Dashboard like this.

![Screenshot from 2025-06-22 09-55-44](https://github.com/user-attachments/assets/95058abd-0dbe-4420-8c0d-a4ef7e001010)

  
# Monitors Implemented for Testing RabbitMQ service:

# Monitors Implemented:

![Screenshot from 2025-06-22 10-12-38](https://github.com/user-attachments/assets/af8ec81d-9de5-4dc4-a698-478d766ec29e)

## Monitors to be Tested:

### Memory Usage Check

- Continuously checks the memory consumption of the RabbitMQ service to detect memory leaks or excessive resource usage that could impact system performance.

![Screenshot from 2025-06-22 10-13-48](https://github.com/user-attachments/assets/66cbfb7b-6145-488f-832c-dd8c802b3131)


### Consumer Lag Check

- Monitors the lag between message production and consumption, helping identify slow consumers or unbalanced workloads across consumers.

![Screenshot from 2025-06-22 10-13-28](https://github.com/user-attachments/assets/3ba5631f-a1aa-457f-97cf-e8d572377b69)

### Queue Depth Check

- Tracks the depth of message queues to ensure that messages are being processed in a timely manner and to flag potential congestion or backlogs.

![Screenshot from 2025-06-22 10-13-17](https://github.com/user-attachments/assets/5b3cee7e-f43d-4a6e-8dd7-078730eed44d)

### Connectivity Check

- Verifies network-level connectivity to the RabbitMQ server, ensuring that the service is reachable and available for communication from the monitoring host.

![Screenshot from 2025-06-22 10-13-08](https://github.com/user-attachments/assets/affedfbc-1c98-44c0-a351-7647a64f4e1e)

# Conclusion:
- All the monitors are running successfully after passing the environment variables for RabbitMQ service.
