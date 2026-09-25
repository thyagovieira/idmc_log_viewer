# IDMC Log Viewer

O **IDMC Log Viewer** é um visualizador web desenvolvido para facilitar a leitura, navegação e análise de logs gerados pelo **Informatica Intelligent Data Management Cloud (IDMC)**.

Logs de execução do IDMC podem conter centenas ou milhares de linhas com informações de sessão, SQL gerado, warnings, erros, estatísticas e mensagens de diferentes componentes. O objetivo deste projeto é transformar esse conteúdo em uma interface mais organizada e amigável, permitindo identificar rapidamente as informações mais importantes de uma execução.

## 🚀 Funcionalidades

O visualizador processa arquivos de log diretamente no navegador e organiza automaticamente as principais informações encontradas.

Entre os recursos disponíveis estão:

- 📂 Carregamento de arquivos `.log`
- 🖱️ Suporte a drag-and-drop
- 🔎 Pesquisa dentro do log
- 🚨 Identificação e destaque de erros
- ⚠️ Identificação de warnings
- 🧩 Identificação de códigos de mensagens do Informatica
- 🗄️ Identificação de informações relacionadas a SQL e banco de dados
- 📊 Resumo da execução
- 📈 Quantidade de registros processados
- ❌ Quantidade de registros rejeitados
- ⏱️ Informações sobre a execução da sessão
- 🧵 Identificação das diferentes threads/componentes do log
- 📋 Facilidades para copiar informações relevantes
- 🌙 Interface voltada para leitura de logs técnicos

O Viewer reconhece diversos padrões comuns encontrados nos logs do Informatica, como:

```text
CMN_*
FR_*
TM_*
TE_*
PETL_*
OPT_*
VAR_*
```

Isso facilita a localização de mensagens como:

```text
CMN_1022
CMN_1761
FR_3016
VAR_27062
```

e outros códigos gerados durante uma execução.

## 🗄️ Visualização de SQL

Execuções que utilizam **SQL ELT / Pushdown Optimization** podem gerar comandos SQL extensos dentro do log.

O IDMC Log Viewer procura separar essas informações do restante do conteúdo para facilitar a identificação de:

- SQL gerado pelo Informatica
- `INSERT INTO`
- `SELECT`
- tabelas de origem
- tabelas de destino
- comandos executados pelo banco
- mensagens relacionadas à execução SQL

Isso é especialmente útil para mappings que utilizam **Pushdown Optimization** e bancos como **Teradata**.

## 📊 Resumo da execução

Sempre que essas informações estiverem disponíveis no arquivo, o Viewer procura apresentar de forma resumida dados como:

```text
Status da execução
Task
Run ID
Agent
Target
Linhas processadas
Linhas afetadas
Linhas rejeitadas
Erros
Warnings
Tempo de execução
```

Dessa forma, muitas vezes não é necessário percorrer manualmente centenas de linhas para descobrir o resultado da execução.

## 🚨 Análise de erros

O sistema identifica mensagens que podem representar falhas durante a execução e permite localizar rapidamente o ponto do log onde o problema ocorreu.

Isso ajuda na investigação de situações como:

```text
Database driver error
DTM terminated
Record length exceeded
SQL execution error
Transformation error
Connection error
Parameter error
Code page warning
```

A ideia é reduzir o tempo gasto procurando manualmente a causa de uma falha dentro de logs extensos.

## 🔒 Privacidade

Atualmente o processamento do arquivo é realizado **localmente no navegador**.

O arquivo de log não precisa ser enviado para um servidor externo para que o Viewer faça a leitura e classificação das informações.

Isso é especialmente importante porque logs podem conter informações como:

- nomes de servidores
- schemas
- tabelas
- usuários
- caminhos internos
- SQLs
- parâmetros
- nomes de mappings e workflows

## 🤖 Análise com Inteligência Artificial

O projeto já possui uma área reservada para uma futura funcionalidade de **Análise com IA**.

O botão permanece propositalmente desabilitado nesta versão.

A ideia é permitir que um modelo de inteligência artificial utilize as informações extraídas pelo próprio Viewer para analisar automaticamente uma falha.

Exemplo do fluxo planejado:

```text
Arquivo LOG
    │
    ▼
IDMC Log Viewer
    │
    ├── Erros
    ├── Warnings
    ├── Mapping
    ├── Target
    ├── SQL
    └── contexto da falha
    │
    ▼
Modelo de IA
    │
    ▼
Diagnóstico
    │
    ├── Causa provável
    ├── Evidências encontradas
    ├── Possíveis causas
    ├── Soluções recomendadas
    └── SQL para diagnóstico
```

A integração com IA está no **roadmap do projeto** e ainda não está disponível na versão atual.

## 💻 Como utilizar

O IDMC Log Viewer foi desenvolvido para ser simples de executar.

Não é necessário instalar servidor web, banco de dados ou outras dependências.

### 1. Baixe o projeto

Clone o repositório:

```bash
git clone <URL-DO-REPOSITORIO>
```

ou faça o download do projeto pelo GitHub.

### 2. Abra o Viewer

Abra o arquivo:

```text
index.html
```

em um navegador moderno, como:

- Google Chrome
- Microsoft Edge
- Firefox

### 3. Carregue o log

Selecione ou arraste um arquivo `.log` exportado pelo IDMC para a área indicada na página.

O arquivo será processado e as informações encontradas serão apresentadas na interface.

## 🏗️ Arquitetura

A versão atual foi propositalmente construída de forma simples.

```text
IDMC Log Viewer
│
├── HTML
├── CSS
└── JavaScript
        │
        ▼
   Parser de logs
        │
        ├── Metadados
        ├── Erros
        ├── Warnings
        ├── SQL
        ├── Estatísticas
        └── Resumo da execução
```

Todo o processamento principal acontece no navegador.

## 🛣️ Roadmap

Algumas funcionalidades planejadas para as próximas versões incluem:

- [ ] Análise de logs utilizando IA
- [ ] Identificação automática da causa raiz
- [ ] Diferenciação entre erro principal e erros derivados
- [ ] Explicações para códigos de erro do Informatica
- [ ] Sugestões automáticas de solução
- [ ] Geração de SQL para diagnóstico
- [ ] Melhor formatação do SQL gerado pelo Pushdown
- [ ] Agrupamento de erros semelhantes
- [ ] Linha do tempo da execução
- [ ] Comparação entre dois logs
- [ ] Exportação do diagnóstico
- [ ] Base de conhecimento de erros conhecidos
- [ ] Histórico de análises
- [ ] Suporte a novos formatos de log

## 🎯 Objetivo do projeto

O objetivo do **IDMC Log Viewer** não é substituir as ferramentas de monitoramento do Informatica.

A proposta é fornecer uma ferramenta complementar voltada principalmente para **análise técnica e troubleshooting**.

Em vez de analisar manualmente um arquivo como:

```text
10.000+ linhas de log
```

a ideia é chegar rapidamente a algo como:

```text
EXECUÇÃO
──────────────
Status: FAILED

ERRO PRINCIPAL
──────────────
CMN_1022
Database driver error

COMPONENTE
──────────────
SQL_1_1_1

TARGET
──────────────
SCHEMA.TABELA

CAUSA PROVÁVEL
──────────────
Erro retornado pelo banco durante
a execução do INSERT.

CONTEXTO
──────────────
[trecho relevante do log]

SQL
──────────────
[SQL relacionado ao erro]
```

Com a futura integração de IA, o objetivo é complementar esse diagnóstico com possíveis causas e procedimentos para resolução.

## ⚠️ Status do projeto

O projeto está em desenvolvimento.

Novos padrões de logs, códigos de erro e funcionalidades serão adicionados conforme forem sendo identificados em execuções reais do Informatica IDMC.

Contribuições, sugestões e exemplos de logs que ajudem a melhorar o parser são bem-vindos.

---

**IDMC Log Viewer**  
Uma interface simples para tornar logs do Informatica IDMC mais fáceis de ler, pesquisar e diagnosticar.
