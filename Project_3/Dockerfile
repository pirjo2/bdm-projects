FROM quay.io/jupyter/pyspark-notebook:spark-3.5.3

RUN mamba install --yes 'delta-spark' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER root

RUN echo 'spark.sql.extensions io.delta.sql.DeltaSparkSessionExtension' >> "${SPARK_HOME}/conf/spark-defaults.conf" && \
    echo 'spark.sql.catalog.spark_catalog org.apache.spark.sql.delta.catalog.DeltaCatalog' >> "${SPARK_HOME}/conf/spark-defaults.conf"

USER ${NB_UID}

# Trigger download of delta lake files
RUN echo "from pyspark.sql import SparkSession" > /tmp/init-delta.py && \
    echo "from delta import *" >> /tmp/init-delta.py && \
    echo "spark = configure_spark_with_delta_pip(SparkSession.builder).getOrCreate()" >> /tmp/init-delta.py && \
    python /tmp/init-delta.py && \
    rm /tmp/init-delta.py

# Install GraphFrames JAR manually
USER root
RUN curl -L -o /home/graphframes.jar https://repos.spark-packages.org/graphframes/graphframes/0.8.2-spark3.5-s_2.12/graphframes-0.8.2-spark3.5-s_2.12.jar && \
    mv /home/graphframes.jar ${SPARK_HOME}/jars/

USER ${NB_UID}
