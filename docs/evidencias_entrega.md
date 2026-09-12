# Evidencias da atividade

| Requisito | Evidencia no projeto |
|---|---|
| Credenciais nao gravadas diretamente nos pipelines | Revisao dos arquivos `.hpl`/`.hwf` e `docs/variaveis_e_segredos.md` |
| Variaveis sensiveis sem valores reais | `docs/variaveis_e_segredos.md` e `.env.example` |
| Dados pessoais identificados | `docs/seguranca_e_privacidade.md` |
| Minimizacao/pseudonimizacao | `PipeLines/pl_qualidade_privacidade.hpl` |
| Revisao dos logs | `Logs/revisao_log.md` |
| Papeis de acesso simulados | `docs/controle_acesso.md` |
| Riscos e mitigacoes | `docs/riscos.md` |
| Atualizacao do README | `README.md` |

## Procedimento de demonstracao

1. Abrir os pipelines e verificar que nao existem credenciais embutidas.
2. Executar `pl_qualidade_privacidade.hpl`.
3. Conferir `Data_out/cadastro_validos_minimizado.csv` e `Data_out/cadastro_invalidos_minimizado.csv`.
4. Confirmar que as saidas usam pseudonimos e nao reproduzem CPF, matricula e nome originais.
5. Executar o workflow e revisar o log tecnico.
6. Conferir `Logs/revisao_log.md`.
7. Apresentar a matriz de riscos e os papeis simulados.
