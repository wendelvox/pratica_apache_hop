# Variaveis e segredos

## Variaveis atualmente utilizadas

| Variavel | Uso | Sensivel |
|---|---|---|
| PROJECT_HOME | Diretorio raiz do projeto | Nao |

## Variaveis reservadas para futuras integracoes

| Variavel | Uso | Regra |
|---|---|---|
| DB_HOST | Host do banco | Nao versionar valor real |
| DB_PORT | Porta do banco | Nao versionar valor real |
| DB_NAME | Banco/schema logico | Nao versionar valor real |
| DB_USER | Usuario do banco | Nunca gravar credencial no pipeline |
| DB_PASSWORD | Senha do banco | Nunca versionar |
| API_TOKEN | Token de API | Nunca versionar |

Nenhum valor real de segredo deve ser colocado em pipelines, workflows, project-config.json, README ou arquivos de dados versionados.
