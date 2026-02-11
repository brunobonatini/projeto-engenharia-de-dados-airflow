FROM apache/spark:3.5.3

USER root

# ===============================
# Instalar Python
# ===============================
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    python3-dev \
    build-essential \
    libpq-dev \
    libffi-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Garantir que pip está atualizado
RUN pip3 install --upgrade pip

# ===============================
# Hadoop AWS (S3A)
# ===============================
RUN curl -L -o /opt/spark/jars/hadoop-aws-3.3.4.jar \
    https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.4/hadoop-aws-3.3.4.jar && \
    curl -L -o /opt/spark/jars/aws-java-sdk-bundle-1.12.262.jar \
    https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.12.262/aws-java-sdk-bundle-1.12.262.jar

# ===============================
# Delta Lake
# ===============================
RUN curl -L -o /opt/spark/jars/delta-core_2.12-3.2.0.jar \
    https://repo1.maven.org/maven2/io/delta/delta-core_2.12/3.2.0/delta-core_2.12-3.2.0.jar

# ===============================
# Configuração S3A + Delta
# ===============================
RUN mkdir -p /opt/spark/conf && \
    echo "spark.hadoop.fs.s3a.endpoint=http://minio:9000" >> /opt/spark/conf/spark-defaults.conf && \
    echo "spark.hadoop.fs.s3a.access.key=minio" >> /opt/spark/conf/spark-defaults.conf && \
    echo "spark.hadoop.fs.s3a.secret.key=minio123" >> /opt/spark/conf/spark-defaults.conf && \
    echo "spark.hadoop.fs.s3a.path.style.access=true" >> /opt/spark/conf/spark-defaults.conf && \
    echo "spark.hadoop.fs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem" >> /opt/spark/conf/spark-defaults.conf && \
    echo "spark.sql.extensions=io.delta.sql.DeltaSparkSessionExtension" >> /opt/spark/conf/spark-defaults.conf && \
    echo "spark.sql.catalog.spark_catalog=org.apache.spark.sql.delta.catalog.DeltaCatalog" >> /opt/spark/conf/spark-defaults.conf

# ===============================
# Instalar libs Python
# ===============================
COPY requirements-spark.txt .
RUN pip3 install --no-cache-dir -r requirements-spark.txt

WORKDIR /projeto

CMD ["/bin/bash"]
