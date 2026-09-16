# Relatório — RPG-Manager

## 1. Visão do Produto

O **RPG-Manager** é um aplicativo de gerenciamento de fichas de personagem para **Dungeons & Dragons 5ª Edição (D&D 5e)**, voltado para **jogadores**. O sistema facilita a criação, edição e consulta de personagens, automatizando cálculos que normalmente são feitos manualmente modificadores de atributos, bônus de perícia/proficiência, Classe de Armadura (CA), iniciativa e Pontos de Vida (PV).

Como diferencial de outras plataformas como Roll20 e Foundry, o RPG-Manager usa **Inteligência Artificial** para sugerir combinações de **classe e raça** e gerar um **pequeno guia** de como jogar com o personagem sugerido. A IA atua como assistente à criação: ajuda iniciantes a lidar com a grande quantidade de opções e permite que veteranos saiam da zona de conforto ou superem bloqueios criativos, ou em casos comuns á ambos onde o jogador não sabe como transformar sua ideia em um personagem que encaixe nas regras — **sem substituir a decisão do jogador**, apenas filtrando alternativas, útil para aqueles que não tem amigos/mestres veteranos que possam guia-lo.

### 1.1 Público-alvo

Jogadores de D&D 5e, especialmente **iniciantes** que precisam de apoio para escolher e entender o personagem, e **veteranos** que querem experimentar combinações novas.

### 1.2 Problema

A ficha de D&D 5e envolve muitos valores derivados que precisam permanecer consistentes entre si (atributos, modificadores, perícias, CA, iniciativa, PV, magias, itens). Além disso, iniciantes têm dificuldade para escolher uma combinação de raça e classe e para entender como usar as características do personagem em jogo.

### 1.3 Hipótese de valor

> *Acreditamos que **jogadores de D&D 5e** vão **usar o RPG-Manager para criar, editar e consultar suas fichas** porque **a automação dos cálculos, o catálogo de informações e o auxílio por IA tornam o processo de preparação mais simples**, permitindo começar a jogar mais rápido e com menos insegurança sobre as regras.*

---

## 2. MVP

### 2.1 Dentro do escopo

- Criação e edição de ficha de personagem
- Cadastro das informações básicas (nome, raça, classe, nível, atributos)
- Cálculo automático de modificadores, perícias, CA, iniciativa e PV
- Consulta/Adição de magias, itens e condições
- **Sugestão de personagem por IA** (classe + raça) a partir de um tema livre
- **Guia curto gerado por IA** de como jogar com o personagem sugerido

### 2.2 Fora do escopo

Gerenciamento de campanhas, sistema de combate(um simulador com monstros de DnD), mapas, chat entre jogadores/narradores, sistema de mestre, multiplayer em tempo real, criação de regras próprias (homebrew), integração com VTTs(plataformas digitais que permitem jogar RPG de mesa à distância), automação da sessão(rolamento de dados e retornar se foi um sucesso ou não).

### 2.3 Fluxo principal

1. Jogador cria a ficha e define raça, classe, nível e atributos.
2. O sistema calcula os valores derivados (modificadores, perícias, CA, iniciativa, PV).
3. O jogador insere magias, itens e habilidades.
4. Opcionalmente, o jogador solicita uma sugestão por IA, que retorna classe + raça e um guia curto.

---

## 3. Backlog

Backlog completo no GitHub Projects: **https://github.com/users/im-a-Cloud/projects/1**

| ID | História | Prioridade | Estimativa |
|---|---|---|---|
| US01 | Como jogador, quero criar uma ficha de personagem para registrar meu personagem de D&D 5e | P1 | 5 |
| US02 | Como jogador, quero definir raça, classe, nível e atributos para configurar meu personagem | P1 | 5 |
| US03 | Como jogador, quero que o sistema calcule automaticamente os modificadores e valores derivados da ficha para evitar cálculos manuais | P1 | 5 |
| US04 | Como jogador, quero consultar magias para encontrar rapidamente informações necessárias durante uma sessão | P1 | 5 |
| US05 | Como jogador, quero consultar itens e condições para acessar informações relevantes durante o jogo | P2 | 3 |
| US06 | Como jogador, quero editar minha ficha para manter as informações do personagem atualizadas | P1 | 3 |
| US07 | Como jogador, quero receber sugestões de raça e classe geradas por IA para facilitar a criação de um personagem | P1 | 5 |
| US08 | Como jogador, quero receber um guia curto de como jogar com o personagem sugerido para entender melhor suas características | P2 | 3 |

**Prioridades:** P1 — essencial ao MVP; P2 — importante; P3 — desejável.

---

## 4. Entidades do Domínio

| Entidade | Descrição | Atributos principais |
|---|---|---|
| **Personagem** | Entidade central; representa a ficha do jogador | nome, raça, classe, nível, atributos, modificadores, perícias, CA, iniciativa, PV, magias, itens |
| **Raça** | Raça disponível no SRD | nome, bônus de atributo, traços, velocidade |
| **Classe** | Classe escolhida pelo personagem | nome, dado de vida, proficiências, características por nível |
| **Magia** | Magia consultável no catálogo | nome, nível, escola, tempo de conjuração, alcance, duração, descrição |
| **Item** | Item ou equipamento consultável | nome, tipo, peso, valor, descrição |
| **Condição** | Condição de status (ex.: envenenado, caído) | nome, descrição, efeitos |
| **Sugestão de IA** | Resultado gerado pela IA | tema informado, classe sugerida, raça sugerida, justificativa, guia, data |

**Relações principais:** Personagem possui Raça, Classe, Magias e Itens; possui valores calculados (modificadores, perícias, CA, iniciativa, PV). A IA sugere Classe + Raça e gera um Guia curto.

---

## 5. Decisão da Stack

O serviço principal será desenvolvido em **Java com Quarkus**.

### 5.1 Justificativa

| Critério | Como Quarkus + Java atende |
|---|---|
| **Perfil da equipe** | Domínio prévio de Java e Spring; transição suave para Quarkus; interesse em aprender reatividade e build-time |
| **Características do domínio** | Alto desempenho, baixo consumo, reatividade nativa (Mutiny/Vert.x) e ampla integração via extensões |
| **Mercado e ecossistema** | Java consolidado no mercado; Quarkus em ascensão em cloud-native; ecossistema maduro + extensões oficiais |
| **Diferenciação didática** | Stack menos usual que Spring Boot, expõe paradigmas modernos sem sacrificar produtividade |

Quarkus compartilha vários conceitos com o Spring (injeção de dependência, RESTEasy/JAX-RS, anotações, build com Maven) e introduz ferramentas novas como build-time processing, extensões nativas e programação reativa com Mutiny. A stack atende ao domínio por oferecer **tipagem forte** para as regras de D&D 5e, **ecossistema maduro** para persistência e segurança, e **boa integração** com o serviço Go e com o componente de IA.

---

## 6. Divisão com o Serviço Go

### 6.1 Serviço principal (Quarkus)

Responsável pelo núcleo do sistema e pelo domínio: gerenciamento das fichas, regras de negócio, raça e classe, atributos, cálculo dos valores derivados, magias, itens, condições, persistência, autenticação e exposição da API principal.

### 6.2 Serviço Go

Responsável por tarefas com características técnicas diferentes: coleta e integração com fontes externas (SRD, APIs de magias/itens), processamento em lote ou agendado, operações de I/O concorrente, cache e pré-computação, e **integração com o provedor de IA** (chamadas ao LLM, streaming, retry e fallback).

### 6.3 Integração com IA

A IA recebe o tema ou preferências do jogador, sugere uma combinação de **classe + raça** e gera um **guia curto** de como jogar. A IA atua como assistente à criação — **não é responsável pelas regras fundamentais do domínio**. As regras e cálculos da ficha permanecem sob responsabilidade da aplicação, garantindo que os valores sejam determinados pela lógica do RPG-Manager.

### 6.4 Justificativa da divisão

O serviço principal concentra o domínio e as operações que exigem consistência das informações da ficha. O serviço Go fica com tarefas de processamento, I/O, concorrência, cache e integração — incluindo as chamadas ao provedor de IA, que são I/O-bound e se beneficiam do modelo de concorrência leve do Go (goroutines). Essa separação **isola o risco da IA**: se o provedor cair, o serviço principal continua funcionando e apenas a feature de sugestão degrada.

---

## 7. Equipe

| Integrante | Matrícula | Responsabilidades |
|---|---|---|
| Álvaro Prudêncio Araújo | 20240078220 | Todas |

As responsabilidades serão distribuídas conforme as necessidades de cada sprint, mantendo a colaboração da equipe em desenvolvimento, testes, integração, documentação e manutenção.

---

## 8. Coorte e Integração

### 8.1 Coorte de apresentação

[2026.2, turma DIM0547]

### 8.2 Estrutura do monorepo

RPGManagerWeb2/
├── api/ # Serviço principal (Quarkus)
├── services/ # Serviço Go (ai-suggester)
├── protos/ # Contratos gRPC/protobuf (se aplicável)
├── docs/ # Documentação do projeto
├── .github/
│ └── workflows/ # CI (GitHub Actions)
├── docker-compose.yml # Infra local (Postgres)
├── mise.toml # Ferramentas e tasks
└── README.md


### 8.3 Comunicação entre os componentes

A comunicação entre o serviço principal e o serviço Go será via **REST** (HTTP/JSON). A integração mantém separação clara entre as responsabilidades, evitando que o serviço Go assuma as regras centrais do domínio.

### 8.4 Integração contínua

O projeto usa **GitHub Actions** para verificar automaticamente os dois stacks. O pipeline roda em eventos de **push** e **pull request** na branch `main`, executando o build do serviço principal (Java/Quarkus) e do serviço Go. O objetivo é garantir que alterações não quebrem a compilação dos componentes. O CI está verde na branch `main`.

---

## Conclusão

O RPG-Manager é uma aplicação focada no gerenciamento de fichas de D&D 5e, centralizando informações e automatizando cálculos. O MVP prioriza criação e edição de fichas, cálculo automático de valores derivados e consulta a catálogos, tendo como diferencial a **sugestão de personagens por IA** e a geração de um **guia curto**. A arquitetura separa o serviço principal (domínio, Quarkus) do serviço Go (integração, I/O, IA), permitindo que cada componente seja desenvolvido conforme o tipo de trabalho que executa. O backlog organiza o desenvolvimento em histórias priorizadas, e o CI no GitHub Actions garante que os dois stacks permaneçam compilando.
