# Prescription Acknowledgment Processing with Apache Flink

## Project Overview
This project processes prescription acknowledgment messages from a Kafka topic using Apache Flink and stores them in a PostgreSQL database. The application listens to the `prescriptions-ack` topic, processes incoming JSON messages, extracts relevant information, and inserts or updates the records in the database.

## Technologies Used
- **Apache Flink** for real-time stream processing
- **Apache Kafka** for message ingestion
- **PostgreSQL** for database storage
- **Maven** for dependency management and build automation
- **Jackson** for JSON parsing

## Project Structure
```
├── src/main/java/org/example
│   ├── BusinessLogic.java  # Processes Kafka messages and inserts data into PostgreSQL
│   ├── Consumer.java       # Kafka consumer configuration
│   ├── PrescriptionAck.java # Data model for acknowledgments
│── pom.xml                 # Maven dependencies and build configurations
```

## Installation and Setup
### Prerequisites
- Java 17
- Apache Flink
- Apache Kafka
- PostgreSQL
- Maven

### Steps to Run
1. **Clone the repository**
   ```sh
   git clone <repository-url>
   cd Prescription_Ackn
   ```

2. **Configure PostgreSQL**
    - Ensure PostgreSQL is running.
    - Update the connection details in `BusinessLogic.java`:
      ```java
      new JdbcConnectionOptions.JdbcConnectionOptionsBuilder()
          .withUrl("jdbc:postgresql://localhost:5432/HIE_db")
          .withDriverName("org.postgresql.Driver")
          .withUsername("postgres")
          .withPassword("root")
      ```

3. **Start Kafka Brokers**
    - Ensure the Kafka brokers are running with the following configuration in `Consumer.java`:
      ```java
      props.setProperty("bootstrap.servers", "154.120.216.119:9093,102.23.123.251:9093,102.23.120.153:9093");
      ```

4. **Build the Project**
   ```sh
   mvn clean install
   ```

5. **Run the Flink Job**
   ```sh
   flink run -c org.example.BusinessLogic target/Prescription_Ackn-1.0-SNAPSHOT.jar
   ```

## How It Works
1. The **Kafka consumer** reads messages from the `prescriptions-ack` topic.
2. The **BusinessLogic** class processes the messages, extracting timestamps, acknowledgment codes, and message IDs.
3. The data is **inserted or updated** in the `prescription_ack` table of the PostgreSQL database.
4. The system ensures **idempotency** using `ON CONFLICT (message_id) DO UPDATE` to avoid duplicate inserts.

## Dependencies
The project uses the following Maven dependencies:
```xml
<dependency>
    <groupId>org.apache.flink</groupId>
    <artifactId>flink-streaming-java</artifactId>
    <version>1.19.1</version>
</dependency>
<dependency>
    <groupId>org.apache.flink</groupId>
    <artifactId>flink-connector-kafka</artifactId>
    <version>1.19.1</version>
</dependency>
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.5.0</version>
</dependency>
```

## Future Enhancements
- Implementing error handling and logging mechanisms.
- Enhancing Kafka consumer resilience.
- Adding monitoring and alerting for Flink jobs.
- Implementing a testing framework for Flink pipelines.

## Contributors
- [Your Name]
- [Other Contributors]

## License
This project is licensed under the MIT License.

