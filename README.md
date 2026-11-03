# SaúdeTech Dashboards — Base do laboratório

Painel web do projeto de extensão **SaúdeTech** (Vigilância Sanitária de Londrina/PR),
em PHP + PostgreSQL, rodando em Docker.

Esta é a **versão base (v1.0.0)** usada no laboratório de **Gestão de Configuração de Software**:
o painel já funciona (login, vistorias, estabelecimentos, usuários), e cada aluno, em
**seu próprio fork e branch**, acrescenta ao Dashboard **um indicador** construído com SQL.

---

## 🚀 Como rodar

Pré-requisitos: Git e Docker Desktop (aberto, com *Engine running*).

```bash
# 1. Crie o arquivo de configuração a partir do modelo e preencha com os dados do banco
copy .env.example .env        # Windows (no Git Bash/Linux/Mac: cp .env.example .env)

# 2. Suba o painel
docker compose up --build

# 3. Acesse http://localhost:8080
```

Usuário de teste do painel: `admin@londrina.pr.gov.br` (senha informada pelo professor).

> O arquivo `.env` contém a senha do banco e **nunca** vai para o GitHub (está no `.gitignore`).

---

## 🧩 Onde o aluno trabalha

| Arquivo                                      | O que fazer                                                                       |
| -------------------------------------------- | --------------------------------------------------------------------------------- |
| `app/Repositories/DashboardRepository.php` | No método `meuIndicador()`, colar a consulta SQL do indicador escolhido        |
| `views/dashboard/index.php`                | No topo, em `$configMeuIndicador`, definir título, pergunta e tipo do gráfico |
| `README.md`                                | Preencher a seção **Meu indicador**, logo abaixo                          |

O gráfico e a tabela são montados automaticamente a partir do resultado da consulta:
**1ª coluna = rótulo**, **demais colunas = números**. É o mesmo formato do roteiro SQL.

---

## 📊 Meu indicador

- **Grupo:** 01
- **Integrantes:** Caroliny Ferreira de Morais (26002895-2), Felipe de Souza Maciel (26012477-2),
  Helen Arissa Tajima (26003547-2), João Victor Almeida Santana (26014448-2),
  Mel Gwinnywer (26012160-2), Yasmin Paduan Capeloto (26008107-2)
- **Indicador:** I08 — Interdições por bairros
- **Pergunta que responde:** Em quais bairros houve mais interdições?
- **Tipo de gráfico:** Barras (`bar`)
- **Consulta:** `app/Repositories/DashboardRepository.php`, método `meuIndicador()` —
  conta as interdições (`interdicoes`) ligadas às vistorias (`vistorias`) de cada
  estabelecimento (`estabelecimentos`), agrupadas por bairro, em ordem decrescente.
  Estabelecimentos sem bairro aparecem como "Não informado".

---

## 📝 Padrão de commits (Conventional Commits)

```
<tipo>(<escopo>): <descrição curta no imperativo>
```

| Tipo         | Quando usar                                | Exemplo                                                        |
| ------------ | ------------------------------------------ | -------------------------------------------------------------- |
| `feat`     | Nova funcionalidade                        | `feat(dashboard): adiciona consulta do indicador I03`        |
| `fix`      | Correção de erro                         | `fix(dashboard): corrige divisão por zero no percentual`    |
| `docs`     | Só documentação                         | `docs(readme): descreve o indicador I03`                     |
| `style`    | Formatação, sem mudar comportamento      | `style(dashboard): ajusta indentação do SQL`               |
| `refactor` | Reorganiza código sem mudar comportamento | `refactor(dashboard): extrai consulta para método próprio` |
| `chore`    | Tarefas de manutenção                    | `chore: atualiza .gitignore`                                 |

## 🏷️ Versionamento semântico (SemVer)

```
vMAIOR.MENOR.CORREÇÃO      ex.: v1.1.0
```

| Parte      | Muda quando…                      | Exemplo neste projeto                     |
| ---------- | ---------------------------------- | ----------------------------------------- |
| MAIOR      | Algo deixa de ser compatível      | Trocar o banco de dados → `v2.0.0`     |
| MENOR      | Nova funcionalidade compatível    | Acrescentar um indicador → `v1.1.0`    |
| CORREÇÃO | Correção sem funcionalidade nova | Corrigir o SQL do indicador → `v1.1.1` |

---

## 📦 Estrutura

```
app/
  Config/        conexão com o banco (lê o .env)
  Core/          base dos controllers, sessão/perfis, rótulos
  Controllers/   Dashboard · Vistoria · Estabelecimento · User
  Repositories/  consultas SQL de cada módulo
views/           telas (layout em views/partials, gráficos em views/dashboard)
public/          index.php (roteador) · diagnostico.php (testa a conexão com o banco)
Dockerfile       imagem PHP 8.2 + Apache + driver PostgreSQL
docker-compose.yml  sobe o painel em http://localhost:8080
```

Problemas de conexão com o banco: coloque `APP_DEBUG=1` no `.env` e abra
`http://localhost:8080/diagnostico.php`.

## 📄 Licença

MIT
