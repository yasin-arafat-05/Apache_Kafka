# Apache_Kafka:

<br>

# `#01 Installation in Arch Linux:`

<br>

- To, install the latest version please visit the apache kafka website and copy paste the link only.

    ```bash
        wget https://dlcdn.apache.org/kafka/3.9.1/kafka_2.13-3.9.1.tgz
    ```

- Unzip the file:
    ```bash
        sudo tar -xvf kafka_2.13-3.9.1.tgz
    ```
<br>

# `#02 Run Apache kafka with zookeeper:`

<br>

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
<br>

**But, in recent, instead of zookeeper we use KRaft. One, disadvantage of using zookkeeper is we need to run two sever, one for zookeeper and one for kafka. But, if we use KRaft we are able to run kafka only with one server.**

<br>

# `#03 Run Apache kafka with KRaft:`

<br>

- Generate a random UUID
    ```bash
        sudo ./bin/kafka-storage.sh random-uuid
    ```

- Format a store directory with the generated uuid
    ```bash
        sudo ./bin/kafka-storage.sh format -t ZiRvJFNWQ_qPDBqTTmDW4g -c ./config/kraft/server.properties
    ```
    Output:
    ```text
    Formatting metadata directory /tmp/kraft-combined-logs with metadata.version 3.9-IV0.
    ```

- Now, start the server:
    ```bash
        sudo ./bin/kafka-server-start.sh ./config/kraft/server.properties
    ```

