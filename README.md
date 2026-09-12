Oficina Prática Apache Hop - ESCOLA PARA CAPACITAÇÃO DE TECNOLOGIAS DA INFORMAÇÃO

## 📊 Monitoramento e Logs
Este workflow possui tratamento de erros e geração de logs automatizada.
- **Local dos Logs:** Os arquivos são gerados em `${PROJECT_HOME}/Logs/pipeline_<data>_<hora>.log`.
- **Tratamento de Erros:** Em caso de falha em qualquer pipeline (`carga_cursos`, `carga_matriculas` ou `validar_cadastro`), o workflow registra o erro, exibe uma mensagem de abort e interrompe a execução para evitar inconsistências.
- **Como verificar:** Em caso de erro, consulte a mensagem de Abort na interface do Hop ou abra o arquivo de log correspondente na pasta de Logs.
