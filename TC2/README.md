# Tarea Corta 2 - IC4302 Bases de Datos II

## Introduction
Following is the documentation for Tarea Corta 2 of the course IC4302 - Databases II. This homework involves developing an application that interacts with multiple database systems, including MariaDB, PostgreSQL, ElasticSearch, MongoDB, Neo4j and CouchDB. It's goal is to implement basic recovery strategies for SQL and NoSQL databases through automatic backup and restore processes. 

The application is deployed in a Kubernetes environment using Helm Charts, ensuring scalability, manageability, and observability. Each component is containerized using Docker, packaging only the necessary dependencies and database interactions. 


## Team Members
- Fabricio Solis 
- Pavel Zamora 

## Objectives

The key objectives for this homework assignment are:

1. **Deploy Databases Using Helm Charts**: Set up each database in a Kubernetes cluster, deploying them using Helm Charts.
   
2. **Implement Backup and Restore Strategies**: Create automated backup and restore processes for each database. 
   
   - **Backup Storage**: Store backups in AWS, tagged with the creation date and time (format: YYYYMMDDHHMM).
   - **Backup Retention (Optional)**: Implement a system to keep the latest *n* backups.
  
3. **Create Kubernetes Jobs for Backups and Restorations**: Utilize Kubernetes CronJobs and Jobs to handle periodic and on-demand backup and restore tasks.
   
## System Architecture
The system is containerized using Docker and deployed on Kubernetes. Each database instance has dedicated Helm Charts, enabling automated configuration, scaling, and observability. The architecture includes:

- **Database Containers**: Each database engine (MariaDB, PostgreSQL, Elasticsearch, MongoDB, Neo4j, CouchDB) runs in its own container, with resource limits and dependencies isolated for efficient management.
  
- **Helm Chart for Backups**: A separate Helm Chart, `backups`, is created to manage backup and restore configurations.
  
- **AWS Storage**: Backups are uploaded to AWS, ensuring secure and reliable storage with time-stamped filenames for easy retrieval.

## Implementation
1. **Database Helm Charts**: 
   - Deploy each database using Helm Charts with required configurations.
   - Ensure each database can scale based on Kubernetes resource allocation.
   
2. **Backup Process**:
   - Use a Kubernetes CronJob for scheduled backups, creating snapshots that are uploaded to AWS.
   - Include a Kubernetes Job for on-demand backups as needed.
   - Implement optional retention management, allowing configuration of the number of recent backups to retain in AWS.
   
3. **Restore Process**:
   - Create a Kubernetes Job to restore a specified backup. The restore configuration allows users to specify the timestamp of the desired backup.

Next up, you will find the requirements to run the application, the steps to execute it, testing examples and the recommendations and conclusions we have gathered from the homework.

## Requirements

The requirements for the homework are the following:

* Create an user in [DockerHub](https://hub.docker.com/)
* Install [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/), if you are using MacOS, please make sure you select the right installer for your CPU architecture.
* Open Docker Desktop, go to **Settings > Kubernetes** and enable Kubernetes.
![K8s](./images/docker-desktop-k8s.png "K8s Docker Desktop")
* Install [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/)
* Install [Helm](https://helm.sh/docs/intro/install/)
* Install [Visual Studio Code](https://code.visualstudio.com/)
* Install [Lens](https://k8slens.dev/)


## Building the docker images

There is a script to build the docker images, to execute it in a bash shell execute:

```bash
cd ./TC2/docker
./build.sh nereo08
```

Change **nereo08** to your DockerHub username

Please take a look on the script contents to make sure you understand what is done under the hood.
This script will build the images for the components of the homework, including the databases and the backup service.

## Helm Charts

### Configure

* Open the file **TC2/charts/application/values.yaml**
* Replace **nereo08** by your DockerHub username

```yaml
config:
  docker_registry: nereo08
```

### Install

Execute:

```bash
cd ./TC2/charts
./install.sh
```

Here we are installing the components in the Kubernetes cluster. The script will install the databases. It is important to have the DockerHub username set correctly in the script to ensure the correct components are installed.

### Uninstall

Execute:

```bash
cd ./TC2/charts
./uninstall.sh
```

This script will uninstall the components from the Kubernetes cluster. It is important to have the DockerHub username set correctly in the script to ensure the correct components are removed.

## Access Debug Pod

```bash
# copy the name that says debug from the following command
kubectl get pods
# then replace debug-844bb45d6f-9jt45 by that name
kubectl exec --stdin --tty debug-844bb45d6f-9jt45 -- /bin/bash
```
In case you need to access the debug pod to check the logs or execute some commands, you can use the previous command to access it.


# How to Test

To test the whole homework together, you can follow the next steps:
First you need to do the build and install steps, in this time you have to choose the components that will be used for this you have to change in TC2\charts\application\values.yaml, after that to verify that the homework is working correctly you can follow the next steps:

- Check that the Docker images are running; you can verify this in the Docker Desktop application. In this area, it’s essential to monitor the different images, as you can review their logs to identify any potential errors. 

- After that you can check in Lens that the pods are running correctly, here you can check the logs of the pods to see if there are any errors and the status of the pods.



# Recommendations and Conclusions

## Recommendations

To successfully complete the homework, the following recommendations are provided to help you navigate the different components and technologies involved:

1. **Learn How to Use a Kubernetes Cluster**
   - **Recommendation**: It is very important to get a basic understanding of Kubernetes, what is their function and how it works, as the whole homework is designed to run in a Kubernetes cluster.
   - **Reason**: Knowing how to deploy and manage the application in Kubernetes will help you run the homework in a real-world environment.

2. **Learn How to Use Helm Charts**
   - **Recommendation**: Learn how to use Helm charts, which are used to deploy the application in Kubernetes. It is relevant to understand how they work and what it their function so you can deploy the application correctly.
   - **Reason**: The homework uses Helm charts to deploy the application in Kubernetes. Knowing how to use Helm charts will help you manage the deployment of the application.

3. **Get Familiar with AWS S3 Storage**
   - **Recommendation**: Study AWS S3 storage, as it is used to store the backups created by the application.
   - **Reason**: Understanding how to use AWS S3 storage will help you manage the backups created by the application and ensure they are stored correctly.

4.  **Get Kubernetes Cronjobs and Jobs**
    - **Recommendation**: Learn how to use Kubernetes CronJobs and Jobs, as they are used to schedule and run the backup and restore processes in the application.
    - **Reason**: Understanding how to use Kubernetes CronJobs and Jobs will help you manage the backup and restore processes and ensure they run correctly.
  
5.  **Monitor Resource Usage**
    - **Recommendation**: Keep an eye on resource usage (CPU, memory) while running the script to ensure it operates within acceptable limits.
    - **Reason**: Monitoring helps prevent performance bottlenecks and ensures that the script runs efficiently, particularly for large datasets.

6.  **Document Configuration and Steps**
    - **Recommendation**: Document the configuration parameters and execution steps clearly for future reference and ease of use.
    - **Reason**: Clear documentation helps users understand the setup and execution process, making it easier to troubleshoot and replicate the environment.

7. **Understand PostgreSQL**
   - **Recommendation**: Learn the basics of PostgreSQL, one of the databases used in the homework, to understand its features and functionalities.
   - **Reason**: Understanding PostgreSQL will help you manage the database, for the backup and restore processes, and ensure data integrity.

8. **Understand Database Backup and Restore Commands**
    - **Recommendation**: Familiarize yourself with each database’s native backup and restore commands (e.g., `pg_dump` for PostgreSQL, `mongodump` for MongoDB). Learm what is the purpose of each command and how to use them.
    - **Reason**: Knowing these commands ensures reliable backup and restore operations, especially when troubleshooting or running manual backups.

9.  **Understand MariaDB**
   - **Recommendation**: Learn the basics of MariaDB, another database used in the homework, to understand its features and functionalities.
   - **Reason**: Understanding MariaDB will help you manage the database, for the backup and restore processes, and ensure data integrity.
  
10. **Understand MongoDB**
    - **Recommendation**: Learn the basics of MongoDB, used as a database for storing information in the homework.
    - **Reason**: Understanding MongoDB will help you manage the database, for the backup and restore processes, and ensure data integrity.

11. **Understand Elasticsearch**
    - **Recommendation**: Learn the basics of Elasticsearch, a NoSQL database used in the homework, to understand its features and functionalities.
    - **Reason**: Understanding Elasticsearch will help you manage the database, for the backup and restore processes, and ensure data integrity.

12. **Understand Neo4j**
    - **Recommendation**: Learn the basics of Neo4j, a graph database used in the homework, to understand its features and functionalities.
    - **Reason**: Understanding Neo4j will help you manage the database, for the backup and restore processes, and ensure data integrity.
  
13. **Understand CouchDB**
    - **Recommendation**: Learn the basics of CouchDB, a NoSQL database used in the homework, to understand its features and functionalities.
    - **Reason**: Understanding CouchDB will help you manage the database, for the backup and restore processes, and ensure data integrity.
  

## Conclusions 

1. Analyzing the Desired Functionality: The homework aims to provide a comprehensive solution for managing multiple databases in a Kubernetes environment, implementing backup and restore strategies to ensure data integrity and availability. By leveraging Helm Charts, Kubernetes Jobs, and AWS storage, the system can automate these processes, enabling efficient data management and recovery capabilities.

2. Containerized Scalability: The use of Kubernetes allows the system to scale horizontally, adding or removing instances as needed to handle varying loads. This ensures that the homework can support increased data volumes and user requests, maintaining performance and reliability under dynamic conditions.

3.  Importance of Diversity of Technologies: The homework's use of multiple technologies, such as PostgreSQL, MongoDB, Elasticsearch, Neo4j, and CouchDB, provides a comprehensive view of different database systems and their unique features. This diversity allows us as students to gain experience with various databases, understanding their strengths and limitations in different scenarios.
   
4. Enhanced Data Resilience through Automated Backup and Restore Strategies: Implementing automated backup and restore processes with Kubernetes CronJobs and Jobs strengthens data resilience and disaster recovery. By scheduling regular, time-stamped backups and securely storing them in AWS, the system minimizes data loss risks and ensures that recent data states are preserved and readily accessible for recovery in case of failures.

5. Centralized Management with Helm Charts: Helm Charts offer a centralized way to manage configurations and deployment, making it easier to standardize setups across multiple databases. This uniformity simplifies management and maintenance, especially when scaling or updating deployments.

6. Seamless Integration with AWS for Storage: Using AWS S3 for backup storage ensures scalability and reliability in data storage. S3’s secure, high-availability infrastructure complements the Kubernetes-based deployment, providing dependable storage for critical data.

7. Enhanced Understanding of Database-Specific Backup Needs: This homework has highlighted the distinct backup and restore requirements of each database system (e.g., pg_dump for PostgreSQL, mongodump for MongoDB). This experience is valuable in recognizing how various databases approach data management and disaster recovery.

8. Resource Optimization in a Kubernetes Cluster: Monitoring CPU and memory usage across database pods emphasizes the importance of resource optimization. This helps maintain stable operations under various loads and teaches the practicalities of resource management within containerized applications.

9.  Real-World Applicability of Kubernetes Automation: Automating database operations through Kubernetes not only demonstrates technical proficiency but also reflects real-world practices in enterprise environments, where automation is critical for efficiency and reliability.

10. Flexibility of Kubernetes in Managing Diverse Workloads: The ability to deploy both SQL and NoSQL databases on a single Kubernetes cluster underscores Kubernetes' flexibility in managing diverse workloads. This adaptability is essential for modern applications that require different data storage solutions in one environment.

# Components

## Databases:

The databases used in the homework are PostgreSQL, MariaDB, MongoDB, Elasticsearch, Neo4j, and CouchDB. Each database has its own container and Helm Chart for deployment in Kubernetes. The databases should be configured with resource limits and dependencies to ensure efficient management and scalability.

PostgreSQL is a powerful, SQL-based relational database that provides robust data storage and querying capabilities. It is widely used in enterprise applications for its reliability and performance. It is one of the most popular open-source databases, known for its ACID compliance and extensibility. PostgreSQL supports a wide range of data types, indexing options, and query optimization features, making it suitable for complex data models and high-performance applications. 

MariaDB is a popular open-source relational database management system that is compatible with MySQL. It is a SQL-based database that provides high performance, scalability, and reliability. MariaDB is widely used in web applications, e-commerce platforms, and enterprise systems for its flexibility and ease of use. It supports ACID compliance, transactions, and advanced indexing options, making it a reliable choice for data storage and retrieval.

MongoDB is a NoSQL database that stores data in a flexible, JSON-like format. It is designed for scalability, high availability, and performance, making it suitable for modern applications that require fast data access and processing. MongoDB is widely used in web applications, mobile apps, and IoT systems for its flexibility and ease of use. It supports dynamic schemas, sharding, and replication, allowing developers to build scalable and resilient data platforms.

Elasticsearch is a distributed search engine that is designed for real-time data processing and analysis. It provides powerful search capabilities, full-text indexing, and data visualization features. Elasticsearch is widely used in log analysis, monitoring, and search applications for its speed, scalability, and ease of use. It supports complex queries, aggregations, and data visualization tools, making it a versatile platform for data exploration and analysis.

Neo4j is a graph database that stores data in nodes, relationships, and properties. It is designed for connected data and complex relationships, making it suitable for social networks, recommendation engines, and knowledge graphs. Neo4j is widely used in graph analytics, network analysis, and fraud detection for its performance and scalability. It supports graph queries, and graph algorithms, allowing developers to build powerful graph-based applications.

CouchDB is a NoSQL database that stores data in a document-oriented format. It is designed for distributed data storage, replication, and synchronization, making it suitable for offline-first applications and mobile devices. CouchDB is widely used in content management, collaboration, and data synchronization for its flexibility and scalability. It supports JSON documents, ACID compliance, and multi-master replication, allowing developers to build resilient and distributed data platforms.


# References

- [1] "Dockerfile reference," Docker Documentation. [Online]. Available: https://docs.docker.com/reference/dockerfile/#overview. [Accessed: Nov. 10, 2024].

- [2] "kubectl commands," Kubernetes Documentation. [Online]. Available: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands. [Accessed: Nov. 10, 2024].

- [3] "PostgreSQL Documentation," PostgreSQL Documentation. [Online]. Available: https://www.postgresql.org/docs/. [Accessed: Nov. 10, 2024].

- [4] "MongoDB Documentation," MongoDB Documentation. [Online]. Available: https://docs.mongodb.com/. [Accessed: Nov. 10, 2024].
  
- [5] "Elasticsearch Documentation," Elasticsearch Documentation. [Online]. Available: https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html. [Accessed: Nov. 10, 2024].

- [6] "Neo4j Documentation," Neo4j Documentation. [Online]. Available: https://neo4j.com/docs/. [Accessed: Nov. 10, 2024].

- [7] "CouchDB Documentation," CouchDB Documentation. [Online]. Available: https://docs.couchdb.org/en/stable/. [Accessed: Nov. 10, 2024].

- [8] "MariaDB Documentation," MariaDB Documentation. [Online]. Available: https://mariadb.com/kb/en/documentation/. [Accessed: Nov. 10, 2024].
