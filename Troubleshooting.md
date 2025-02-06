# Fixing Signal ILL Error While Installing MongoDB on Kali Linux

## Step 1: Remove Previous Versions
Before installing MongoDB 4.2.25, you need to remove any existing MongoDB versions from your system. Run the following commands:
```bash
sudo systemctl stop mongod
sudo apt-get purge -y mongodb-org mongodb-org-server mongodb-org-shell mongodb-org-mongos mongodb-org-tools mongodb-server-core
sudo apt-get autoremove -y
sudo apt-get clean
```
Additionally, manually remove any remaining MongoDB repository lists from:
```bash
sudo rm -f /etc/apt/sources.list.d/mongodb*.list
```
Also, check and remove any leftover binaries:
```bash
sudo rm -f /usr/bin/mongod /usr/bin/mongos
```
## Step 2: Add MongoDB 4.2 Repository
To install MongoDB 4.2.25, add the official MongoDB repository:
```bash
echo "deb [signed-by=/usr/share/keyrings/mongodb-4.2.gpg arch=amd64] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```
Then, download and add the GPG key:
```bash
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo tee /usr/share/keyrings/mongodb-4.2.gpg > /dev/null
```
## Step 3: Install MongoDB 4.2.25
Update package lists and install MongoDB:
```bash
sudo apt-get update
sudo apt-get install -y mongodb-org=4.2.25 mongodb-org-server=4.2.25 mongodb-org-shell=4.2.25 mongodb-org-mongos=4.2.25 mongodb-org-tools=4.2.25
```
## Step 4: Start and Enable MongoDB
Start the MongoDB service and enable it on boot:

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```
Check if MongoDB is running:
```bash
sudo systemctl status mongod
```
## Step 5: Fix PID File Warning (Optional)
If you see a warning related to the PID file, modify the MongoDB service file:

```bash
sudo nano /usr/lib/systemd/system/mongod.service
```
Find the line:

```
PIDFile=/var/run/mongodb/mongod.pid
```
Change it to:

```
PIDFile=/run/mongodb/mongod.pid
```
Reload the daemon and restart MongoDB:

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl restart mongod
```
Now, MongoDB 4.2.25 should be successfully installed

