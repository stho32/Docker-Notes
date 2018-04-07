# Docker-Notes
Notes about docker

  - pluralsight path "Container Management using Docker"
  - IQ Test awful :)
  - started with "Docker networking" because I need that the most
  
## Docker networking

### Single Host Networking

Single Host Networking is about a network using only one docker host.
(Like best for a developer)

  - create a network ps-bridge ("pluralsight-bridge") 
  ```
  docker network create -d bridge --subnet 10.0.0.1/24 ps-bridge
  ```

  - lets list all networks to see if it really happened
  ```
  docker network ls
  ```
  
  - lets check the configuration of the network
  ```
  docker network inspect ps-bridge
  ```
  
  - lets create a bunch of containers and attach them
  ```
  docker run -dt --name container1 --network ps-bridge alpine sleep 1d
  docker run -dt --name container2 --network ps-bridge alpine sleep 1d
  ```
  
  Now 2 containers are attached to the newly created network bridge.
  
  - let us inspect the network again
  ```
  docker network inspect ps-bridge
  ```
  
  Now you see 2 attached containers in that network with MAC and IP-Adresses.
  
  - now let us test the connectivity between both containers
  - let us grab a shell into the first container
  ```
  docker exec -it container1 sh
  ```
  - within that shell we can check for the ip address again. it is exactly the ip address of the container that we have seen inspecting the pluralsight bridge a few bullet points up.
  ```
  ip a
  ```
  - from here we can successfully ping the other host
  ```
  ping 10.0.0.3
  ```
  
  - Since both containers are part of the same network name resolution works. Thus we can ping by name.
  ```
  ping container2
  ```
  
We now have a virtual switch, added two containers, they automatically got IP adresses and now they can speak to each other using their respective names. 
