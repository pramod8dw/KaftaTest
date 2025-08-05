# Apache Kafka: Demo Commands and Workflow

## 1. Start Kafka Server with Docker

Use the provided `docker-compose.yml` ( Kafka) and run:

```bash
docker-compose up
```

✅ **Tip**: Ensure port `9092` is available on your machine.

---

## 2. Download Kafka Binary Tools (Optional)

If you want to use the Kafka CLI tools locally (e.g., `kafka-topics.sh`):

📥 [Download Kafka 3.9.1](https://dlcdn.apache.org/kafka/3.9.1/kafka_2.13-3.9.1.tgz)

Then extract and navigate to the `bin/` folder:

```bash
tar -xzf kafka_2.13-3.9.1.tgz
cd kafka_2.13-3.9.1/bin
```

---

## 3. List Kafka Topics

```bash
kafka-topics.sh --list --bootstrap-server localhost:9092
```

📝 You should see either an empty list or previously created topics.

(Alternative with `kcat` if installed:)

```bash
kcat -b localhost:9092 -L
```

---

## 4. Create a Kafka Topic

```bash
kafka-topics.sh --create \
  --bootstrap-server localhost:9092 \
  --replication-factor 1 \
  --partitions 3 \
  --topic demo-topic
```

📝 Creates a topic named `demo-topic` with 3 partitions and a single replica.

To verify:

```bash
kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic demo-topic
```

---

## 5. Start a Kafka Producer

```bash
kafka-console-producer.sh \
  --broker-list localhost:9092 \
  --topic demo-topic \
  --property "parse.key=true" \
  --property "key.separator=:"
```

📝 Start entering messages in the following format:

```text
user1:Hello Kafka
user2:Another message
```

📌 Keys (`user1`, `user2`) will help control partitioning behavior.

---

## 6. Start a Kafka Consumer

```bash
kafka-console-consumer.sh \
  --bootstrap-server localhost:9092 \
  --topic demo-topic \
  --group demo-group
```

🔄 To view more details:

```bash
kafka-console-consumer.sh \
  --bootstrap-server localhost:9092 \
  --topic demo-topic \
  --group demo-group \
  --property print.partition=true \
  --property print.offset=true
```

📝 Messages will be printed with partition and offset information.

---

## 7. Describe the Consumer Group

```bash
kafka-consumer-groups.sh \
  --bootstrap-server localhost:9092 \
  --describe \
  --group demo-group
```

📌 This will show:
- Current offsets
- Lag per partition
- Partition assignment

---

## 8. Reset Consumer Group Offset (Optional)

To re-consume from the beginning:

```bash
kafka-consumer-groups.sh \
  --bootstrap-server localhost:9092 \
  --group demo-group \
  --topic demo-topic \
  --reset-offsets --to-earliest --execute
```

---

## 9. Simulate Load Balancing

In separate terminals, run multiple consumers in the same group:

```bash
# Terminal 1
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic demo-topic --group demo-group

# Terminal 2
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic demo-topic --group demo-group
```

📌 Observe how partitions are distributed across consumers (only 3 partitions = 3 max active consumers).

---

## 10. Clean Up (Optional)

Stop Docker:

```bash
docker-compose down
```

Delete the topic:

```bash
kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic demo-topic
```

---

## ✅ Summary of What Was Demonstrated

- Kafka basics: topic, producer, consumer, partition
- Partitioning with keys
- Consumer groups and load balancing
- Offset management and replayability
- Using CLI tools effectively

---

## ⚠️ Common Troubleshooting Tips

- 🔌 **Connection refused**: Ensure Docker containers are running and Kafka is listening on `localhost:9092`.
- ❗ **Command not found**: Ensure you're in the `bin/` directory of the Kafka install or add it to your `PATH`.
- ❓ **No messages received**: Check if the topic name matches exactly, and that you're using the same group ID.
