# Segurança e privacidade

## Objetivo

Documentar as medidas adotadas na oficina para evitar exposição desnecessária de dados pessoais e credenciais.

## Credenciais

Os pipelines do projeto não armazenam usuário, senha, token ou chave diretamente no XML dos pipelines. Os caminhos utilizam `${PROJECT_HOME}`. Caso uma conexão externa seja adicionada, credenciais deverão ficar fora dos pipelines, preferencialmente em variáveis de ambiente, configuração segura ou mecanismo de credenciais do ambiente de execução.

## Variáveis sensíveis

| Variável | Finalidade | Valor versionado |
|---|---|---|
| `PROJECT_HOME` | Diretório do projeto | Não é sensível |
| `DB_HOST` | Servidor de banco | Não |
| `DB_PORT` | Porta do banco | Não |
| `DB_NAME` | Nome do banco | Não |
| `DB_USER` | Usuário de banco | **Não armazenar** |
| `DB_PASSWORD` | Senha de banco | **Não armazenar** |
| `API_TOKEN` | Token de integração | **Não armazenar** |

As variáveis `DB_*` e `API_TOKEN` são apenas referências documentais para futuras integrações. Nenhum valor real é fornecido neste arquivo.

## Dados pessoais identificados

O cenário acadêmico trabalha conceitualmente com matrícula, CPF e nome. Esses campos são tratados como identificadores pessoais no contexto da oficina.

Medidas adotadas:

- utilização de dados simulados para fins didáticos;
- normalização do CPF antes das validações;
- geração de pseudônimos determinísticos para CPF, matrícula e nome no pipeline `pl_qualidade_privacidade.hpl`;
- não exportação dos valores pessoais originais nas saídas minimizadas;
- manutenção apenas dos atributos acadêmicos necessários à demonstração;
- recomendação de não registrar CPF, matrícula ou nome em logs operacionais.

## Fluxo de proteção

```text
Dados simulados
      ↓
Normalização
      ↓
Validação
      ↓
Pseudonimização
      ↓
Classificação
 ┌────┴─────┐
 ↓          ↓
Válidos   Inválidos
 ↓          ↓
Data_out   Data_out
```

## Observação

Pseudonimização não significa anonimização irreversível. O pipeline utiliza identificadores derivados por SHA-256 para manter consistência entre registros sem expor o valor original nas saídas.
