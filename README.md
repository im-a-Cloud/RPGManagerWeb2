# RPG-Manager

Aplicativo de gerenciamento e edição de fichas de personagem de **D&D 5e**, voltado para **jogadores**.

O projeto calcula automaticamente modificadores, perícias, CA, iniciativa e PV, oferece um catálogo consultável de magias, itens e condições, e usa **IA para sugerir personagens (classe + raça)** e gerar um **guia curto de como jogar** com eles.

---

## Stack

| Camada | Tecnologia |
|---|---|
| Serviço principal | Java 21 + Quarkus 3.x |
| Microsserviço | Go 1.21 |
| Banco de dados | PostgreSQL 16 |
| Build e tasks | mise |
| Infra local | Docker Compose |
| CI | GitHub Actions |

---

## Estrutura do monorepo

```
RPG-Manager/
├── api/                  # Serviço principal (Quarkus)
├── services/             # Microsserviços Go
│   └── ai-suggester/     # Integração com provedor de IA
├── protos/               # Contratos gRPC/protobuf (se aplicável)
├── docs/
│   └── proposta.md       # Documento de proposta da Sprint 0
├── docker-compose.yml    # Infra local (Postgres)
├── mise.toml             # Ferramentas e tasks
└── README.md
```


---

## Pré-requisitos

- [mise](https://mise.jdx.dev/) instalado e ativado no seu shell
- [Docker](https://www.docker.com/) e Docker Compose (para o banco local)
- Git

O `mise` cuida de instalar Java, Maven e Go nas versões corretas automaticamente.

---

## Como rodar

### 1. Instalar as ferramentas do projeto

```bash
mise install
```
```bash
docker compose up -d
```

```bash
mise run build
```

```bash
mise run test
```

```bash
cd api
./mvnw quarkus:dev
```

```bash
cd services/ai-suggester
go run .

```



## Licença
Projeto acadêmico desenvolvido para a disciplina DIM0547. Uso educacional.

