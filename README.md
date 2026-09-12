# Oficina Prática Apache Hop

## 🎓 Escola para Capacitação em Tecnologias da Informação

Este repositório apresenta uma **oficina prática de Apache Hop**, desenvolvida para demonstrar, de forma didática, a construção de **pipelines de integração, transformação e validação de dados**, utilizando arquivos CSV como fontes de entrada.

O projeto foi organizado como um exercício prático de **ETL (Extract, Transform, Load)**, permitindo trabalhar leitura de arquivos, transformação de dados, agrupamento, validação, identificação de inconsistências e execução orquestrada por workflow.

## 🎯 Objetivo

Demonstrar a utilização do Apache Hop em um cenário de tratamento de dados acadêmicos, simulando uma rotina de processamento de cadastros.

O projeto contempla:

- leitura de catálogo de cursos;
- leitura de cadastro acadêmico;
- processamento de matrículas;
- contagem de ocorrências por matrícula;
- contagem de ocorrências por CPF;
- validação de dados cadastrais;
- identificação de possíveis duplicidades;
- geração de dados de saída;
- execução dos processos por workflow;
- tratamento de erros e geração de logs.

## 🏗️ Estrutura do projeto

```text
pratica_apache_hop/
├── Data_in/
│   ├── cadastro_academico.csv
│   └── catalogo_cursos.csv
├── Data_out/
├── Logs/
├── PipeLines/
│   ├── pl_carga_cursos.hpl
│   ├── pl_carga_matriculas.hpl
│   └── pl_validar_cadastro.hpl
├── Workflows/
│   └── wf_pipeline_indicadores_academicos.hwf
├── metadata/
├── Desenvolvimento-config.json
├── project-config.json
└── README.md
```

## 🔄 Pipelines

### `pl_carga_cursos.hpl`

Processa o catálogo de cursos a partir de `Data_in/catalogo_cursos.csv`.

### `pl_carga_matriculas.hpl`

Processa o cadastro acadêmico a partir de `Data_in/cadastro_academico.csv`, trabalhando com informações como matrícula, CPF, nome, curso, campus e situação acadêmica.

### `pl_validar_cadastro.hpl`

Realiza validações sobre o cadastro acadêmico. O fluxo agrupa os registros por **matrícula** e por **CPF**, gerando as respectivas quantidades de ocorrências e permitindo identificar possíveis duplicidades cadastrais.

## 🔗 Workflow

O projeto possui o workflow:

```text
Workflows/wf_pipeline_indicadores_academicos.hwf
```

O workflow organiza a execução dos processos e o tratamento das situações de erro, funcionando como ponto de orquestração da rotina de processamento.

## 📥 Dados de entrada

Os dados utilizados na oficina são exemplos simulados e estão disponíveis em `Data_in`.

### `catalogo_cursos.csv`

Catálogo simplificado de cursos utilizado como referência para o processamento.

### `cadastro_academico.csv`

Cadastro acadêmico utilizado nas etapas de processamento e validação, contendo informações como matrícula, CPF, nome, curso, campus e situação acadêmica.

> Os arquivos são destinados a fins didáticos e não representam dados acadêmicos reais.

## 📤 Dados de saída

Os resultados dos processamentos são direcionados para:

```text
Data_out/
```

A separação entre entrada e saída facilita a análise dos resultados e das inconsistências encontradas.

## 📊 Validações e duplicidades

Um dos principais objetivos da oficina é demonstrar como o Apache Hop pode ser utilizado em atividades de **qualidade de dados**.

O processo de validação contabiliza ocorrências de matrícula e CPF, permitindo identificar situações como:

- matrículas repetidas;
- CPFs repetidos;
- registros cadastrais inconsistentes;
- registros que precisam de tratamento antes de uma carga definitiva.

A estrutura também pode ser ampliada para novas regras de qualidade, normalização e classificação dos registros em **válidos e inválidos**.

## 📝 Monitoramento e Logs

O workflow possui tratamento de erros e geração de logs automatizada.

- **Local dos logs:** `${PROJECT_HOME}/Logs/pipeline_<data>_<hora>.log`.
- **Tratamento de erros:** em caso de falha em um dos pipelines, o workflow registra o erro, apresenta uma mensagem de `Abort` e interrompe a execução para evitar a continuidade do processamento em situação inconsistente.
- **Verificação:** consulte a mensagem de `Abort` na interface do Apache Hop ou o arquivo de log correspondente na pasta `Logs`.

## ⚙️ Configuração do projeto

O arquivo `project-config.json` define a estrutura de diretórios utilizada pelo projeto, incluindo `Data_in` para arquivos de entrada, `metadata` para metadados e o diretório do projeto como base de execução e testes.

Os pipelines utilizam a variável:

```text
${PROJECT_HOME}
```

Assim, os caminhos dos arquivos permanecem relativos ao projeto, facilitando sua execução em diferentes ambientes.

## 🚀 Como executar

1. Instale o **Apache Hop**.
2. Clone o repositório:

```bash
git clone https://github.com/wendelvox/pratica_apache_hop.git
```

3. Abra o projeto no **Hop GUI**.
4. Verifique a configuração do projeto através do `project-config.json`.
5. Confira os arquivos existentes em `Data_in`.
6. Abra o workflow:

```text
Workflows/wf_pipeline_indicadores_academicos.hwf
```

7. Execute o workflow.
8. Consulte os resultados em `Data_out` e os registros de execução em `Logs`.

## 🧩 Conceitos praticados

- ETL;
- pipelines;
- workflows;
- transformação de dados;
- leitura e escrita de CSV;
- agrupamento de registros;
- contagem de ocorrências;
- identificação de duplicidades;
- validação de cadastros;
- tratamento de erros;
- geração de logs;
- organização de projetos Apache Hop.

## 🛠️ Tecnologias

- **Apache Hop** — plataforma de orquestração e integração de dados;
- **CSV** — formato utilizado como fonte de dados;
- **Git/GitHub** — versionamento e disponibilização do projeto.

## 📚 Referência

O projeto utiliza o **Apache Hop**, plataforma voltada à orquestração de dados e metadados por meio de pipelines e workflows.

Documentação oficial: https://hop.apache.org/

## 👨‍💻 Autor

**Wendel S. Santos**

Projeto desenvolvido para fins de estudo, capacitação e demonstração prática de processos de integração e qualidade de dados com Apache Hop.
