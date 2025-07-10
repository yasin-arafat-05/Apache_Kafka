# Apache_Kafka:

# `#01 Installation in Arch Linux:`
- To, install the latest version please visit the apache kafka website and copy paste the link only.

    ```bash
        wget https://dlcdn.apache.org/kafka/3.9.1/kafka_2.13-3.9.1.tgz
    ```

- Unzip the file:
    ```bash
        sudo tar -xvf kafka_2.13-3.9.1.tgz
    ```

# `#02 Run Apache kafka with zookeeper:`

- 1st go to the root unzip folder
    ```bash
        cd kafka_2.13-3.9.1
    ```

- Then, run the zookeeper server**(need to give script file from bin and config file from config)**
    ```bash
        sudo ./bin/zookeeper-server-start.sh ./config/zookeeper.properties
    ```

- Then, run the kafka server **(need to give script file from bin and config file from config)**
    ```bash
        sudo ./bin/kafka-server-start.sh ./config/server.properties
    ```
