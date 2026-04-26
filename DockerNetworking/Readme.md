# 🚀 Docker Networking Practical Session

## Step 1: install docker if not already there 

-- sudo apt install docker.io

Step 2: docker 

Step 3: Run First Container Using Nginx Image
sudo docker run -d --name login nginx:latest
What Happened?
Docker checked local system for nginx:latest
If not found, it pulled image from Docker Hub
Created container named login
Started it in detached mode

Step 4: Run Second Container Using Same Image
sudo docker run -d --name logout nginx:latest
What Happened?
Reused same nginx:latest image
Created second container named logout

Step 5: Check Docker Images
sudo docker images
Output
IMAGE          ID
nginx:latest   6e23479198b9
Explanation

Only one image appears because both containers use same image.

Step 6: Check Running Containers
sudo docker ps
Output
CONTAINER ID   IMAGE          NAMES
16232ef2a3f8   nginx:latest   logout
ba39da97afa9   nginx:latest   login
Explanation

Two separate running containers created from one image.

Step 7: Enter Inside Login Container
sudo docker exec -it login /bin/bash
What Happened?

Interactive shell opened inside login container.

Step 8: Install Ping Utility Inside Container
apt update
apt install iputils-ping
Why?

Minimal containers usually don’t contain ping command.

Step 9: Ping Second Container
ping 172.17.0.3
Result
64 bytes from 172.17.0.3: icmp_seq=27 ttl=64 time=0.051 ms
64 bytes from 172.17.0.3: icmp_seq=28 ttl=64 time=0.040 ms
64 bytes from 172.17.0.3: icmp_seq=29 ttl=64 time=0.040 ms
64 bytes from 172.17.0.3: icmp_seq=30 ttl=64 time=0.039 ms
64 bytes from 172.17.0.3: icmp_seq=31 ttl=64 time=0.041 ms
64 bytes from 172.17.0.3: icmp_seq=32 ttl=64 time=0.040 ms

--- 172.17.0.3 ping statistics ---
32 packets transmitted, 32 received, 0% packet loss, time 31758ms
rtt min/avg/max/mdev = 0.038/0.046/0.090/0.010 ms
root@ba39da97afa9:/# ^C
root@ba39da97afa9:/#

64 bytes from 172.17.0.3
64 bytes from 172.17.0.3
64 bytes from 172.17.0.3
Meaning

login container successfully communicated with logout container.

Step 10: Inspect Login Container Networking
sudo docker inspect login
Important Values Found
IPAddress : 172.17.0.2
Gateway   : 172.17.0.1
Network   : bridge

Step 11: Inspect Logout Container Networking
sudo docker inspect logout
Important Values Found
IPAddress : 172.17.0.3
Gateway   : 172.17.0.1
Network   : bridge

## imp one container pinged the other so they communicating successfully so this is docker networking 

Final Networking Understanding
login container   -> 172.17.0.2
logout container  -> 172.17.0.3

Both connected to:

docker0 bridge -> 172.17.0.1
Internal Communication Flow
login container
   ↓
eth0
   ↓
veth pair
   ↓
docker0 bridge
   ↓
logout container
Final Conclusion
One image can create multiple containers
Containers get separate private IPs
Default Docker bridge network allows communication
## Ping between containers confirms networking works

internally it is lot of things happening related to namespace if you want to learn that you can watch this video

https://www.youtube.com/watch?v=j_UUnlVC2Ss

this video explains regarding the namespace which is the core concepts how docker are connectd with the namespace concept and how the how the isolation of containers and namspace happens and networking of containers and namespace happens

i personally like this video
i watched this because i was unable to get the connection between veth pair connecting docker0 and that whole process

This video explains:
How Docker uses namespaces for container isolation
How processes are separated inside containers
How container networking works using namespaces
How Docker connects Linux kernel features with containers
It is a very useful resource for understanding what happens internally when a container runs.
