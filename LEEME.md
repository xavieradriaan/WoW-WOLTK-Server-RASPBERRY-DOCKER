TUTORIAL
https://www.azerothcore.org/wiki/installation
https://www.youtube.com/watch?v=Kw_xhyssmTw
******[Docker Setup]******

sudo apt update && sudo apt upgrade -y

sudo reboot

sudo apt autoremove -y

sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update && sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

grep docker /etc/group

sudo usermod -aG docker ubuntu

groups ubuntu

sudo nano /boot/firmware/config.txt

kernel=kernel8.img

sudo reboot

systemctl status docker

(Control + C to get back to the terminal)

docker run hello-world

docker ps -a

docker rm d48f8dbbfe93      (Replace e11cced53bca with the container ID for Image "hello-world")

docker ps -a

docker rmi hello-world


----------------------------------------------------------------------------------------------------------------------------


******[Portainer Setup] (Optional)******

docker volume create portainer_data

docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest

Log in via browser to set up Portainer account: (If you're checking on the local machine) https://localhost:9443 (If you're checking from another machine use: https://192.168.1.1:9443 (Replace the 192.168.1.1 with your actual docker servers IP)

sudo reboot


-------------------------------------------------------------------------------------------------------------------------------


******[AzerothCore 3.3.5 With Playerbots Module Docker Setup]******

mkdir /home/ubuntu/docker

chown -R ubuntu:ubuntu /home/ubuntu/docker

cd /home/ubuntu/docker

git clone https://github.com/liyunfan1223/azerothcore-wotlk.git --branch=Playerbot

cd /home/ubuntu/docker/azerothcore-wotlk/modules

git clone https://github.com/liyunfan1223/mod-playerbots.git --branch=master

cd /home/ubuntu/docker/azerothcore-wotlk/

sudo nano /etc/nanorc

(Uncomment set lineumbers AKA remove the # from the front of it)

sudo nano docker-compose.yml

(Add To Line 90)

      # playerbots folder
      - ./modules/mod-playerbots/:/azerothcore/modules/mod-playerbots:ro

docker compose up -d --build

******(Adjusting Your Realmlist SQL Configurations)******

docker ps -a

(Locate mysql container ID and change 06eb70e041dc  that you see below with your ID)

docker exec -it 8a3ccdf7bc8e   mysql -uroot -p      (Acccount: root)  (Password: password)

use acore_auth

DELETE FROM realmlist WHERE id=1;
INSERT INTO realmlist (id, name, address, localAddress, localSubnetMask, port, icon, flag, timezone, allowedSecurityLevel, gamebuild)
VALUES ('1', 'Northrend', '10.243.222.228', '127.0.0.1','255.255.255.0','8085', '1', '0', '1', '0', '12340');

exit

docker ps -a

docker restart 647511e6b0e6 e771ebadb593 173a55a6946c a41133c2472c 7f0f6ed8d25b    (Should be 5 containers total! Replace their ID's with the demonstration ones)

docker ps -a

******(Adding an Account)******

docker attach ac-worldserver

account create test1 test

account set addon test1 2

Control + P + Control + Q  ***(Do not do Control + C or you will bring the world server down!)***


--------------------------------------------------------------------------

más mods: https://www.azerothcore.org/catalogue#/details/228365811


https://www.azerothcore.org/catalogue#/details/138432861

https://www.azerothcore.org/catalogue#/details/113240184


https://www.azerothcore.org/catalogue#/details/74011079


--------------------------------------------------------------------

******[AzerothCore 3.3.5 Docker Without Modules Setup]******

mkdir /home/ubuntu/docker

chown -R ubuntu:ubuntu /home/ubuntu/docker

cd /home/ubuntu/docker

git clone https://github.com/azerothcore/azerothcore-wotlk.git --depth 1

cd /home/ubuntu/docker/azerothcore-wotlk/

docker compose up -d --build



------------------------------------------------------------------------------------------------------------------------------


******(Adjusting Your Realmlist SQL Configurations)******

docker ps -a

docker exec -it ec9a4b862224 mysql -uroot -p       (Acccount: root)  (Password: password)

use acore_auth

DELETE FROM realmlist WHERE id=1;
INSERT INTO realmlist (id, name, address, localAddress, localSubnetMask, port, icon, flag, timezone, allowedSecurityLevel, gamebuild)
VALUES ('1', 'Northrend', '192.168.60.188', '127.0.0.1','255.255.255.0','8085', '1', '0', '1', '0', '12340');

exit

docker ps -a

docker restart 647511e6b0e6 e771ebadb593 173a55a6946c a41133c2472c 7f0f6ed8d25b   (Should be 5 containers total! Replace their ID's with the demonstration ones)

docker ps -a

******(Adding an Account)******

docker attach ac-worldserver

account create test1 test

account set addon test1 2

Control + P + Control + Q  ***(Do not do Control + C or you will bring the world server down!)***

-----------------------------------------------------------------------


Datos ADICIONALES


modificaciones del servidor propio:

en : ubuntu@raspberrypi:~/docker/azerothcore-wotlk/env/dist/etc
sudo nano worldserver.conf

-------------------------------------------

para hacer modificaciones en los bots:
cd docker/azerothcore-wotlk/modules/mod-playerbots/conf 

sudo nano playerbots.conf.dist

luego de modificar, reiniciar manualmente la raspberry:
cd
cd docker/azerothcore-wotlk 
docker ps -a
visualizar todos los contenedores  para restart menos portainer

docker restart 647511e6b0e6 e771ebadb593 173a55a6946c a41133c2472c 7f0f6ed8d25b

por último:

docker attach ac-worldserver


