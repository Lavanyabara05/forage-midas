# Midas Core: Real-Time Event-Driven Transaction Engine
### J.P. Morgan & Chase - Software Engineering Job Simulation

[![Java](https://img.shields.io/badge/Java-21-orange)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2.5-brightgreen)](https://spring.io/projects/spring-boot)
[![Kafka](https://img.shields.io/badge/Apache%20Kafka-3.1.4-blue)](https://kafka.apache.org/)



## 🇺🇸 English Description
**Midas Core** is a high-throughput financial transaction engine built during the J.P. Morgan & Chase Software Engineering simulation. The system processes a stream of financial transactions via **Apache Kafka**, validates account balances, and persists data using **Spring Data JPA**.

### Key Features:
* **Event-Driven Architecture:** Utilizes Kafka Consumers to listen for real-time transaction events.
* **Data Integrity:** Implements `@Transactional` logic to ensure atomic balance updates between senders and recipients.
* **Automated Testing:** Verified using **Embedded Kafka** and **JUnit 5** to simulate production messaging environments.
* **Environment Mastery:** Configured complex Maven build lifecycles and environment-specific application profiles.

### Tech Stack:
* Backend:Java 21, Spring Boot 3.x
* Messaging:Apache Kafka
* Database: H2 (In-memory), Spring Data JPA
* Build Tool:Maven

 Implementation Highlights

Kafka Consumer (Transaction Listener)
This component uses `@KafkaListener` to ingest real-time transaction data from the Kafka topic.

```java
@Component
public class KafkaConsumer {
    @Autowired
    private DatabaseConduit databaseConduit;

    @Transactional
    @KafkaListener(topics = "${general.kafka-topic}", groupId = "midas-core-group")
    public void consume(Transaction transaction) {
        // Logic to process the incoming transaction
        UserRecord fromUser = databaseConduit.findUser(transaction.getSenderId());
        UserRecord toUser = databaseConduit.findUser(transaction.getRecipientId());

        if (fromUser != null && toUser != null && fromUser.getBalance() >= transaction.getAmount()) {
            fromUser.setBalance(fromUser.getBalance() - transaction.getAmount());
            toUser.setBalance(toUser.getBalance() + transaction.getAmount());

            databaseConduit.save(fromUser);
            databaseConduit.save(toUser);
        }
    }
}

 2. Database & Entity Integration
Since you have a strong interest in database concepts and referential integrity, adding this section highlights your ability to map objects to a database.

```markdown
 Data Persistence (JPA & H2)
The project utilizes Spring Data JPA to manage user balances within an in-memory H2 database.

```java
@Entity
public class UserRecord {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private float balance;

    // Getters and Setters
}

## 🇯🇵 日本語の説明 (Japanese Description)
**Midas Core** は、J.P. Morgan & Chase のソフトウェアエンジニアリング実習中に構築された高スループットの金融取引エンジンです。このシステムは、**Apache Kafka** を介して送られてくる取引ストリームを処理し、口座残高を検証し、**Spring Data JPA** を使用してデータを保存します。

### 主な機能:
* **イベント駆動型アーキテクチャ:** Kafka Consumer を使用して、リアルタイムの取引イベントをリスニングします。
* **データの整合性:** `@Transactional` ロジックを実装し、送金者と受取人の間のアトミックな残高更新を保証します。
* **自動テスト:** **Embedded Kafka** と **JUnit 5** を使用して、本番環境のメッセージングをシミュレートし、動作を検証しました。

### 使用技術:
* **バックエンド:** Java 21, Spring Boot 3.x
* **メッセージング:** Apache Kafka
* **データベース:** H2 (インメモリ), Spring Data JPA
* **ビルドツール:** Maven

