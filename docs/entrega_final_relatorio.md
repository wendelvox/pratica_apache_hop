# Relatório Final de Entrega - Oficina Prática Apache Hop

**Autor:** Wendel S. Santos  
**Repositório:** [github.com/wendelvox/pratica_apache_hop](https://github.com/wendelvox/pratica_apache_hop)  
**Data da Entrega:** Setembro de 2026  

Este documento consolida a entrega final da atividade, atendendo a todos os requisitos de organização, funcionalidade, governança, segurança e documentação do projeto.

---

## 1. Projeto Apache Hop com Pipelines e Workflows Organizados
O projeto segue uma arquitetura modular e separada por responsabilidades, utilizando a estrutura de diretórios padrão do Apache Hop:
- **`PipeLines/`**: Contém as lógicas de ETL/ELT individuais (ex: `pl_validar_cadastro.hpl`, `pl_monitoramento_mais_futuro.hpl`, `pl_qualidade_privacidade.hpl`).
- **`Workflows/`**: Contém a orquestração (`wf_pipeline_indicadores_academicos.hwf`), gerenciando a ordem de execução e o tratamento de falhas.
- **`Data_in/` e `Data_out/`**: Separação clara entre fontes de dados e artefatos gerados.
- **`docs/`**: Centralização de toda a governança, segurança e evidências.

## 2. Workflow Principal Funcional e Scripts de Dados
O workflow principal **`wf_pipeline_indicadores_academicos.hwf`** está totalmente funcional e orquestra a seguinte sequência:
1. `Validar cadastro` → 2. `Validar historico` → 3. `Validar Trancamento` → 4. `Validar bolsista` → 5. `Monitoramento Mais Futuro`.

**Sobre os Scripts SQL:**  
Nesta oficina prática, a fonte de dados utilizada são arquivos **CSV** (`Data_in/`) para fins didáticos e de portabilidade, eliminando a necessidade de um banco de dados ativo para a avaliação. No entanto, a arquitetura está preparada para SQL: os caminhos de dados utilizam a variável `${PROJECT_HOME}`, e a documentação (`docs/variaveis_e_segredos.md`) já prevê variáveis como `${DB_HOST}`, `${DB_USER}` e `${DB_PASSWORD}`. A migração de um `CSV Input` para um `Table Input` (JDBC) pode ser feita sem alterar a lógica de transformação dos pipelines.

## 3. Evidência de Execução Final e Validação dos Dados Carregados
A execução do workflow gera artefatos validados na pasta `Data_out/`, comprovando a aplicação das regras de negócio:
- **Validação de Integridade:** Separação automática de registros válidos e inválidos (campos nulos, CPFs inválidos, duplicidades de matrícula).
- **Regra de Negócio Principal:** O pipeline `pl_monitoramento_mais_futuro.hpl` cruza as ocorrências acadêmicas com a data de início da bolsa. Apenas reprovações/trancamentos **posteriores** ao início do benefício são contabilizados.
- **Saída de Decisão:** Geração do arquivo `estudantes_bolsa_a_cancelar.csv` contendo apenas os estudantes com mais de 4 ocorrências elegíveis, com dados minimizados para proteção.

## 4. Registros de Logs e Métricas
O projeto possui captura de desempenho habilitada (`capture_transform_performance=Y`) e geração de logs técnicos.
- **Métrica de Execução:** O pipeline principal de monitoramento processou os dados de teste em **0.54 segundos**, com **0 erros (E=0)**.
- **Conformidade de Logs:** Conforme atestado em `Logs/revisao_log.md`, os logs registram apenas metadados técnicos (nomes de transforms, contagens de linhas R/W). **Nenhum dado pessoal (CPF, Nome, Matrícula)** é exposto nos logs, garantindo conformidade com a LGPD.

## 5. Documentação Técnica e README Atualizado
O repositório possui documentação completa e atualizada:
- `README.md`: Visão geral, estrutura, fluxo técnico e instruções de execução.
- `docs/seguranca_e_privacidade.md`: Diretrizes de pseudonimização (SHA-256) e minimização de dados.
- `docs/riscos.md`: Matriz de riscos e mitigações.
- `docs/controle_acesso.md`: Definição de papéis simulados (Administrador, Desenvolvedor ETL, Auditor).
- `docs/RELATORIO_FINAL_ENTREGA.md`: Este documento.

## 6. Checklist Final de Conformidade Preenchido

| Requisito da Entrega | Status | Evidência no Projeto |
| :--- | :---: | :--- |
| Projeto com pipelines e workflows organizados | ✅ | Pastas `PipeLines/` e `Workflows/` estruturadas. |
| Workflow principal funcional | ✅ | `wf_pipeline_indicadores_academicos.hwf` executa com sucesso. |
| Evidência de execução e validação | ✅ | Arquivos gerados em `Data_out/` com regras de negócio aplicadas. |
| Registros de logs e métricas | ✅ | Logs em `Logs/` e `capture_transform_performance=Y` ativo. |
| Documentação técnica / README atualizado | ✅ | `README.md` e pasta `docs/` completas. |
| Checklist final de conformidade | ✅ | Esta seção do relatório. |
| Credenciais protegidas por variáveis | ✅ | Uso de `${PROJECT_HOME}` e `.env.example` sem segredos reais. |
| Limitações e próximos passos descritos | ✅ | Seção 8 deste relatório. |

## 7. Registro de Credenciais Protegidas por Variáveis
O projeto adere estritamente às boas práticas de segurança:
- Nenhum arquivo `.hpl`, `.hwf` ou `.json` contém senhas, tokens ou usuários hardcoded.
- O arquivo `.env.example` serve como modelo seguro para variáveis de ambiente.
- O arquivo `docs/variaveis_e_segredos.md` documenta exatamente quais variáveis devem ser injetadas no ambiente de execução (ex: `${DB_PASSWORD}`), garantindo que segredos permaneçam fora do controle de versão (`.gitignore` configurado).

## 8. Descrição de Limitações e Indicação de Próximos Passos

**Limitações Identificadas:**
1. **Processamento em Memória:** O uso de `MemoryGroupBy` é eficiente para volumes pequenos/médios, mas pode causar estouro de memória (OOM) em datasets de milhões de linhas.
2. **Parsing de Datas em JavaScript:** O transform `ScriptValueMod` utilizado para comparar datas executa código JS linha a linha, o que possui *overhead* de desempenho maior que componentes nativos Java.
3. **Fonte de Dados Estática:** O uso de CSVs limita a atualização em tempo real dos dados acadêmicos.

**Próximos Passos (Roadmap de Evolução):**
- **Curto Prazo:** Substituir o `ScriptValueMod` (JS) por `User Defined Java Expression` (UDJE) para otimizar a comparação de datas, reduzindo o tempo de CPU em até 60%.
- **Médio Prazo:** Migrar as fontes e destinos de `CSV Input/Output` para `Table Input/Output` (JDBC), conectando o workflow a um banco de dados relacional (PostgreSQL/MySQL) utilizando as variáveis de ambiente já documentadas.
- **Longo Prazo:** Implementar um pipeline de CI/CD (ex: GitHub Actions) para executar testes automatizados de validação dos pipelines a cada novo commit.

---
*Relatório gerado para fins de avaliação da Oficina Prática de Apache Hop.*