# Matriz de riscos e mitigacoes

| Risco | Impacto | Probabilidade | Mitigacao |
|---|---|---|---|
| CPF real armazenado no projeto | Alto | Media | Usar dados simulados e pseudonimizar saidas |
| Credenciais gravadas no pipeline | Alto | Baixa | Manter segredos fora dos XMLs e documentar variaveis sem valores |
| CPF, matricula ou nome expostos em logs | Alto | Media | Registrar somente contagens, status e mensagens tecnicas |
| Dados duplicados | Medio | Alta | Contagem por matricula/CPF e classificacao dos registros |
| CPF nao normalizado | Medio | Alta | Normalizar removendo mascara antes da validacao |
| Registros invalidos enviados a carga | Alto | Media | Separar fluxo valido/invalido antes da saida |
| Acesso indevido aos resultados | Alto | Media | Aplicar papeis simulados e principio do menor privilegio |
| Excesso de dados na saida | Alto | Media | Minimizacao e pseudonimizacao dos identificadores |
| Falha de execucao sem rastreabilidade | Medio | Media | Workflow com Abort/Success e revisao dos logs |
| Pseudonimizacao confundida com anonimização | Medio | Media | Documentar que SHA-256 gera pseudonimos e nao elimina todo risco de reidentificacao |
