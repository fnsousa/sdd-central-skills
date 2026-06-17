# SDD Central Skills

Um repositório centralizado de skills especializadas para o **Speckit**, desenvolvidas com foco em **Spec-Driven Development (SDD)**.

Este projeto concentra conhecimento prático sobre arquitetura, linguagens, padrões de teste e melhores práticas para guiar o desenvolvimento orientado por especificações.

## Visão Geral

O SDD Central Skills fornece um conjunto de skills que podem ser integradas em seus projetos para:

- **Orientar decisões arquiteturais** usando padrões comprovados
- **Padronizar implementação** em linguagens e frameworks específicos
- **Garantir qualidade** através de práticas de teste bem definidas
- **Acelerar desenvolvimento** com diretrizes claras e contextualizadas

Cada skill é uma coleção de boas práticas, padrões de código, e workflows que podem ser consultados durante o desenvolvimento, especialmente quando implementando casos de uso, portas, adaptadores, testes, ou tomando decisões de design.

## Getting Started

### 1. Adicionar o Bootstrap ao Seu Projeto

O primeiro passo é copiar o arquivo `bootstrap.sh` para a **raiz do seu projeto consumidor**:

```bash
# No repositório sdd-central-skills
curl -fsSL \
  https://raw.githubusercontent.com/fnsousa/sdd-central-skills/refs/heads/main/bootstrap.sh \
  -o bootstrap.sh
```

O script `bootstrap.sh` é responsável por:

1. **Clonar** este repositório (com histórico mínimo)
2. **Ler** a lista de skills desejadas
3. **Copiar** os arquivos de skill para o diretório `.specify/skills/` do seu projeto
4. **Limpar** skills que não estão mais na lista de dependências

### 2. Criar o Arquivo `.specify/skills.yaml`

No diretório `.specify/` do seu projeto, crie um arquivo `skills.yaml` listando as skills que deseja utilizar:

```yaml
# .specify/skills.yaml
# Formato: nome@versão

skills:
  - architecture/hexagonal@1.0.0
  - java-21@1.0.0
  - testing/junit5@1.0.0
  - testing/mockito@1.0.0
```

Cada skill é referenciada no formato `<nome>@<versão>`, onde:

- **nome**: O caminho e nome da skill no repositório (ex: `architecture/hexagonal`, `java-21`)
- **versão**: A versão desejada da skill (ex: `1.0.0`)

### 3. Executar o Bootstrap

Execute o script bootstrap na raiz do seu projeto:

```bash
./bootstrap.sh
```

O script irá:

1. Clonar o repositório SDD Central Skills
2. Validar que todas as skills listadas em `.specify/skills.yaml` existem
3. Copiar os arquivos de skill para `.specify/skills/<nome>/skill.md`
4. Remover skills que foram descontinuadas (não mais listadas)

**Exemplo de estrutura após bootstrap:**

```
seu-projeto/
├── .specify/
│   ├── skills.yaml
│   └── skills/
│       ├── architecture/hexagonal/
│       │   └── skill.md
│       ├── java-21/
│       │   └── skill.md
│       ├── testing/junit5/
│       │   └── skill.md
│       └── testing/mockito/
│           └── skill.md
├── bootstrap.sh
└── ... (outros arquivos do projeto)
```

### 4. Consultar as Skills

Após o bootstrap, você pode consultar cada skill durante o desenvolvimento:

- **Decisões arquiteturais**: Consulte `.specify/skills/architecture/hexagonal/skill.md`
- **Implementação em Java 21**: Consulte `.specify/skills/java-21/skill.md`
- **Escrita de testes**: Consulte `.specify/skills/testing/junit5/skill.md` e `.specify/skills/testing/mockito/skill.md`

## Skills Disponíveis

### 🏛️ Architecture

#### **Hexagonal Architecture** (`architecture/hexagonal@1.0.0`)

Use esta skill para manter a implementação orientada pela spec enquanto preserva limites claros entre domínio, aplicação e infraestrutura.

**Quando usar:**
- Implementando casos de uso
- Definindo portas (inbound e outbound)
- Estruturando adaptadores (web, persistência, messaging)
- Revisando dependências entre camadas
- Alinhando a arquitetura com as decisões capturadas na spec

**Principais conceitos:**
- Núcleo livre de framework (Spring, JPA, Jackson, HTTP)
- Portas inbound expõem casos de uso
- Portas outbound descrevem dependências do núcleo
- Adaptadores traduzem entrada/saída externa
- Package structure orientada por features ou layers
- Testes focados em cada limite de arquitetura

---

### ☕ Java

#### **Java 21** (`java-21@1.0.0`)

Use esta skill para aplicar funcionalidades do Java 21 onde melhoram clareza, segurança ou alinhamento com a spec.

**Quando usar:**
- Modernizando código Java
- Escolhendo entre tipos imutáveis (records vs classes)
- Implementando alternativas de domínio (sealed types)
- Lidando com ausência de valores (Optional)
- Escrevendo switch expressions e pattern matching
- Utilizando virtual threads e collections modernas

**Principais conceitos:**
- **Records**: para carregadores de dados imutáveis (commands, queries, respostas, value objects)
- **Sealed types**: para alternativas de domínio com conjunto finito
- **Switch expressions**: para ramificações exaustivas e explícitas
- **Pattern matching**: para verificações seguras de tipo
- **Optional**: para retornos possivelmente ausentes (nunca campos ou parâmetros)
- **Collections**: `List.of()`, `Set.of()`, `Map.of()` para fixtures imutáveis
- **Imutabilidade**: preferir para commands, respostas, value objects e eventos de domínio

---

### 🧪 Testing

#### **JUnit 5** (`testing/junit5@1.0.0`)

Use esta skill para traduzir cenários e critérios de aceitação da spec em testes JUnit 5 claros e mantíveis.

**Quando usar:**
- Escrevendo testes unitários de domínio
- Testando serviços de aplicação
- Testando adaptadores (slices)
- Cobrindo critérios de aceitação
- Testando casos limítrofes e caminhos de falha
- Organizando testes por cenários ou regras

**Principais conceitos:**
- **@DisplayName**: para contexto de negócio
- **@Nested**: para agrupar cenários por regra, estado ou ramificação
- **@ParameterizedTest**: para testar comportamento equivalente com muitas entradas
- **assertThrows**: para exceções esperadas com verificação de detalhes
- **Determinismo**: entradas fixas (clocks, IDs, builders de dados)
- **AAA pattern**: Arrange, Act, Assert (mesmo sem comentários)
- **assertAll**: quando múltiplos fatos independentes descrevem o mesmo resultado

#### **Mockito** (`testing/mockito@1.0.0`)

Use esta skill para isolar colaboradores em testes unitários e de serviço de aplicação, mantendo foco no comportamento especificado.

**Quando usar:**
- Mockando portas outbound
- Isolando dependências externas
- Testando interações com colaboradores
- Capturando argumentos para verificação
- Testando cenários de falha de dependências
- Evitando mocks frágeis

**Principais conceitos:**
- **Mock apenas dependências**: fora do comportamento testado (geralmente portas outbound)
- **Objetos reais**: prefira domain objects e value objects sobre mocks
- **Stubbing**: apenas chamadas necessárias ao cenário
- **Verificação**: priorize resultado/estado antes de verificar interações
- **Fronteiras arquiteturais**: mantenha mocks nos limites (não mocke a classe testada)
- **@ExtendWith(MockitoExtension.class)**: setup no JUnit 5
- **@InjectMocks**: funciona melhor com injeção por construtor

---

## Estrutura do Repositório

```
sdd-central-skills/
├── README.md                          # Este arquivo
├── bootstrap.sh                       # Script para bootstrap em projetos consumidores
├── architecture/
│   └── hexagonal/
│       ├── 1.0.0.md                  # Skill: Hexagonal Architecture v1.0.0
│       └── agents/
│           └── openai.yaml           # Configuração de agente (Speckit)
├── java-21/
│   ├── 1.0.0.md                      # Skill: Java 21 v1.0.0
│   ├── 2.0.0.md                      # Skill: Java 21 v2.0.0 (futura)
│   └── agents/
│       └── openai.yaml               # Configuração de agente (Speckit)
└── testing/
    ├── junit5/
    │   ├── 1.0.0.md                  # Skill: JUnit 5 v1.0.0
    │   └── agents/
    │       └── openai.yaml           # Configuração de agente (Speckit)
    └── mockito/
        ├── 1.0.0.md                  # Skill: Mockito v1.0.0
        └── agents/
            └── openai.yaml           # Configuração de agente (Speckit)
```

## Atualizando Skills no Seu Projeto

Para atualizar as skills do seu projeto:

1. **Modifique** `.specify/skills.yaml` adicionando/removendo/atualizando versões
2. **Execute** novamente `./bootstrap.sh`
3. **Commit** as mudanças

Exemplo de atualização:

```yaml
# Antes
skills:
  - java-21@1.0.0

# Depois (usando nova versão)
skills:
  - java-21@2.0.0
```

Após executar `./bootstrap.sh`, o diretório `.specify/skills/java-21/` terá o arquivo `skill.md` atualizado.

## Contribuindo

Para adicionar novas skills ou melhorar as existentes:

1. **Forque** este repositório
2. **Crie** uma branch com o padrão `skill/nome-skill` ou `skill/nome-skill-versao`
3. **Desenvolva** ou melhore a skill seguindo o formato YAML frontmatter + Markdown
4. **Submeta** um Pull Request

### Formato de Skill

Cada skill deve ter o seguinte formato:

```markdown
---
name: nome-da-skill
version: 1.0.0
author: seu-nome
description: Descrição breve e clara do propósito da skill
---

# Título da Skill

Conteúdo em Markdown...

## Seções

- Workflow recomendado
- Padrões e idiomas
- Exemplos de código
- Limites e considerações
```

## Licença

[Adicionar informação de licença conforme necessário]

## Contato

Para dúvidas, sugestões ou issues, por favor abra uma issue neste repositório.
