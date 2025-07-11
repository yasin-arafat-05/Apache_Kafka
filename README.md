# Apache_Kafka:

<br>

# `#01 Installation in Arch Linux:`

<br>

- **To, install the latest version please visit the apache kafka website and copy paste the link only.**

    ```bash
        wget https://dlcdn.apache.org/kafka/3.9.1/kafka_2.13-3.9.1.tgz
    ```

- **Unzip the file**
    ```bash
        sudo tar -xvf kafka_2.13-3.9.1.tgz
    ```
<br>

# `#02 Run Apache kafka with zookeeper:`

<br>

- **1st go to the root unzip folder**
    ```bash
        cd kafka_2.13-3.9.1
    ```

- Then, run the zookeeper server **(need to give script file from bin and config file from config)**
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

- **Generate a random UUID**
    ```bash
        sudo ./bin/kafka-storage.sh random-uuid
    ```

- **Format a store directory with the generated uuid**
    ```bash
        sudo ./bin/kafka-storage.sh format -t ZiRvJFNWQ_qPDBqTTmDW4g -c ./config/kraft/server.properties
    ```
    Output:
    ```text
    Formatting metadata directory /tmp/kraft-combined-logs with metadata.version 3.9-IV0.
    ```

- **Now, start the server**
    ```bash
        sudo ./bin/kafka-server-start.sh ./config/kraft/server.properties
    ```

<br>

# `#04 kafka with producer and consumer:`
<br>

```css
+-----------+       +-----------+       +-----------+
|           |       |  Broker   |       |           |
| Producer  | ----> | test-topic| ----> | Consumer  |
|           |       |           |       |           |
+-----------+       +-----------+       +-----------+
```              

- **Start the server (prefer: with kraft)**

- **Create a topic**
    - "--topic" Topic name for this example: "test-topic"
    - "--bootstrap-server" kraft/zokeeper server running on our computer.
    - "--partitions" and "--replication-factor" see the basic note.
    ```bash
        sudo bin/kafka-topics.sh --create --topic TOPIC_NAME --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
    ```
    output: **Created topic test-topic.**

- **Producer, (Testing purpouse we have producer script file)**
    -- "--topic" topic name, previously we create **test-topics**
    ```bash
    sudo ./bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test-topic
    ```
- **Consumer, (Simillary, we have also consumer-script)**
    - "--from-beginning" if we don't create our cusumer but producer have already broadcast message to get all the message from beginning.
    ```bash
         sudo ./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning
    ```

<br>

# `#04 kafka with multiple partitions and consumers:`
<br>

```text 
┌───────────────────────────────────────┐
│              Producer                 │
│  (Writes to Topic A with Keys A/B)    │
└───────────────────────────────────────┘
                    │
                    ▼
┌────────────────────────────────────────┐
│              Order                     │
├────────────────────────────────────────┤
│  Key A                                 │
│  ┌─────────────────────────────────┐   │
│  │         Partition 0             │   │
│  ├─────────────────────────────────┤   │
│  │ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ ...     │   │
│  │ 0 │ 1 │ 2 │ 3 │ 4 │ 5 │ ...     │   │
│  │ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ ...     │   │
│  └─────────────────────────────────┘   │
│                                        │
│  Key B                                 │
│  ┌─────────────────────────────────┐   │
│  │         Partition 1             │   │
│  ├─────────────────────────────────┤   │
│  │ 1 │ 2 │ 3 │ 4 │ 5 │ ...         │   │
│  │ 0 │ 1 │ 2 │ 3 │ 4 │ ...         │   │
│  │ 1 │ 2 │ 3 │ 4 │ 5 │ ...         │   │
│  └─────────────────────────────────┘   │
└────────────────────────────────────────┘
                    │
                    ▼
┌───────────────────────────────────────┐
│           Consumer Group              │
├───────────────────────────────────────┤
│ ┌─────────────┐  ┌─────────────┐      │
│ │ Consumer    │  │ Consumer    │      │
│ │ (key-A:p0)  │  │ (key-A:p1)  │      │
│ └─────────────┘  └─────────────┘      │
└───────────────────────────────────────┘
```

- **Create topic**
    ```bash
     sudo ./bin/kafka-topics.sh \
    --create \
    --topic order \
    --bootstrap-server localhost:9092 \
    --partitions 2 \
    --replication-factor 1
    ```

- **Create producer**
    - Key will separate with : 
    ```bash
    sudo ./bin/kafka-console-producer.sh \
    --bootstrap-server localhost:9092 \
    --topic order \
    --property parse.key=true \
    --property key.separator=:
    >keyA:Message 1 
    >keyB:Message 2
    ```

- **Create consumer-1**
    ```bash
    sudo ./bin/kafka-console-consumer.sh \
    --bootstrap-server localhost:9092 \
    --topic order \
    --group og 
    ```
    
- **Create consumer-2**
    ```bash
    sudo ./bin/kafka-console-consumer.sh \
    --bootstrap-server localhost:9092 \
    --topic order \
    --group og 
    ```
**Send Multiple message and try to understand how message is send in multiple consumer group.**

- List all the Topic:
    ```bash
    sudo ./bin/kafka-topics.sh \
    --bootstrap-server localhost:9092 \
    --list
    ```

- **Describe a particular topic**
    ```bash
     sudo ./bin/kafka-topics.sh \
    --bootstrap-server localhost:9092 \
    --topic order --describe
    ```
    **Output:**
    ```text
    Topic: order    TopicId: qEJx3jj7SpydMFHnM8tKyw PartitionCount: 2       ReplicationFactor: 1 Configs: segment.bytes=1073741824
        Topic: order    Partition: 0    Leader: 1       Replicas: 1     Isr: 1       Elr:    LastKnownElr: 
        Topic: order    Partition: 1    Leader: 1       Replicas: 1     Isr: 1       Elr:    LastKnownElr: 
    ```

- **Check all groups**
    ```bash
        sudo ./bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
    ```

- **Describe a particular group**
    - Here, og is group name.
    ```bash
    sudo ./bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group og --describe
    ```


