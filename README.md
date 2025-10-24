# Tarea-3---BigData
### Features

<h1>♻️ Análisis en tiempo real de compraventas de residuos sólidos en Colombia</h1>

<p>
Este proyecto hace parte del curso de  Big Data (código y grupo 202016911_2) y tiene como propósito implementar un sistema de procesamiento en tiempo real usando Apache Kafka y Apache Spark Streaming para analizar datos relacionados con la compraventa de residuos sólidos en Colombia .
</p>

<hr>

<h2>⚙️ Instalación y configuración</h2>

<h3>1️⃣ Instalar dependencias</h3>
<pre><code>pip install kafka-python
</code></pre>

<h3>2️⃣ Descargar e instalar Apache Kafka 3.7.2</h3>
<pre><code>wget https://downloads.apache.org/kafka/3.7.2/kafka_2.13-3.7.2.tgz
tar -xzf kafka_2.13-3.7.2.tgz
sudo mv kafka_2.13-3.7.2 /opt/Kafka
</code></pre>

<h3>3️⃣ Iniciar los servicios de Kafka</h3>
<pre><code>sudo /opt/Kafka/bin/zookeeper-server-start.sh /opt/Kafka/config/zookeeper.properties &
sudo /opt/Kafka/bin/kafka-server-start.sh /opt/Kafka/config/server.properties &
</code></pre>

<h3>4️⃣ Crear el tópico</h3>
<pre><code>/opt/Kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic compraventas
</code></pre>

<hr>

<h2>📁 Estructura del proyecto</h2>

<pre><code>├── rows.csv
├── kafka_producer.py
├── spark_streaming_consumer.py
</code></pre>

<hr>

<h2>🧩 Descripción de los archivos</h2>

<h3>rows.csv</h3>
<p>
Es el <strong>dataset base</strong> obtenido desde <em>Datos Abiertos de Colombia</em>, correspondiente a los 
<strong>Sitios de aprovechamiento de residuos sólidos</strong>.
</p>
<p><strong>Campos:</strong></p>
<ul>
  <li>AÑO</li>
  <li>ID_COMPRAVENTA</li>
  <li>COORDENADA_X</li>
  <li>COORDENADA_Y</li>
  <li>POINT</li>
</ul
<p>
Este archivo es la <strong>fuente de datos</strong> que se usa para generar los registros enviados en tiempo real al sistema Kafka.
</p>

<h3>kafka_producer.py</h3>
<p>
Corresponde al <strong>código del productor de Kafka</strong>. Este script se encarga de:
</p>
<ul>
  <li>Leer el dataset (<code>rows.csv</code>) utilizando <strong>Polars</strong>.</li>
  <li>Limpiar los datos y transformarlos al formato adecuado.</li>
  <li>Seleccionar registros aleatorios para simular un flujo continuo de información.</li>
  <li>Enviar los datos en formato <strong>JSON</strong> al topic llamado <code>"compraventas"</code> en el servidor de Kafka.</li>
</ul>
<h3>spark_streaming_consumer.py</h3>
<p>
Es el <strong>consumidor de datos</strong>, implementado con <strong>Apache Spark Structured Streaming</strong>.  
Su función es:
</p>
<ul>
  <li>Conectarse al mismo topic <code>"compraventas"</code>.</li>
  <li>Recibir los mensajes en formato <strong>JSON</strong> enviados por el Producer.</li>
  <li>Analizar los datos y <strong>contar la cantidad de compraventas por año</strong> en tiempo real.</li>
  <li>Mostrar los resultados continuamente en consola mientras el flujo de datos está activo.</li>
</ul>

<hr>

<h2>🚀 Ejecución</h2>

<p>En una terminal, ejecutar el productor:</p>
<pre><code>python3 kafka_producer.py
</code></pre>

<p>En otra terminal, ejecutar el consumidor:</p>
<pre><code>spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.3 spark_streaming_consumer.py
</code></pre>

<p>Para visualizar la interfaz de seguimiento de Spark Streaming:</p>
<pre><code>http://localhost:4040
</code></pre>

<hr>

<h2>📊 Resultados</h2>
<p>
El sistema permite <strong>visualizar en tiempo real</strong> el número de compraventas por año según los datos enviados desde el productor.  
Esta información puede servir como base para identificar <strong>tendencias de reciclaje</strong>, 
<strong>aumentos en la actividad de recolección</strong> o <strong>zonas con mayor actividad comercial</strong> de materiales reciclables.
</p>

<hr>

<h2>🔗 Enlaces del conjunto de datos</h2>
<ul>
  <li><strong>Link CSV:</strong> <a href="https://www.datos.gov.co/api/views/h4gp-dmhe/rows.csv" target="_blank">https://www.datos.gov.co/api/views/h4gp-dmhe/rows.csv</a></li>
  <li><strong>Fuente:</strong> <a href="https://www.datos.gov.co/Ambiente-y-Desarrollo-Sostenible/Sitios-de-aprovechamiento-de-residuos-s-lidos/h4gp-dmhe" target="_blank">Sitios de aprovechamiento de residuos sólidos - Datos Abiertos de Colombia</a></li>
</ul>

<hr>

<h2>👩‍💻 Autora</h2>
<p>
<strong>María José Montes</strong><br>
Estudiante de <strong>Ingeniería de Sistemas - UNAD</strong><br>
📅 Proyecto desarrollado para el curso <strong>Big Data</strong>, año <strong>2025</strong>.
</p>
