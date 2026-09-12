# Oficina Prática Apache Hop

## 🎓 Escola para Capacitação em Tecnologias da Informação

Este repositório apresenta uma **oficina prática de Apache Hop**, desenvolvida para demonstrar, de forma didática, a construção de **pipelines de integração, transformação, validação e qualidade de dados**, utilizando arquivos CSV como fontes de entrada.

O projeto também incorpora práticas de **segurança, privacidade e governança de dados**, mantendo o cenário didático sem credenciais reais e utilizando pseudonimização nas saídas destinadas à análise.

## 🎯 Objetivo

Demonstrar a utilização do Apache Hop em um cenário de tratamento de dados acadêmicos simulados.

O projeto contempla:

- leitura de catálogo de cursos;
- leitura de cadastro acadêmico;
- normalização de identificadores;
- validação de campos obrigatórios e integridade;
- contagem de ocorrências por matrícula;
- contagem de ocorrências por CPF;
- identificação de duplicidades;
- classificação de registros válidos e inválidos;
- pseudonimização de CPF, matrícula e nome nas saídas de privacidade;
- minimização dos dados exportados;
- execução dos processos por workflow;
- tratamento de erros e geração de logs;
- documentação de riscos e papéis de acesso simulados.

## 🏗️ Estrutura do projeto

```text
pratica_apache_hop/
├── Data_in/
│   ├── cadastro_academico.csv
│   └── catalogo_cursos.csv
├── Data_out/
├── Logs/
│   └── revisao_log.md
├── PipeLines/
│   ├── pl_carga_cursos.hpl
│   ├── pl_carga_matriculas.hpl
│   ├── pl_qualidade_privacidade.hpl
│   └── pl_validar_cadastro.hpl
├── Workflows/
│   └── wf_pipeline_indicadores_academicos.hwf
├── docs/
│   ├── controle_acesso.md
│   ├── riscos.md
│   ├── seguranca_e_privacidade.md
│   └── variaveis_e_segredos.md
├── metadata/
├── Desenvolvimento-config.json
├── project-config.json
└── README.md
```

## 🔄 Fluxo técnico

A rotina de qualidade segue os seguintes princípios:

```text
CSV
 ↓
Normalização
 ↓
Validação
 ↓
Identificação de duplicidades
 ↓
Classificação
 ├── Válidos
 └── Inválidos
       ↓
    Data_out
```

O pipeline `pl_validar_cadastro.hpl` realiza as regras de qualidade e o pipeline `pl_qualidade_privacidade.hpl` produz saídas minimizadas e pseudonimizadas, evitando a exportação dos identificadores pessoais originais.

## 🔄 Pipelines

### `pl_carga_cursos.hpl`

Processa o catálogo de cursos a partir de `Data_in/catalogo_cursos.csv`.

### `pl_carga_matriculas.hpl`

Processa o cadastro acadêmico a partir de `Data_in/cadastro_academico.csv`.

### `pl_validar_cadastro.hpl`

Realiza validações sobre o cadastro acadêmico. O fluxo normaliza valores, agrupa registros por **matrícula** e **CPF**, verifica campos obrigatórios, integridade, situação acadêmica, existência de curso e duplicidades e separa registros válidos de rejeitados.

### `pl_qualidade_privacidade.hpl`

Complementa o processo de qualidade com foco em privacidade. O pipeline:

1. lê os dados simulados;
2. normaliza CPF, matrícula e campos textuais;
3. gera pseudônimos determinísticos por SHA-256;
4. valida campos essenciais;
5. separa registros válidos e inválidos;
6. grava somente identificadores pseudonimizados e atributos acadêmicos necessários.

Saídas esperadas:

```text
Data_out/cadastro_validos_minimizado.csv
Data_out/cadastro_invalidos_minimizado.csv
```

## 🔗 Workflow

O projeto possui o workflow:

```text
Workflows/wf_pipeline_indicadores_academicos.hwf
```

O workflow organiza a execução das cargas e validações, com caminhos de falha e sucesso e geração de logs técnicos.

## 📥 Dados de entrada

Os dados utilizados na oficina são exemplos simulados e estão disponíveis em `Data_in`.

### `catalogo_cursos.csv`

Catálogo simplificado de cursos utilizado como referência para validação.

### `cadastro_academico.csv`

Cadastro acadêmico simulado utilizado nas etapas de processamento e qualidade. O cenário contém intencionalmente registros com campos ausentes, identificadores inválidos, duplicidades e situações acadêmicas fora do domínio esperado para demonstrar regras de qualidade.

> Não utilizar dados reais de estudantes neste repositório.

## 📤 Dados de saída

Os resultados dos processamentos são direcionados para:

```text
Data_out/
```

As saídas de privacidade não reproduzem CPF, matrícula ou nome originais. Esses identificadores são substituídos por pseudônimos determinísticos.

## 🔐 Segurança e privacidade

### Credenciais

Os pipelines versionados não possuem usuário, senha, token ou chave gravados diretamente. Os caminhos de arquivos utilizam `${PROJECT_HOME}`. Integrações futuras devem manter segredos fora dos arquivos `.hpl` e `.hwf`.

### Dados pessoais identificados

O cenário trabalha com **nome, CPF e matrícula** como identificadores pessoais. As medidas adotadas são:

- dados simulados para fins didáticos;
- minimização dos campos exportados;
- normalização antes da validação;
- pseudonimização de CPF, matrícula e nome nas saídas de privacidade;
- ausência de identificadores pessoais nos logs operacionais;
- documentação de riscos e controles.

Detalhamento: `docs/seguranca_e_privacidade.md`.

## 🔑 Variáveis e segredos

A variável utilizada pelo projeto é:

```text
${PROJECT_HOME}
```

Não há valores reais de segredos versionados. As variáveis `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD` e `API_TOKEN` são documentadas apenas como referências para futuras integrações.

Detalhamento: `docs/variaveis_e_segredos.md`.

## 👥 Papéis de acesso simulados

Para fins de governança da oficina, foram definidos os papéis de **Administrador, Desenvolvedor ETL, Analista de Dados, Auditor e Usuário**, com diferentes níveis de acesso e responsabilidade.

Detalhamento: `docs/controle_acesso.md`.

## ⚠️ Riscos e mitigações

Os principais riscos considerados são exposição de dados pessoais, credenciais em pipelines, dados duplicados, CPF não normalizado, registros inválidos enviados para carga, exposição de informações em logs, excesso de dados nas saídas e acesso indevido aos resultados.

Cada risco possui uma proposta de mitigação documentada em `docs/riscos.md`.

## 📝 Monitoramento e revisão dos logs

O workflow possui tratamento de erros e geração de logs automatizada.

- **Local dos logs:** `${PROJECT_HOME}/Logs/pipeline_<data>_<hora>.log`.
- **Falhas:** são encaminhadas para mensagens técnicas e `Abort`.
- **Sucesso:** possui caminho explícito de `Success`.
- **Privacidade:** logs não devem conter CPF, matrícula ou nome.
- **Evidência de revisão:** `Logs/revisao_log.md` contém o checklist de revisão e os critérios utilizados.

## 📊 Validações e duplicidades

O projeto demonstra atividades de qualidade de dados, incluindo:

- campos obrigatórios;
- integridade de matrícula;
- integridade e normalização de CPF;
- domínio de situação acadêmica;
- identificação de matrículas duplicadas;
- identificação de CPFs duplicados;
- validação de curso;
- separação entre registros válidos e inválidos.

## ⚙️ Configuração do projeto

O `project-config.json` utiliza `${PROJECT_HOME}` como base do projeto e define `Data_in` como diretório de datasets e `metadata` como diretório de metadados.

## 🚀 Como executar

1. Instale o Apache Hop.
2. Clone o repositório:

```bash
git clone https://github.com/wendelvox/pratica_apache_hop.git
```

3. Abra o projeto no Hop GUI.
4. Confira os arquivos de `Data_in`.
5. Execute o workflow `Workflows/wf_pipeline_indicadores_academicos.hwf`.
6. Para demonstrar especificamente a camada de privacidade, execute `PipeLines/pl_qualidade_privacidade.hpl`.
7. Consulte `Data_out` e `Logs`.

## 🧩 Conceitos praticados

- ETL;
- pipelines;
- workflows;
- normalização;
- validação de dados;
- qualidade de dados;
- duplicidades;
- classificação de registros;
- pseudonimização;
- minimização de dados;
- proteção de credenciais;
- tratamento de erros;
- logs e evidências;
- governança e papéis de acesso.

## 🛠️ Tecnologias

- **Apache Hop** — integração e orquestração de dados;
- **CSV** — fonte de dados simulados;
- **Git/GitHub** — versionamento e disponibilização do projeto.

## 📚 Documentação complementar

- `docs/seguranca_e_privacidade.md` — dados pessoais e medidas de proteção;
- `docs/variaveis_e_segredos.md` — inventário de variáveis e segredos;
- `docs/controle_acesso.md` — papéis de acesso simulados;
- `docs/riscos.md` — riscos e respectivas mitigações;
- `Logs/revisao_log.md` — evidência de revisão dos logs.

## 👨‍💻 Autor

**Wendel S. Santos**

Projeto desenvolvido para fins de estudo, capacitação e demonstração prática de integração, qualidade, segurança e governança de dados com Apache Hop.
