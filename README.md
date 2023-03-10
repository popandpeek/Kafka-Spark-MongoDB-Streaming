Document Streaming 

This project shows the deployment of a streaming architecture using Docker containers that accepts invoices and serves them to an end user.

1. FastAPI server that accepts invoices and posts them to an Apache Kafka server
 
2. Apache Spark streaming instance that pulls messages from Kafka and pushes them into MongoDb

3. FastAPI server that provides endpoints to get customer and invoice data from MongoDB (and protects login information)

4. Streamlit app to display Customer and Invoice data to end user

5. See Document-Streaming-Flow.pdf for visual representation

Instructions

1. Computing Set-up
   * This project requires Docker Desktop to be installed and working with Docker Hub - https://www.docker.com/101-tutorial/
      * Ubuntu or WSL on Windows
   * VS Code with Docker extensions
	
2. Download the following images from Docker Hub
   * mongo:latest
   * mongo-express:latest
   * bitnmai/zookeeper:latest
   * bitnami/kafka:latest
   * jupyter/pyspark-notebook:spark-2

3. Build local Docker containers
   * Go to API-Ingest folder -> build-command.txt -> In terminal -> docker build -t api-ingest .
   * b. Go to API-Egest folder -> build-command.txt -> In terminal -> docker build -t api-egest . 
   * c. Go to Streamlit folder -> build-command.txt -> In terminal -> docker build -f Dockerfile -t streamlitapp:latest .

4. Start up Docker containers 
   * Go to project root folder -> In terminal -> docker-compose -f docker-compose.yml up

5. Start up Apache Kafka instance
   * In VS Code go-to Docker tab, right on click bitnami/kafka:latest container and click attach shell
   * In terminal -> cd opt/bitnami/kafka/bin
   * In terminal -> ./kafka-console-consumer.sh --topic ingestion-topic --bootstrap-server localhost:9092
   * Leave terminal running
	
6. Access Apache Spark instance
   * Goto localhost:8888 in browser
   * Get token from logs of step 4 above (See terminal)
   * Provide login (see docker-compose.yml)
   * Open work folder and choose streaming-kafka-src-dst-mongodb.ipynb workbook
   * Run all cells, last cell will continue running to await messages in Apache Kafka instance

7. Run api-client
   * VS Code -> go to client folder
   * Choose api-client and run
	
8. MongoDB
   * Go to localhost:8081 in browser (see docker-compose.yml for login information)
   * Click on docstreaming db, then invoices table

9. Streamlit
   * Go to localhost:8501 in browser
   * Choose customer id from MongoDB and enter into appropriate input box on left
   * Choose invoice if from Mongo db or Streamlit b output and enter into appropriate input box on left
