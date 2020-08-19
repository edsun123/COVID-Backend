## Build/Install Instructions
#### What is contained in this repository?
 * A Java Spring Boot Backend API
 * Go code for mix network nodes (For upload anonymization)
 * Dockerfiles for the above componenets
 * Python scripts for spinning up and connecting the following containers (2 Mix Nodes -> Backend -> MySQL Database)
 * A basic client that encrypts some uuids and sends them to the backend through the mix nodes

#### Backend Dependencies
 * Java 8
 * Maven (Will install the remaining dependencies)
 
#### Mix Network Dependencies
 * Golang 1.14
 * gorilla/mux
 	* Run the following with go installed `go get -u github.com/gorilla/mux`

#### Docker Deployment Dependencies
 * Docker
 * Python3 (If you would like to use the deploy script provided)
 

#### Building Docker Images
 * Docker images are not yet hosted anywhere so will need be built manually for the time being
 * Run the following commands from the project root directory
 * Building Backend
 	* `mvn clean package`
	* `docker build -t "covid-backend" .
 * Building MixNode
 	* `docker build -t "mix-net-node" ./mixNet
 * Run `docker images` and look for "covid-backend" and "mix-net-node" to make sure that the images have built
 
#### Deploying the System with Docker
 * Run `python3 run.py` to deploy and connect the backend components
 * You will be asked to select either SQLKeySet or SQLBloomFilter (These can also be passed as an argument to run.py)
 	* See Backend Run Modes below for more details on this
 * The test client in mixnet/TestClient.go can be run with  `go run TestClient.go {MixNode0_IP:port} {MixNode1_IP:port}`. The mix node addresses set by the run.py script are localhost:8081 and localhost:8082

## Backend
### Backend Run Modes
 * The backend has 2 different key storage schemas that result in different behavior
#### SQLKeySet
 * Key Uploads: Keys are stored in plain text as they are collected in a SQL Database
 * Key Requests: Keys are returned to the user in plain text
#### SQLBloomFilter
 * Key Uploads: Keys are stored in a [Bloom Filter](https://en.wikipedia.org/wiki/Bloom_filter) and the original key is discarded
 * Key Requests: A bloom filter with the keys that have been uploaded is returned. The user can then perform their own checks against the bloom filter to check if the keys they have are *possibly* in the bloom filter
 	* Because this approach uses a bloom filter, there is a false positive rate determined by the size of the filter, number of hash functions used, and the number of entries in the filter. The current values for these parameters are meant only for testing. 
	
### Making requests to the backend
 * 
