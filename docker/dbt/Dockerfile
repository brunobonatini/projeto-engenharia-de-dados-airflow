FROM python:3.11-slim

WORKDIR /projeto

# Instalar dependências do sistema
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    && rm -rf /var/lib/apt/lists/*

# Instalar dbt com versão fixa
RUN pip install --no-cache-dir dbt-postgres==1.7.9

# Criar diretório padrão do dbt
RUN mkdir -p /root/.dbt

# Copiar projeto
COPY dbt_project/ /projeto/dbt_project/

CMD ["bash"]
