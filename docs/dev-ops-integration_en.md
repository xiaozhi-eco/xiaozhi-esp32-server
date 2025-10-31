# Full Module Source Code Deployment Auto-Update Method

This tutorial is for full-module source code deployment enthusiasts on how to automatically pull source code, compile, and start services through automatic commands to achieve maximum efficiency system upgrades.

The test platform of this project `https://2662r3426b.vicp.fun` has been using this method since its launch with good results.

Tutorial can refer to Bilibili blogger `毕乐labs` video tutorial: ["Open Source Xiaozhi Server xiaozhi-server Auto-Update and Latest Version MCP Endpoint Configuration Tutorial"](https://www.bilibili.com/video/BV15H37zHE7Q)

# Prerequisites
- Your computer/server is a Linux operating system
- You have successfully run the entire workflow
- You like to follow the latest features but find manual deployment a bit troublesome, hoping for an automatic update method

The second condition must be met because certain files involved in this tutorial, such as JDK, Node.js environment, Conda environment, etc., require you to have run through the entire workflow. If you haven't, when I mention certain files, you may not know what they mean.

# Tutorial Effects
- Solves the problem of not being able to pull the latest project source code in China
- Automatically pulls code and compiles frontend files
- Automatically pulls code, compiles Java files, automatically kills port 8002, automatically starts port 8002
- Automatically pulls Python code, automatically kills port 8000, automatically starts port 8000

# Step 1: Choose Your Project Directory

For example, I planned my project directory as follows. This is a new blank directory. If you don't want to make mistakes, you can follow me:
```
/home/system/xiaozhi
```

# Step 2: Clone This Project

Now, first execute this command to pull the source code. This command is suitable for servers and computers in China network, no VPN required:
```
cd /home/system/xiaozhi
git clone https://ghproxy.net/https://github.com/xinnan-tech/xiaozhi-esp32-server.git
```

After execution, your project directory will have an additional folder `xiaozhi-esp32-server`, which is the project source code.

# Step 3: Copy Base Files

If you have previously run through the entire workflow, you are familiar with the FunASR model file `xiaozhi-server/models/SenseVoiceSmall/model.pt` and your private configuration file `xiaozhi-server/data/.config.yaml`.

Now you need to copy the `model.pt` file to the new directory. You can do it like this:
```
# Create required directories
mkdir -p /home/system/xiaozhi/xiaozhi-esp32-server/main/xiaozhi-server/data/

cp your original .config.yaml full path /home/system/xiaozhi/xiaozhi-esp32-server/main/xiaozhi-server/data/.config.yaml
cp your original model.pt full path /home/system/xiaozhi/xiaozhi-esp32-server/main/xiaozhi-server/models/SenseVoiceSmall/model.pt
```

# Step 4: Create Three Auto-Compile Files

## 4.1 Auto-Compile manager-web Module
Create a file named `update_8001.sh` in the `/home/system/xiaozhi/` directory with the following content:

```
cd /home/system/xiaozhi/xiaozhi-esp32-server
git fetch --all
git reset --hard
git pull origin main


cd /home/system/xiaozhi/xiaozhi-esp32-server/main/manager-web
npm install
npm run build
rm -rf /home/system/xiaozhi/manager-web
mv /home/system/xiaozhi/xiaozhi-esp32-server/main/manager-web/dist /home/system/xiaozhi/manager-web
```

After saving, execute the permission command:
```
chmod 777 update_8001.sh
```
After execution, continue below.

## 4.2 Auto-Compile and Run manager-api Module
Create a file named `update_8002.sh` in the `/home/system/xiaozhi/` directory with the following content:

```
cd /home/system/xiaozhi/xiaozhi-esp32-server
git pull origin main


cd /home/system/xiaozhi/xiaozhi-esp32-server/main/manager-api
rm -rf target
mvn clean package -Dmaven.test.skip=true
cd /home/system/xiaozhi/

# Find process ID using port 8002
PID=$(sudo netstat -tulnp | grep 8002 | awk '{print $7}' | cut -d'/' -f1)

rm -rf /home/system/xiaozhi/xiaozhi-esp32-api.jar
mv /home/system/xiaozhi/xiaozhi-esp32-server/main/manager-api/target/xiaozhi-esp32-api.jar /home/system/xiaozhi/xiaozhi-esp32-api.jar

# Check if process ID was found
if [ -z "$PID" ]; then
  echo "No process found using port 8002"
else
  echo "Found process using port 8002, process ID: $PID"
  # Kill the process
  kill -9 $PID
  kill -9 $PID
  echo "Killed process $PID"
fi

nohup java -jar xiaozhi-esp32-api.jar --spring.profiles.active=dev &

tail tail -f nohup.out
```

After saving, execute the permission command:
```
chmod 777 update_8002.sh
```
After execution, continue below.

## 4.3 Auto-Compile and Run Python Project
Create a file named `update_8000.sh` in the `/home/system/xiaozhi/` directory with the following content:

```
cd /home/system/xiaozhi/xiaozhi-esp32-server
git pull origin main

# Find process ID using port 8000
PID=$(sudo netstat -tulnp | grep 8000 | awk '{print $7}' | cut -d'/' -f1)

# Check if process ID was found
if [ -z "$PID" ]; then
  echo "No process found using port 8000"
else
  echo "Found process using port 8000, process ID: $PID"
  # Kill the process
  kill -9 $PID
  kill -9 $PID
  echo "Killed process $PID"
fi
cd main/xiaozhi-server
# Initialize conda environment
source ~/.bashrc
conda activate xiaozhi-esp32-server
pip install -r requirements.txt
nohup python app.py >/dev/null &
tail -f /home/system/xiaozhi/xiaozhi-esp32-server/main/xiaozhi-server/tmp/server.log
```

After saving, execute the permission command:
```
chmod 777 update_8000.sh
```
After execution, continue below.

# Daily Updates

After all the scripts above are created, for daily updates, we just need to execute the following commands in sequence to achieve automatic updates and startup:

```
cd /home/system/xiaozhi
# Update and start Java program
./update_8001.sh
# Update web program
./update_8002.sh
# Update and start Python program
./update_8000.sh


# To view Java logs later, execute the following command
tail -f nohup.out
# To view Python logs later, execute the following command
tail -f /home/system/xiaozhi/xiaozhi-esp32-server/main/xiaozhi-server/tmp/server.log
```

# Notes

The test platform `https://2662r3426b.vicp.fun` uses nginx for reverse proxy. For detailed nginx.conf configuration, please [refer here](https://github.com/xinnan-tech/xiaozhi-esp32-server/issues/791)

## FAQ

### 1. Why is port 8001 not seen?
Answer: 8001 is for development environment, used for running the frontend. If you are doing server deployment, it is not recommended to use `npm run serve` to start port 8001 to run the frontend. Instead, compile into HTML files as in this tutorial, then use nginx to manage access.

### 2. Do I need to manually update SQL statements every time I update?
Answer: No, because the project uses **Liquibase** to manage database versions and will automatically execute new SQL scripts.

