# Proyecto de Streaming con Kafka y Spark

Este proyecto utiliza **Apache Kafka** y **Spark Streaming** en Python para simular la generación de datos de sensores, transmitirlos a través de Kafka, y procesarlos en tiempo real con Spark Streaming.

## Requisitos Previos

1. **Python** instalado en el sistema.
2. **Apache Kafka** y **ZooKeeper** configurados.
3. **Apache Spark** con soporte para **Spark Streaming y Kafka**.

## Configuración y Ejecución

### Paso 1: Instalación y configuración de Kafka

1. **Descargar Apache Kafka**:
   ```bash
   wget https://downloads.apache.org/kafka/3.6.2/kafka_2.13-3.6.2.tgz
   ```

2. **Descomprimir el archivo**:
   ```bash
   tar -xzf kafka_2.13-3.6.2.tgz
   ```

3. **Mover Kafka a una ubicación permanente**:
   ```bash
   sudo mv kafka_2.13-3.6.2 /opt/Kafka
   ```

### Paso 2: Iniciar ZooKeeper y el servidor de Kafka

1. **Iniciar ZooKeeper**:
   ```bash
   sudo /opt/Kafka/bin/zookeeper-server-start.sh /opt/Kafka/config/zookeeper.properties &
   ```

2. **Iniciar el servidor de Kafka**:
   ```bash
   sudo /opt/Kafka/bin/kafka-server-start.sh /opt/Kafka/config/server.properties &
   ```

### Paso 3: Crear el topic para el broker de mensajeria Kafka

1. **Crear un topic para transmitir los datos**:
   ```bash
   /opt/Kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic sensor_data
   ```

### Paso 4: Ejecutar el Productor (`kafka_producer.py`)

1. **Instalar la biblioteca de Kafka para Python**:
   ```bash
   pip install kafka-python
   ```

2. **Ejecutar el script del productor**:
   - Este script genera datos simulados de sensores y los envía al tema `sensor_data`.
   ```bash
   python3 kafka_producer.py
   ```

### Paso 5: Ejecutar el Consumidor (`spark_streaming_consumer.py`)

1. **Ejecutar el script del consumidor con Spark**:
   - Asegúrate de que el productor está generando datos antes de iniciar el consumidor.
   ```bash
   spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.3 spark_streaming_consumer.py
   ```

## Descripción de los Scripts

- **kafka_producer.py**: Este script simula datos de sensores (temperatura, humedad, y marca de tiempo) y los envía a Kafka cada segundo.
- **spark_streaming_consumer.py**: Este script lee datos en tiempo real desde Kafka, los procesa en ventanas de tiempo de 1 minuto, y calcula el promedio de temperatura y humedad para cada sensor.

## Notas Adicionales

1. Es importante iniciar el productor (`kafka_producer.py`) antes de ejecutar el consumidor (`spark_streaming_consumer.py`) para asegurar que haya datos disponibles.
2. Los resultados del procesamiento se muestran en la consola de Spark Streaming.
