# Proposta — RPG-Manager

Documento de proposta da Sprint 0 da disciplina DIM0547.

---

## 1. Visão do produto

Para **jogadores de D&D 5e**
Que **precisam de um site para criar e gerenciar múltiplas fichas de personagem**
O **RPG-Manager** é uma **API de edição de fichas de D&D 5e**
Que **permite a criação de vários personagens diferentes, com cálculos automáticos das regras**
Diferente de **sites e aplicativos como Roll20, Foundry, Critical Role, 5eTools**
Nosso aplicativo **faz os cálculos automaticamente e tem opção de IA para sugerir personagens (classe + raça) e gerar um pequeno guia de como jogar com eles**.

A sugestão por IA aceita tanto conceitos amplos — *"quero um personagem conjurador para ser suporte"* — quanto pedidos específicos — *"quero um personagem com alto dano físico, da raça X, que usa a arma Y, e estou aberto a multiclasse"*.

---

## 2. Definição do MVP

O MVP é o escopo mínimo que entrega valor real ao usuário. Declaramos explicitamente o que fica **dentro** e o que fica **fora**.

| No MVP | Fora do MVP |
|---|---|
| CRUD de fichas de personagem (criação, edição, exclusão) | Rolagem de dados integrada e histórico de sessões |
| Cálculo automático de modificadores, perícias, CA, iniciativa e PV | Multiclase (multiclassing) e talentos opcionais |
| Cadastro de raça, classe, antecedente e nível (progressão básica) | Homebrew (criação de magias, itens e raças customizadas) |
| Catálogo de magias, itens e condições consultável (somente leitura) | Exportação em PDF, impressão e integração com VTTs (Roll20, Foundry) |
| Autenticação e perfil de jogador | Compartilhamento de fichas entre usuários ou mesas |
| **Sugestão de personagem por IA (classe + raça) a partir de um tema livre** | **Sugestão de build completa (atributos, perícias, magias otimizadas)** |
| **Guia curto gerado por IA de como jogar com o personagem sugerido** | **IA que cria fichas inteiras automaticamente ou conversa em tempo real** |
| Cache das consultas mais frequentes ao catálogo e das sugestões de IA | Recomendação personalizada com base em histórico do usuário |

### Hipótese de valor

> *Acreditamos que **jogadores de D&D 5e, especialmente os iniciantes**, vão **usar a feature de sugestão por IA para escolher classe e raça e receber um guia rápido de como jogar** porque **ela diminui a dificuldade de traduzir um conceito de personagem para as regras e elimina a paralisia de escolha diante de tantas opções**, dando um ponto de partida concreto. Isso permite começar a jogar mais rápido e com menos insegurança sobre as regras — e também atrai veteranos que querem testar algo novo.*

---

## 3. Backlog inicial

Backlog completo no GitHub Projects do repositório: **[PREENCHER link do GitHub Projects]**

Mínimo de 5 itens, ao menos 3 estimados, todos priorizados. Formato: **como [papel], quero [ação] para [benefício]**.

P1 é essencial ao MVP, P2 é importante, P3 é desejável.

| Prio | História | Critérios de aceitação | Sprint |
|---|---|---|---|
| P1 | Como jogador, quero criar e editar minha ficha de personagem para ter meus personagens organizados em um só lugar | CRUD de ficha; campos de raça, classe, antecedente e nível; validação dos campos obrigatórios | 1 |
| P1 | Como jogador, quero que o app calcule automaticamente modificadores, perícias, CA, iniciativa e PV para não errar ou recalcular na hora da sessão | Cálculos conforme D&D 5e; recálculo ao alterar atributos ou nível; valores exibidos na ficha | 1 |
| P1 | Como jogador, quero consultar um catálogo de magias, itens e condições para tirar dúvidas durante a criação da ficha | Catálogo somente leitura; busca por nome; paginado; dados vindos do SRD | 2 |
| P1 | Como jogador, quero receber sugestões de classe e raça a partir de um tema livre para superar a paralisia de escolha | Entrada de texto livre; retorno de classe + raça justificadas; sugestão validada contra as regras | 2 |
| P2 | Como jogador iniciante, quero um guia curto de como jogar com o personagem sugerido para começar com menos insegurança | Guia gerado por IA com papel, pontos fortes e dicas de uso; limite de tamanho; texto em português | 3 |
| P2 | Como jogador, quero me autenticar para que minhas fichas fiquem salvas e acessíveis só por mim | JWT com refresh; rotas de ficha protegidas; ficha vinculada ao usuário logado | 3 |

---

## 4. Entidades principais do domínio

| Entidade | Descrição | Atributos principais |
|---|---|---|
| **Usuário** | Dono das fichas; autentica-se no sistema | id, nome, e-mail, senha (hash), data de criação |
| **Ficha (Personagem)** | Personagem de D&D 5e criado pelo jogador | id, nome, raça, classe, antecedente, nível, atributos (FOR, DES, CON, INT, SAB, CAR), PV, CA, iniciativa, perícias, inventário, magias |
| **Raça** | Raça disponível no SRD | id, nome, bônus de atributo, traços, velocidade |
| **Classe** | Classe disponível no SRD | id, nome, dado de vida, proficiências, características por nível |
| **Antecedente** | Antecedente do personagem | id, nome, perícias concedidas, idiomas |
| **Magia** | Magia consultável no catálogo | id, nome, nível, escola, tempo de conjuração, alcance, duração, descrição |
| **Item** | Item consultável no catálogo | id, nome, tipo, peso, valor, descrição |
| **Sugestão de IA** | Resultado gerado pela IA a partir de um tema livre | id, usuário, tema informado, classe sugerida, raça sugerida, justificativa, guia, data |

---

## 5. Decisão: Java com Quarkus

### Justificativa

| Critério | Como Quarkus + Java atende |
|---|---|
| **Perfil da equipe** | Domínio prévio de Java e Spring; transição suave para Quarkus; interesse em aprender reatividade e build-time |
| **Características do domínio** | Alto desempenho, baixo consumo, reatividade nativa (Mutiny/Vert.x) e ampla integração via extensões |
| **Mercado e ecossistema** | Java consolidado no mercado; Quarkus em ascensão em cloud-native; ecossistema maduro + extensões oficiais |
| **Diferenciação didática** | Stack menos usual que Spring Boot, expõe paradigmas modernos sem sacrificar produtividade |

Optamos por **Java com Quarkus** por familiaridade da equipe com Java e com o ecossistema Spring. Quarkus compartilha vários conceitos com o Spring (injeção de dependência, RESTEasy/JAX-RS, anotações, build com Maven) e introduz ferramentas novas como **build-time processing**, **extensões nativas** e **programação reativa com Mutiny**. Isso reduz a curva de aprendizado sem abrir mão de ganhos técnicos e didáticos.

### Consequências

- Serviço principal concentra **regras de domínio, persistência, autenticação e orquestração**.
- Uso de **Panache** para persistência, **RESTEasy Reactive** para API, **OIDC/JWT** para autenticação.
- Build-time otimizado, com possibilidade futura de native image via GraalVM.

---

## 6. Divisão de responsabilidades entre o serviço principal e Go

| Vai para o serviço principal (Quarkus) | Vai para um microsserviço Go |
|---|---|
| Entidades de domínio e suas regras (ficha, atributos, perícias, magias, inventário) | Coleta e integração com sistemas externos (SRD, APIs de magias/itens/monstros) |
| Persistência e migrações (fichas, usuários, sugestões geradas) | Processamento em lote ou agendado (importação em massa do SRD, recálculo periódico) |
| Autenticação e autorização (dono da ficha) | Trabalho concorrente de I/O intensivo (sincronização de várias fontes em paralelo) |
| Orquestração dos casos de uso (criar, editar, validar, versionar ficha) | **Integração com o provedor de IA** (chamadas ao LLM, streaming, cache, retry e fallback) |
| Regras de negócio que validam sugestões da IA contra as regras de D&D 5e | Cache e pré-computação (tabelas de referência, magias, condições, resultados frequentes) |

### Racional

- O **serviço principal** concentra o que é **rico em regras de negócio e consistência transacional** — onde o ecossistema Java/Quarkus brilha (JPA/Panache, validação, segurança, injeção de dependência). As regras de D&D 5e são complexas e exigem testes robustos e tipagem forte. É aqui que a **sugestão gerada pela IA é validada** antes de chegar ao usuário.
- Os **microsserviços Go** ficam com o que é **concorrente, I/O-bound ou periódico** — cenários em que a leveza, o baixo consumo de memória e o modelo de concorrência (goroutines) do Go trazem ganho real. A **integração com o provedor de IA** se encaixa aqui: chamadas HTTP a um LLM são I/O-bound, podem ser paralelizadas, beneficiam-se de cache e exigem timeout/retry controlados.
- Essa separação **isola o risco da IA**: se o provedor cair ou mudar de contrato, o serviço principal continua funcionando (CRUD, cálculos, catálogo) e apenas a feature de sugestão degrada.
- **Evita o anti-padrão de "microsserviço por moda"**: cada serviço Go existe porque resolve um problema específico (integração, lote, IA), e não porque dividimos o domínio arbitrariamente.

### Estrutura do monorepo


RPG-Manager/
├── api/ # Serviço principal (Quarkus)
├── services/ # Microsserviços Go
│ └── ai-suggester/ # Integração com provedor de IA
├── protos/ # Contratos gRPC/protobuf (se aplicável)
├── docs/
│ └── proposta.md # Este documento
├── docker-compose.yml # Infra local (Postgres)
├── mise.toml # Ferramentas e tasks
└── README.md

## 7. Equipe

| Nome | Matrícula | Papel |
|Meu Nome|2469|TODOS|


## 8. Coorte e integração

- **Coorte de apresentação:** [ 2026.2, turma DIM0547]
- **Integração com outra disciplina:** ["Não há integração com outra disciplina no momento."]