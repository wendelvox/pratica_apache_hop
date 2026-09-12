# Papéis de acesso simulados

A oficina não implementa autenticação. Os papéis abaixo são uma simulação de governança para demonstrar segregação de responsabilidades.

| Papel | Permissões simuladas |
|---|---|
| Administrador | Configurar projeto, executar workflows, revisar configurações e resultados |
| Desenvolvedor ETL | Criar e alterar pipelines/workflows e executar testes |
| Analista de Dados | Executar processos e consultar resultados minimizados |
| Auditor | Consultar logs, evidências e matriz de riscos; não altera pipelines |
| Usuário | Consultar somente resultados disponibilizados |

## Princípios

- menor privilégio;
- segregação entre desenvolvimento e auditoria;
- acesso a dados pessoais somente quando estritamente necessário;
- resultados preferencialmente pseudonimizados;
- logs disponíveis para auditoria sem expor identificadores pessoais.
