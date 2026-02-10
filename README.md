# 🔐 Enigma
Enigma is a tool for brute forcing Crypto Wallets

![EnigmaV2](assets/example.gif)

## ⚠️**Disclaimer**⚠️

This script is developed for educational and research purposes only.

**By using this code, you agree to the following:**

1. You will not use this code, in whole or in part, for malicious intent, including but not limited to unauthorized mining on third-party systems.
2. You will seek explicit permission from any and all system owners before running or deploying this code.
3. You understand the implications of running mining software on hardware, including the potential for increased wear and power consumption.
4. The creator of this script cannot and will not be held responsible for any damages, repercussions, or any negative outcomes that result from using this script.

If you do not agree to these terms, please do not use or distribute this code.

## 🤔 **How it works?**

We'll begin by delving into the foundational concepts. Upon establishing a wallet through platforms like **Exodus/TrustWallet** or similar services, users receive a **mnemonic phrase (_seed-phrase_)** comprised of **12 unique words**. The selection of words for this passphrase isn't arbitrary; they are derived from a specific lexicon containing **2048 potential words**. From this collection, the passphrase words are selected at random (**_the entire list of these words is accessible_** [**_HERE_**](https://www.blockplate.com/pages/bip-39-wordlist)). Utilizing this passphrase, an individual has the capability to access their wallet on any device and manage their assets. My application operates by employing brute force techniques to decipher these passphrases.

If Enigma finds a wallet with a balance, it will create `wallets_with_balance.txt` file that will contain the info of the discovered wallet.

Upon execution, Enigma generates a comprehensive log file named `Enigma.log`, which neatly records the entire session history for review and analysis.

# ⚙️ Technical Details

## 🌱 Master Seed and Wallet Generation

![hierarchical_deterministic_wallets](assets/hierarchical_deterministic_wallets.png)

Enigma is engineered around the key principle of the **Master Seed** in cryptocurrency wallet generation, as per the standards described in BIP 32 for Hierarchical Deterministic (HD) Wallets. The Python script provided within this repository is designed to create a mnemonic phrase (also known as a seed phrase), which essentially acts as the **Master Seed** from which all cryptographic keys can be derived.

For a more in-depth understanding of this topic, feel free to explore the detailed documentation available here: [BIP 32 wiki](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki).

### 🔑 The Role of Master Seed in Enigma

The script leverages the `bip_utils` library to generate a 12-word BIP39 mnemonic. This mnemonic is a human-readable representation of the wallet's **Master Seed**. This seed is then used to generate seeds for various cryptocurrency wallets, specifically for Bitcoin (BTC) and Ethereum (ETH), by following the BIP44 protocol that defines a logical hierarchy for deterministic wallets.

### 📋 Code Workflow:

1. **Seed Generation**: The `bip()` function in the script calls upon the BIP39 protocol to generate a new 12-word mnemonic. This is the first and most crucial step in the HD wallet creation process.
    
2. **Seed to Wallet Transformation**: The functions `bip44_ETH_wallet_from_seed` and `bip44_BTC_seed_to_address` take the generated mnemonic and produce the corresponding wallet addresses for Ethereum and Bitcoin, respectively. These addresses are derived from the master seed and follow a deterministic path outlined by BIP44, ensuring that each mnemonic generates a unique and recoverable set of addresses.
    
3. **Balance Checking**: With the generated addresses, the script uses online blockchain explorers through their APIs (Etherscan for Ethereum and Blockchain.info for Bitcoin) to check if the generated wallets contain any balance.
    
4. **Logging Results**: If a balance is found, the script writes the mnemonic, the derived addresses, and the wallet balances to a file (`wallets_with_balance.txt`), preserving the potentially valuable information for further examination.
    

Through the integration of BIP39 and BIP44 protocols, Enigma serves as a practical example of how the **Master Seed** forms the bedrock of cryptocurrency wallets, allowing for a secure, hierarchical structure of key derivation and management.

## 📦 Installation

Clone the repository using:

```bash
git clone https://github.com/juliuspleunes4/Enigma.git
cd Enigma
```
Remember to install the required libraries using:
```bash
pip install -r Enigma/requirements.txt
```
## ⚙️ Configuration

1. 🔑 Obtain an Etherscan API key following the instructions [here](https://docs.etherscan.io/getting-started/viewing-api-usage-statistics).
2. 📂 Navigate to the Enigma directory and copy the example environment file:
```bash
cp Enigma.env.example Enigma.env
```
3. ✏️ Open Enigma.env and insert your API key:
```bash
# In Enigma.env
ETHERSCAN_API_KEY=your_api_key_here
```
## ▶️ Execution

Run Enigma from the command line:

```bash
python Enigma/Enigma.py
```
---
## 🐳 Running Enigma in Docker

### ✅ Prerequisites
- Docker installed on your system. You can download and install Docker from [Docker's official website](https://www.docker.com/get-started).
- Docker Compose (usually comes with the Docker installation).

### 🏃 Steps to Run

1. **Clone the Repository**  
   If you haven't already, clone the Enigma repository to your local machine:
   ```bash
   git clone https://github.com/juliuspleunes4/Enigma.git
   ```

2. **Setting Up Environment Variables**  
   Before running the Docker container, you need to set up your environment variables:
   - Navigate to the `Enigma-Docker` directory.
   - Copy the example environment file:
     ```bash
     cp Enigma.env.example Enigma.env
     ```
   - Open the `Enigma.env` file and replace `your_api_key_here` with your actual Etherscan API key.
   - Open the `docker-compose.yml` file and replace `your_etherscan_api_key` with your actual Etherscan API key.

3. **Building the Docker Image**  
   From the root directory of the project in the Enigma-Docker folder (where the `Dockerfile` is located), run the following command to build the Docker image:
   ```bash
   docker-compose build
   ```
   This command reads the `Dockerfile` and `docker-compose.yml` to build the Enigma Docker image.

4. **Running the Docker Container**  
   After the build is complete, start the Docker container using Docker Compose:
   ```bash
   docker-compose up
   ```
   This command starts the Enigma service defined in `docker-compose.yml`. The script inside the Docker container (`EC.py`) will automatically execute.

5. **Viewing Logs and Output**  
   The output of the script, including any logs, will be displayed in your terminal. Additionally, log files and any generated files like `wallets_with_balance.txt` will be stored in the `./data` directory on your host machine, which is mapped to `/usr/src/app/data` in the container for persistent storage.

6. **Stopping the Container**  
   To stop the Docker container, use the command:
   ```bash
   docker-compose down
   ```
   This command stops and removes the containers, networks, and volumes created by `docker-compose up`.

### 📝 Note
- The Docker environment provides an isolated and consistent runtime for Enigma.
- Ensure that the Docker daemon is running before executing these commands.
- Adjustments to the script or environment variables require a rebuild of the Docker image for changes to take effect.
- **Modified Script for Docker**: The Docker version of Enigma runs a slightly modified version of the script (`EC.py`) compared to the standalone version. These modifications are specifically tailored for the Docker environment to ensure smooth operation within a container. For instance, any code segments that require GUI interaction or OS-specific commands have been adjusted or removed since Docker containers typically run in a headless (non-GUI) environment.
- **Streamlined Dependencies**: The `requirements.txt` file for the Docker version contains fewer libraries. This is because Docker provides a controlled environment where only the necessary dependencies are included to run the script. This streamlined approach helps in reducing the overall size of the Docker image and improves the efficiency of the script within the container.
---

## ☁️ Running Enigma on AWS

This guide will walk you through the process of using Enigma on AWS. The steps include setting up an Amazon ECR repository for your Docker image, creating an EC2 instance with Ubuntu, and then pulling and running the Enigma Docker container on that instance.

### 📤 Step 1: Uploading Your Docker Image to Amazon ECR

1. **Create an ECR Repository:**
   - Navigate to the Amazon ECR console.
   - Click on "Create repository."
   - Name your repository (e.g., `Enigma-docker`), and click "Create repository."

2. **Authenticate Docker to Your ECR Repository:**
   - Retrieve the `docker login` command that you can use to authenticate your Docker client to your registry:
     ```bash
     aws ecr get-login-password --region [your-region] | docker login --username AWS --password-stdin [your-account-id].dkr.ecr.[your-region].amazonaws.com
     ```
   - Replace `[your-region]` and `[your-account-id]` with your AWS region and account ID.

3. **Tag Your Docker Image:**
   - Tag your local Enigma Docker image with the ECR repository URI:
     ```bash
     docker tag Enigma:latest [your-account-id].dkr.ecr.[your-region].amazonaws.com/Enigma-docker:latest
     ```

4. **Push the Image to ECR:**
   - Push your Docker image to the ECR repository:
     ```bash
     docker push [your-account-id].dkr.ecr.[your-region].amazonaws.com/Enigma-docker:latest
     ```

### 🖥️ Step 2: Creating an EC2 Instance with Ubuntu OS

1. **Launch an EC2 Instance:**
   - Go to the EC2 Dashboard in AWS Management Console.
   - Click "Launch Instance."
   - Choose an "Ubuntu Server" AMI (Amazon Machine Image).
   - Select an instance type (e.g., `t2.micro` for testing purposes).
   - Configure instance settings as needed, then click "Review and Launch."

2. **Configure Security Group:**
   - Add rules to allow SSH access to the instance.

3. **Launch and Access the Instance:**
   - Review your settings and click "Launch."
   - Select a key pair or create a new one, and then launch the instance.
   - Once the instance is running, connect to it via SSH.

### 🚀 Step 3: Pulling and Running the Enigma Docker Container

1. **Install Docker on EC2 Instance:**
   - Connect to your EC2 instance via SSH.
   - Update the package list and install Docker:
     ```bash
     sudo apt update
     sudo apt install docker.io
     ```

2. **Authenticate EC2 Docker to ECR:**
   - Run the same `docker login` command used earlier to authenticate Docker to your ECR repository.

3. **Pull the Docker Image:**
   - Pull the Enigma image from your ECR repository:
     ```bash
     docker pull [your-account-id].dkr.ecr.[your-region].amazonaws.com/Enigma-docker:latest
     ```

4. **Run the Enigma Container:**
   - Run the Docker container:
     ```bash
     docker run [your-account-id].dkr.ecr.[your-region].amazonaws.com/Enigma-docker:latest
     ```

### 📊 Step 4: Monitoring and Interacting with the Enigma Container

To monitor and interact with the Enigma container running on your EC2 instance, you can use the following steps:

1. **Access the EC2 Instance**:
   - Connect to your EC2 instance using SSH.

2. **Elevate to Root (Optional)**:
   - If necessary, switch to the root user for broader permissions. However, be cautious as root access can modify critical system files.
     ```bash
     sudo su
     ```
   
3. **List All Containers**:
   - Check the status of all Docker containers, including the Enigma container.
     ```bash
     docker ps -a
     ```

4. **Copy Log Files from Container to EC2 Instance**:
   - If you want to inspect the log files generated by Enigma, copy them from the container to your EC2 instance. Replace `Enigma_container` with the actual container ID or name.
     ```bash
     docker cp Enigma_container:/usr/src/app/Enigma.log /home/ubuntu/Enigma.log
     ```

5. **Review the Log File**:
   - Navigate to the directory where you copied the log file and use `grep` or other tools to analyze it. For example, to count the number of founded wallets in the log file:
     ```bash
     cd /home/ubuntu
     grep -c '!' Enigma.log
     ```

### 🎉 Conclusion

Setting up Enigma on an AWS EC2 instance with your Docker image in Amazon ECR offers improved scalability and reliability for your wallet scanning tasks. This approach provides a streamlined and effective solution to harness Enigma’s full potential on a powerful cloud platform.

---
## 🆕 Updates

- 💰 **Dual Cryptocurrency Detection**: Enigma now supports detection of both BTC and ETH wallets.