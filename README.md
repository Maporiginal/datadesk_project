# DataDesk Local

DataDesk é um projeto de portfólio de Engenharia de Dados que demonstra, de forma local, autocontida e explicável, como uma plataforma de dados recebe arquivos, preserva a origem, processa informações, constrói produtos analíticos e opera esse fluxo com rastreabilidade.

O objetivo não é simular uma plataforma cloud de produção. É demonstrar decisões técnicas reais de ingestão, qualidade de dados, processamento, orquestração, observabilidade, recuperação e publicação analítica em um ambiente que pode ser executado localmente com Docker.

## O que o projeto faz

- Recebe PDFs, imagens e planilhas XLSX.
- Preserva o arquivo original no MinIO e a entrada bruta no PostgreSQL.
- Enfileira o processamento de forma durável, com idempotência, limites de capacidade, lease, heartbeat e cancelamento.
- Processa documentos com OCR e planilhas com PySpark em workers desacoplados da API.
- Registra dados válidos, rejeições e reconciliações no PostgreSQL.
- Transforma dados tratados em camadas `silver` e `gold` com dbt.
- Publica dados Gold por APIs para consumo no Power BI.
- Orquestra e valida o pipeline com Airflow.
- Monitora métricas, logs, alertas, consultas PostgreSQL e recuperação com Grafana, Prometheus, Loki e Alertmanager.

## Fluxo técnico

```text
PDF / imagem / XLSX
        |
        v
FastAPI: validação, idempotência e fila durável
        |
        +--> MinIO: arquivo original
        +--> PostgreSQL raw: entrada preservada e auditoria
        |
        v
Worker OCR ou worker PySpark
        |
        v
PostgreSQL app: registros tratados e rejeições auditáveis
        |
        v
dbt: silver -> gold
        |
        +--> APIs BI -> Power BI
        |
        v
Airflow: orquestração, validações e histórico de execução
        |
        v
ops + Prometheus + Loki + Grafana + Alertmanager
```

## Tecnologias demonstradas

| Área | Tecnologias |
| --- | --- |
| API e integração | Python, FastAPI, Uvicorn |
| Banco e dados | PostgreSQL, schemas `raw`, `app`, `silver`, `gold` e `ops` |
| Armazenamento | MinIO, compatível com S3 |
| Processamento | PySpark, JDBC, OCR com Tesseract e PyMuPDF |
| Transformação analítica | dbt e dbt-postgres |
| Orquestração | Apache Airflow com LocalExecutor |
| Observabilidade | Prometheus, Grafana, Loki, Alloy, Alertmanager, cAdvisor e exporters PostgreSQL |
| Operação PostgreSQL | `pg_stat_statements`, `pg_stat_kcache`, monitoramento por PID e operador administrativo auditável |
| BI | APIs Gold, Power BI PBIP, modelo semântico e medidas DAX |
| Continuidade | backup, manifesto com hashes, restore isolado e recovery drill |
| Execução local | Docker Compose, Shell e PowerShell |

## Decisões que o projeto evidencia

- Separação entre API leve e processamento pesado: FastAPI não executa OCR, PySpark ou dbt na mesma requisição HTTP.
- Saga entre PostgreSQL e MinIO: os dois sistemas não compartilham uma única transação; estados persistentes e reconciliação tratam falhas parciais.
- Dados brutos não são descartados silenciosamente: uma linha segue para `app` ou vira rejeição rastreável.
- dbt não é armazenamento: ele transforma dados persistidos em produtos analíticos Gold consumidos por APIs e Power BI.
- Airflow orquestra tarefas; workers executam processamento; PostgreSQL mantém estado durável.
- Grafana é operacional, enquanto Power BI é a camada de apresentação analítica de negócio.
- Alertas de PostgreSQL são investigáveis por query, PID, aplicação, uso de CPU/memória e logs centralizados.

## Pacote de avaliação técnica

O arquivo [`datadesk_project_portfolio_protegido.7z`](datadesk_project_portfolio_protegido.7z) contém o projeto completo e está protegido por AES-256, incluindo os nomes dos arquivos internos.

A senha é disponibilizada no currículo ou diretamente pelo autor para avaliação técnica. Depois de extraído, o projeto pode ser iniciado com Docker:

```bash
./run.sh
```

No Windows com Docker Desktop:

```powershell
powershell -ExecutionPolicy Bypass -File .\run.ps1
```

Para abrir o pacote no Windows, use [7-Zip](https://www.7-zip.org/).

## Integridade do pacote

```text
SHA-256: bfce5eac0a121f60042ed295eeba6f0c06a089370cbe3a6904c4e99700642da1
```

## Autoria

Matheus Andrade. Projeto de portfólio destinado à demonstração e avaliação técnica em Engenharia de Dados.
