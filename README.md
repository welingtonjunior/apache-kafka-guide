# 📘 Guia Completo: Apache Kafka

## 📖 Introdução
O **Apache Kafka** é uma plataforma distribuída de streaming que permite publicar, subscrever, armazenar e processar fluxos de eventos em tempo real.  
É amplamente utilizado em sistemas que exigem **alta escalabilidade, tolerância a falhas e processamento de dados em tempo real**.

---

## 🔑 Conceitos Fundamentais
- **Producer** → publica mensagens em tópicos.
- **Consumer** → lê mensagens de tópicos.
- **Broker** → servidor Kafka que armazena e distribui mensagens.
- **Topic** → canal de comunicação onde mensagens são publicadas.
- **Partition** → divisão de um tópico para paralelismo.
- **Offset** → identificador único de cada mensagem em uma partição.
- **Consumer Group** → conjunto de consumidores que compartilham a leitura de um tópico.

---

## ⚙️ Arquitetura
1. **Producers** enviam mensagens para um **topic**.  
2. O **Broker** distribui mensagens entre **partitions**.  
3. **Consumers** (ou consumer groups) leem mensagens.  
4. Kafka armazena mensagens por tempo configurado, independente do consumo.  

---

## 🛠️ Instalação e Configuração
### Pré-requisitos
- **Java 8+**
- **Zookeeper** (nas versões antigas, em versões novas Kafka pode rodar sem Zookeeper)
- Apache Kafka [📦 download](https://kafka.apache.org/downloads)

### Passos básicos
```bash
# 1. Inicie o Zookeeper
bin/zookeeper-server-start.sh config/zookeeper.properties

# 2. Inicie o Kafka Broker
bin/kafka-server-start.sh config/server.properties

# 3. Crie um tópico
bin/kafka-topics.sh --create --topic meu-topico --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

# 4. Liste tópicos
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

---

## 💻 Exemplo em Java (Producer & Consumer)
### Producer
```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

Producer<String, String> producer = new KafkaProducer<>(props);
producer.send(new ProducerRecord<>("meu-topico", "chave1", "mensagem teste"));
producer.close();
```

### Consumer
```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("group.id", "meu-grupo");
props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Collections.singletonList("meu-topico"));

while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : records) {
        System.out.printf("offset=%d, chave=%s, valor=%s%n", record.offset(), record.key(), record.value());
    }
}
```

---

## 🚀 Casos de Uso
- Processamento de **logs em tempo real**.  
- **Streaming de dados** para analytics.  
- Integração entre **microsserviços**.  
- Monitoramento e eventos em sistemas distribuídos.  

---

## 🔐 Boas Práticas
- Definir número adequado de **partições** para escalabilidade.  
- Usar **Consumer Groups** para balanceamento de carga.  
- Configurar **replicação** para tolerância a falhas.  
- Monitorar lag (atraso de consumo).  

---

## 📚 Recursos para Estudo
- [Documentação Oficial](https://kafka.apache.org/documentation/)  
- Livro: *Kafka: The Definitive Guide* – Neha Narkhede, Gwen Shapira, Todd Palino  
- Cursos: Kafka Fundamentals (Confluent)  
