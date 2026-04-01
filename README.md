# 🎯 Domain-Driven Design (DDD) - Escola

![Java](https://img.shields.io/badge/Java-11+-orange?style=flat-square&logo=java)
![DDD](https://img.shields.io/badge/Architecture-DDD-blue?style=flat-square)
![Maven](https://img.shields.io/badge/Maven-3.6+-red?style=flat-square&logo=apache-maven)
![JUnit](https://img.shields.io/badge/Tests-JUnit%205-green?style=flat-square)

## 📋 Visão Geral

Projeto educacional que demonstra a implementação prática dos **conceitos e padrões do Domain-Driven Design (DDD)** em uma aplicação Java. O sistema modela um contexto escolar com dois bounded contexts: **Acadêmico** (matrícula de alunos) e **Gamificação** (selos e conquistas), aplicando conceitos fundamentais como Linguagem Ubíqua, Aggregates, Eventos de Domínio, Contextos Delimitados e Camada Anticorrupção.

> **🎓 Projeto Educacional**: Desenvolvido com base no curso **"Java e Domain Driven Design: Apresentando os conceitos"** da **Alura**.

---

## 🎯 Objetivo do Projeto

Este projeto tem como objetivos:
- ✅ **Aplicar DDD** na prática com Java
- ✅ **Criar software focado no domínio** do negócio
- ✅ **Organizar código profissionalmente** com camadas bem definidas
- ✅ **Implementar Bounded Contexts** independentes
- ✅ **Utilizar Eventos de Domínio** para comunicação entre contextos
- ✅ **Garantir agregação de valor** ao cliente

---

## 🏗️ Arquitetura DDD

### 📊 Visão Geral dos Bounded Contexts

```
┌─────────────────────────────────────────────────────────────────┐
│                    SISTEMA ESCOLA (DDD)                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │          BOUNDED CONTEXT: ACADÊMICO                        │ │
│  ├────────────────────────────────────────────────────────────┤ │
│  │                                                            │ │
│  │  ┌──────────────────────────────────────────────────────┐  │ │
│  │  │  CAMADA DE APLICAÇÃO                                 │  │ │
│  │  ├──────────────────────────────────────────────────────┤  │ │
│  │  │  • MatricularAluno (Use Case)                        │  │ │
│  │  │  • EnviarEmailIndicacao (Use Case)                   │  │ │
│  │  └──────────────────────────────────────────────────────┘  │ │
│  │                        ▼                                   │ │
│  │  ┌──────────────────────────────────────────────────────┐  │ │
│  │  │  CAMADA DE DOMÍNIO                                   │  │ │
│  │  ├──────────────────────────────────────────────────────┤  │ │
│  │  │  AGGREGATE ROOT:                                     │  │ │
│  │  │  • Aluno (CPF, Nome, Email, Telefones, Senha)        │  │ │
│  │  │                                                      │  │ │
│  │  │  VALUE OBJECTS:                                      │  │ │
│  │  │  • CPF (validação)                                   │  │ │
│  │  │  • Email (validação)                                 │  │ │
│  │  │  • Telefone (DDD + número)                           │  │ │
│  │  │                                                      │  │ │
│  │  │  ENTITIES:                                           │  │ │
│  │  │  • Indicacao                                         │  │ │
│  │  │                                                      │  │ │
│  │  │  DOMAIN EVENTS:                                      │  │ │
│  │  │  • AlunoMatriculado                                  │  │ │
│  │  │                                                      │  │ │
│  │  │  REPOSITORIES (interfaces):                          │  │ │
│  │  │  • RepositorioDeAlunos                               │  │ │
│  │  │                                                      │  │ │
│  │  │  SERVICES:                                           │  │ │
│  │  │  • CifradorDeSenha (interface)                       │  │ │
│  │  │  • LogDeAlunoMatriculado (interface)                 │  │ │
│  │  │  • FabricaDeAluno (Factory)                          │  │ │
│  │  └──────────────────────────────────────────────────────┘  │ │
│  │                        ▼                                   │ │
│  │  ┌──────────────────────────────────────────────────────┐  │ │
│  │  │  CAMADA DE INFRAESTRUTURA                            │  │ │
│  │  ├──────────────────────────────────────────────────────┤  │ │
│  │  │  • RepositorioDeAlunosEmMemoria                      │  │ │
│  │  │  • RepositorioDeAlunosComJDBC                        │  │ │
│  │  │  • CifradorDeSenhaComMD5                             │  │ │
│  │  │  • EnviarEmailIndicacaoComJavaMail                   │  │ │
│  │  └──────────────────────────────────────────────────────┘  │ │
│  └────────────────────────────────────────────────────────────┘ │
│                               │                                 │
│                               │ Domain Event                    │
│                               │ (AlunoMatriculado)              │
│                               ▼                                 │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │          BOUNDED CONTEXT: GAMIFICAÇÃO                      │ │
│  ├────────────────────────────────────────────────────────────┤ │
│  │                                                            │ │
│  │  ┌──────────────────────────────────────────────────────┐  │ │
│  │  │  CAMADA DE APLICAÇÃO                                 │  │ │
│  │  ├──────────────────────────────────────────────────────┤  │ │
│  │  │  • GeraSeloAlunoNovato (Event Handler)               │  │ │
│  │  └──────────────────────────────────────────────────────┘  │ │
│  │                        ▼                                   │ │
│  │  ┌──────────────────────────────────────────────────────┐  │ │
│  │  │  CAMADA DE DOMÍNIO                                   │  │ │
│  │  ├──────────────────────────────────────────────────────┤  │ │
│  │  │  ENTITY:                                             │  │ │
│  │  │  • Selo (CPF do aluno, nome do selo)                 │  │ │
│  │  │                                                      │  │ │
│  │  │  REPOSITORIES (interfaces):                          │  │ │
│  │  │  • RepositorioDeSelos                                │  │ │
│  │  └──────────────────────────────────────────────────────┘  │ │
│  │                        ▼                                   │ │
│  │  ┌──────────────────────────────────────────────────────┐  │ │
│  │  │  CAMADA DE INFRAESTRUTURA                            │  │ │
│  │  ├──────────────────────────────────────────────────────┤  │ │
│  │  │  • RepositorioDeSelosEmMemoria                       │  │ │
│  │  └──────────────────────────────────────────────────────┘  │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                 │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │          SHARED KERNEL (Contexto Compartilhado)            │ │
│  ├────────────────────────────────────────────────────────────┤ │
│  │  • CPF (Value Object compartilhado)                        │ │
│  │  • Publicador (Domain Events)                              │ │
│  │  • Ouvinte (Event Listener)                                │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### 🔧 Padrões DDD Implementados

#### 1️⃣ **Bounded Context (Contexto Delimitado)**
- **Acadêmico**: Gerencia matrícula e dados de alunos
- **Gamificação**: Gerencia selos e conquistas
- **Shared Kernel**: Elementos compartilhados (CPF, Events)

#### 2️⃣ **Linguagem Ubíqua**
Termos do negócio refletidos no código:
- `Aluno`, `Matricular`, `Indicacao`
- `Selo`, `AlunoNovato`
- `CPF`, `Email`, `Telefone`

#### 3️⃣ **Aggregate Root**
- **Aluno**: Raiz do aggregate que controla acesso a Telefones

#### 4️⃣ **Value Objects**
- `CPF`: Imutável, com validação
- `Email`: Validação de formato
- `Telefone`: DDD + número

#### 5️⃣ **Domain Events**
- `AlunoMatriculado`: Evento disparado quando aluno é matriculado
- Comunicação assíncrona entre contextos

#### 6️⃣ **Repository Pattern**
- Abstração para persistência
- Implementações: `EmMemoria`, `ComJDBC`

#### 7️⃣ **Factory Pattern**
- `FabricaDeAluno`: Encapsula criação complexa

#### 8️⃣ **Camada Anticorrupção**
- Event Handlers isolam contextos
- `GeraSeloAlunoNovato` traduz evento do contexto Acadêmico

---

## 🛠️ Tecnologias e Recursos

### ☕ Stack Tecnológico

| Tecnologia | Versão | Propósito |
|------------|--------|-----------|
| **Java** | 11+ | Linguagem de programação |
| **Maven** | 3.6+ | Gerenciamento de dependências |
| **JUnit Jupiter** | 5.6.2 | Framework de testes |

### 📦 Estrutura de Pacotes

```
br.com.alura.escola/
│
├── 📂 academico/                           # Bounded Context Acadêmico
│   ├── aplicacao/                          # Casos de uso
│   │   ├── aluno/
│   │   │   └── matricular/
│   │   │       ├── MatricularAluno         # Use Case
│   │   │       └── MatricularAlunoDto      # DTO
│   │   └── indicacao/
│   │       └── EnviarEmailIndicacao        # Use Case
│   │
│   ├── dominio/                            # Camada de domínio
│   │   ├── aluno/
│   │   │   ├── Aluno                       # Aggregate Root
│   │   │   ├── AlunoMatriculado            # Domain Event
│   │   │   ├── AlunoNaoEncontrado          # Exception
│   │   │   ├── Email                       # Value Object
│   │   │   ├── Telefone                    # Value Object
│   │   │   ├── RepositorioDeAlunos         # Interface
│   │   │   ├── CifradorDeSenha             # Interface
│   │   │   ├── LogDeAlunoMatriculado       # Interface
│   │   │   └── FabricaDeAluno              # Factory
│   │   └── indicacao/
│   │       └── Indicacao                   # Entity
│   │
│   └── infra/                              # Infraestrutura
│       ├── aluno/
│       │   ├── RepositorioDeAlunosEmMemoria
│       │   ├── RepositorioDeAlunosComJDBC
│       │   └── CifradorDeSenhaComMD5
│       └── indicacao/
│           └── EnviarEmailIndicacaoComJavaMail
│
├── 📂 gamificacao/                         # Bounded Context Gamificação
│   ├── aplicacao/
│   │   └── GeraSeloAlunoNovato            # Event Handler
│   │
│   ├── dominio/
│   │   └── selo/
│   │       ├── Selo                        # Entity
│   │       └── RepositorioDeSelos          # Interface
│   │
│   └── infra/
│       └── selo/
│           └── RepositorioDeSelosEmMemoria
│
└── 📂 shared/                              # Shared Kernel
    └── dominio/
        ├── CPF                             # Value Object compartilhado
        └── evento/
            ├── Publicador                  # Event Publisher
            ├── Ouvinte                     # Event Listener
            └── Evento                      # Interface base
```

### 📊 Métricas do Projeto

- **Total de classes Java**: 32
- **Bounded Contexts**: 2 (Acadêmico, Gamificação)
- **Aggregate Roots**: 1 (Aluno)
- **Value Objects**: 3 (CPF, Email, Telefone)
- **Domain Events**: 1 (AlunoMatriculado)
- **Repositories**: 2 interfaces com 3 implementações
- **Use Cases**: 3

---

## 📁 Estrutura do Projeto

```
domain-driven-design-main/
│
├── 📄 README.md                            # Documentação original
├── 📄 README_TECNICO.md                    # Este arquivo
├── 📄 .gitignore
│
└── 📂 1964-java-domain-driven-design-projeto_inicial/
    ├── 📄 pom.xml                          # Configuração Maven
    ├── 📂 src/
    │   ├── 📂 main/java/                   # Código-fonte
    │   │   └── br/com/alura/escola/
    │   │       ├── academico/
    │   │       ├── gamificacao/
    │   │       └── shared/
    │   │
    │   └── 📂 test/java/                   # Testes
    │       └── br/com/alura/escola/
    │
    └── 📂 bin/                             # Classes compiladas
```

---

## 🚀 Configuração e Execução

### 📋 Pré-requisitos

#### Obrigatórios
- ✅ **Java JDK 11** ou superior
  ```bash
  java -version
  ```
- ✅ **Maven 3.6+**
  ```bash
  mvn -version
  ```

#### Opcionais
- 🖥️ **Eclipse** ou **IntelliJ IDEA**
- 🐘 **PostgreSQL** ou outro banco (para RepositorioComJDBC)

---

## 🔧 Instalação e Build

### 1. Clonar/Baixar o Projeto
```bash
cd domain-driven-design-main/1964-java-domain-driven-design-projeto_inicial
```

### 2. Compilar o Projeto
```bash
mvn clean compile
```

**Saída esperada**:
```
[INFO] BUILD SUCCESS
[INFO] Total time: 3.456 s
```

### 3. Executar Testes
```bash
mvn test
```

### 4. Gerar JAR
```bash
mvn clean package
```

---

## 🔍 Análise Detalhada dos Componentes

### 📦 Contexto Acadêmico

#### **Aluno.java** - Aggregate Root
```java
// AGGREGATE ROOT
public class Aluno {
    private CPF cpf;
    private String nome;
    private Email email;
    private List<Telefone> telefones = new ArrayList<>();
    private String senha;
    
    public Aluno(CPF cpf, String nome, Email email) {
        this.cpf = cpf;
        this.nome = nome;
        this.email = email;
    }
    
    public void adicionarTelefone(String ddd, String numero) {
        if (telefones.size() == 2) {
            throw new IllegalArgumentException(
                "Número máximo de telefones já atingido!"
            );
        }
        this.telefones.add(new Telefone(ddd, numero));
    }
}
```

**Características**:
- ✅ **Aggregate Root**: Controla acesso aos Telefones
- ✅ **Encapsulamento**: Regras de negócio protegidas
- ✅ **Invariantes**: Máximo 2 telefones por aluno
- ✅ **Value Objects**: CPF, Email, Telefone

---

#### **CPF.java** - Value Object (Shared Kernel)
```java
public class CPF {
    private String numero;
    
    public CPF(String numero) {
        if (numero == null || !numero.matches("\\d{11}")) {
            throw new IllegalArgumentException("CPF inválido!");
        }
        this.numero = numero;
    }
    
    public String getNumero() {
        return numero;
    }
}
```

**Características**:
- ✅ **Imutável**: Não possui setters
- ✅ **Validação**: No construtor
- ✅ **Compartilhado**: Entre contextos Acadêmico e Gamificação
- ✅ **Igualdade por valor**: Não por identidade

---

#### **Email.java** - Value Object
```java
public class Email {
    private String endereco;
    
    public Email(String endereco) {
        if (endereco == null || !endereco.matches("^[\\w-\\.]+@([\\w-]+\\.)+[\\w-]{2,4}$")) {
            throw new IllegalArgumentException("Email inválido!");
        }
        this.endereco = endereco;
    }
    
    public String getEndereco() {
        return endereco;
    }
}
```

**Características**:
- ✅ **Validação de formato**: Regex para email
- ✅ **Imutabilidade**: Garantida
- ✅ **Semântica**: Expressa conceito do domínio

---

#### **Telefone.java** - Value Object
```java
public class Telefone {
    private String ddd;
    private String numero;
    
    public Telefone(String ddd, String numero) {
        if (ddd == null || numero == null) {
            throw new IllegalArgumentException("DDD e número são obrigatórios!");
        }
        
        if (!ddd.matches("\\d{2}")) {
            throw new IllegalArgumentException("DDD inválido!");
        }
        
        if (!numero.matches("\\d{8}|\\d{9}")) {
            throw new IllegalArgumentException("Número inválido!");
        }
        
        this.ddd = ddd;
        this.numero = numero;
    }
}
```

**Características**:
- ✅ **Validação**: DDD (2 dígitos) e número (8 ou 9 dígitos)
- ✅ **Composição**: Parte do Aggregate Aluno

---

#### **AlunoMatriculado.java** - Domain Event
```java
public class AlunoMatriculado implements Evento {
    private final CPF cpfDoAluno;
    private final LocalDateTime momento;
    
    public AlunoMatriculado(CPF cpfDoAluno) {
        this.cpfDoAluno = cpfDoAluno;
        this.momento = LocalDateTime.now();
    }
    
    public CPF getCpfDoAluno() {
        return cpfDoAluno;
    }
    
    public LocalDateTime getMomento() {
        return momento;
    }
}
```

**Características**:
- ✅ **Imutável**: Final fields
- ✅ **Timestamp**: Momento do evento
- ✅ **Comunicação**: Entre Bounded Contexts
- ✅ **Desacoplamento**: Contextos não se conhecem diretamente

---

#### **MatricularAluno.java** - Use Case
```java
public class MatricularAluno {
    private final RepositorioDeAlunos repositorio;
    
    public MatricularAluno(RepositorioDeAlunos repositorio) {
        this.repositorio = repositorio;
    }
    
    public void executar(MatricularAlunoDto dados) {
        Aluno aluno = dados.criarAluno();
        repositorio.matricular(aluno);
        
        // Publicar evento de domínio
        Publicador.publicar(new AlunoMatriculado(aluno.getCpf()));
    }
}
```

**Características**:
- ✅ **Caso de Uso**: Orquestra operação de matrícula
- ✅ **Camada de Aplicação**: Não contém lógica de domínio
- ✅ **Publica Evento**: Notifica outros contextos
- ✅ **Dependência de Interface**: Repository pattern

---

#### **FabricaDeAluno.java** - Factory
```java
public class FabricaDeAluno {
    private Aluno aluno;
    
    public FabricaDeAluno comNomeCPFEmail(String nome, String cpf, String email) {
        this.aluno = new Aluno(new CPF(cpf), nome, new Email(email));
        return this;
    }
    
    public FabricaDeAluno comTelefone(String ddd, String numero) {
        this.aluno.adicionarTelefone(ddd, numero);
        return this;
    }
    
    public Aluno criar() {
        return this.aluno;
    }
}
```

**Características**:
- ✅ **Fluent Interface**: Chamadas encadeadas
- ✅ **Encapsulamento**: Criação complexa isolada
- ✅ **Builder Pattern**: Construção passo a passo

---

### 📦 Contexto Gamificação

#### **Selo.java** - Entity
```java
public class Selo {
    private CPF cpfDoAluno;
    private String nomeSelo;
    
    public Selo(CPF cpfDoAluno, String nomeSelo) {
        this.cpfDoAluno = cpfDoAluno;
        this.nomeSelo = nomeSelo;
    }
    
    public CPF getCpfDoAluno() {
        return cpfDoAluno;
    }
    
    public String getNomeSelo() {
        return nomeSelo;
    }
}
```

**Características**:
- ✅ **Entity**: Identidade baseada em CPF + Nome
- ✅ **Contexto Separado**: Não conhece Aluno
- ✅ **CPF compartilhado**: Via Shared Kernel

---

#### **GeraSeloAlunoNovato.java** - Event Handler
```java
public class GeraSeloAlunoNovato extends Ouvinte {
    private final RepositorioDeSelos repositorio;
    
    public GeraSeloAlunoNovato(RepositorioDeSelos repositorio) {
        this.repositorio = repositorio;
    }
    
    @Override
    public void reageAo(Evento evento) {
        if (evento instanceof AlunoMatriculado) {
            AlunoMatriculado alunoMatriculado = (AlunoMatriculado) evento;
            Selo novato = new Selo(
                alunoMatriculado.getCpfDoAluno(),
                "Novato"
            );
            repositorio.adicionar(novato);
        }
    }
}
```

**Características**:
- ✅ **Event Handler**: Reage a eventos de outro contexto
- ✅ **Camada Anticorrupção**: Traduz evento para seu domínio
- ✅ **Desacoplamento**: Não conhece contexto Acadêmico
- ✅ **Single Responsibility**: Apenas gera selo

---

### 🔌 Camada de Infraestrutura

#### **RepositorioDeAlunosEmMemoria.java**
```java
public class RepositorioDeAlunosEmMemoria implements RepositorioDeAlunos {
    private List<Aluno> matriculados = new ArrayList<>();
    
    @Override
    public void matricular(Aluno aluno) {
        this.matriculados.add(aluno);
    }
    
    @Override
    public Aluno buscarPorCPF(CPF cpf) {
        return this.matriculados.stream()
            .filter(a -> a.getCpf().equals(cpf))
            .findFirst()
            .orElseThrow(() -> new AlunoNaoEncontrado(cpf));
    }
}
```

**Características**:
- ✅ **Implementação em memória**: Para testes
- ✅ **Independente do domínio**: Pode ser trocada
- ✅ **Repository Pattern**: Interface no domínio

---

#### **CifradorDeSenhaComMD5.java**
```java
public class CifradorDeSenhaComMD5 implements CifradorDeSenha {
    
    @Override
    public String cifrarSenha(String senha) {
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");
            md.update(senha.getBytes());
            byte[] bytes = md.digest();
            
            StringBuilder sb = new StringBuilder();
            for (byte b : bytes) {
                sb.append(String.format("%02x", b));
            }
            return sb.toString();
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException("Erro ao cifrar senha", e);
        }
    }
}
```

**Características**:
- ✅ **Detalhe de implementação**: MD5
- ✅ **Interface no domínio**: `CifradorDeSenha`
- ✅ **Inversão de dependência**: Domínio não depende de infra

---

### 🌐 Shared Kernel

#### **Publicador.java** - Event Publisher
```java
public class Publicador {
    private static List<Ouvinte> ouvintes = new ArrayList<>();
    
    public static void adicionar(Ouvinte ouvinte) {
        ouvintes.add(ouvinte);
    }
    
    public static void publicar(Evento evento) {
        ouvintes.forEach(ouvinte -> ouvinte.reageAo(evento));
    }
    
    public static void limpar() {
        ouvintes.clear();
    }
}
```

**Características**:
- ✅ **Observer Pattern**: Publicador/Observador
- ✅ **Comunicação entre contextos**: Via eventos
- ✅ **Desacoplamento**: Contextos independentes

---

## 🎓 Conceitos DDD Abordados

### ✅ 1. Linguagem Ubíqua (Ubiquitous Language)

**Definição**: Linguagem comum entre desenvolvedores e especialistas do domínio.

**Aplicação no Projeto**:
- Termos do negócio no código: `Aluno`, `Matricular`, `Selo`, `Indicacao`
- Classes refletem conceitos reais da escola
- Métodos com nomes expressivos: `adicionarTelefone()`, `matricular()`

**Exemplo**:
```java
// ❌ Ruim (linguagem técnica)
public class Person {
    private String id;
    private String field1;
}

// ✅ Bom (linguagem ubíqua)
public class Aluno {
    private CPF cpf;
    private Email email;
}
```

---

### ✅ 2. Bounded Context (Contexto Delimitado)

**Definição**: Limite explícito onde um modelo de domínio é aplicável.

**Aplicação no Projeto**:
- **Contexto Acadêmico**: Matrícula, alunos, indicações
- **Contexto Gamificação**: Selos, conquistas
- **Shared Kernel**: CPF, eventos

**Benefícios**:
- 🔸 Modelos focados e coesos
- 🔸 Times podem trabalhar independentemente
- 🔸 Facilita manutenção

---

### ✅ 3. Aggregate (Agregado)

**Definição**: Grupo de objetos que são tratados como uma unidade.

**Aplicação no Projeto**:
```java
// Aluno é o Aggregate Root
public class Aluno {
    private List<Telefone> telefones; // Parte do aggregate
    
    // Controla acesso aos telefones
    public void adicionarTelefone(String ddd, String numero) {
        // Valida regra de negócio
        if (telefones.size() == 2) {
            throw new IllegalArgumentException("Máximo atingido!");
        }
        this.telefones.add(new Telefone(ddd, numero));
    }
}
```

**Regras**:
- 🔸 Apenas o root é acessado externamente
- 🔸 Invariantes protegidas
- 🔸 Modificações sempre pelo root

---

### ✅ 4. Value Object (Objeto de Valor)

**Definição**: Objetos imutáveis definidos por seus atributos.

**Aplicação no Projeto**:
```java
// CPF é um Value Object
public class CPF {
    private final String numero;
    
    public CPF(String numero) {
        // Validação
        this.numero = numero;
    }
    
    // Sem setters (imutável)
    
    @Override
    public boolean equals(Object o) {
        // Igualdade por valor
    }
}
```

**Características**:
- 🔸 Imutáveis
- 🔸 Sem identidade
- 🔸 Igualdade por valor
- 🔸 Validações no construtor

---

### ✅ 5. Domain Events (Eventos de Domínio)

**Definição**: Algo que aconteceu no domínio que outros contextos podem querer saber.

**Aplicação no Projeto**:
```java
// Evento
public class AlunoMatriculado implements Evento {
    private final CPF cpfDoAluno;
    private final LocalDateTime momento;
}

// Publicação
Publicador.publicar(new AlunoMatriculado(aluno.getCpf()));

// Escuta
public class GeraSeloAlunoNovato extends Ouvinte {
    public void reageAo(Evento evento) {
        // Reage ao evento
    }
}
```

**Benefícios**:
- 🔸 Desacoplamento entre contextos
- 🔸 Comunicação assíncrona
- 🔸 Auditoria e rastreabilidade

---

### ✅ 6. Repository Pattern

**Definição**: Abstração para acesso a dados.

**Aplicação no Projeto**:
```java
// Interface no domínio
public interface RepositorioDeAlunos {
    void matricular(Aluno aluno);
    Aluno buscarPorCPF(CPF cpf);
}

// Implementação na infraestrutura
public class RepositorioDeAlunosEmMemoria implements RepositorioDeAlunos {
    // Detalhes de implementação
}

public class RepositorioDeAlunosComJDBC implements RepositorioDeAlunos {
    // Detalhes de implementação
}
```

**Benefícios**:
- 🔸 Domínio não conhece persistência
- 🔸 Fácil trocar implementações
- 🔸 Testabilidade

---

### ✅ 7. Anticorruption Layer (Camada Anticorrupção)

**Definição**: Camada que traduz entre contextos diferentes.

**Aplicação no Projeto**:
```java
// GeraSeloAlunoNovato é uma camada anticorrupção
public class GeraSeloAlunoNovato extends Ouvinte {
    public void reageAo(Evento evento) {
        // Traduz evento do contexto Acadêmico
        // para o domínio da Gamificação
        if (evento instanceof AlunoMatriculado) {
            CPF cpf = ((AlunoMatriculado) evento).getCpfDoAluno();
            // Cria Selo (conceito da Gamificação)
            Selo novato = new Selo(cpf, "Novato");
        }
    }
}
```

**Benefícios**:
- 🔸 Protege integridade do modelo
- 🔸 Contextos independentes
- 🔸 Evita "contaminação" de conceitos

---

### ✅ 8. Factory Pattern

**Definição**: Encapsula criação complexa de objetos.

**Aplicação no Projeto**:
```java
Aluno aluno = new FabricaDeAluno()
    .comNomeCPFEmail("João", "12345678900", "joao@email.com")
    .comTelefone("11", "987654321")
    .comTelefone("11", "123456789")
    .criar();
```

**Benefícios**:
- 🔸 Criação complexa isolada
- 🔸 Interface fluente
- 🔸 Código mais legível

---

## 📊 Camadas da Arquitetura

### 1️⃣ Camada de Aplicação
- **Responsabilidade**: Casos de uso, orquestração
- **Componentes**: `MatricularAluno`, `GeraSeloAlunoNovato`
- **Características**: 
  - ✅ Sem lógica de domínio
  - ✅ Coordena operações
  - ✅ Publica eventos

### 2️⃣ Camada de Domínio
- **Responsabilidade**: Lógica de negócio
- **Componentes**: Entities, Value Objects, Aggregates, Events
- **Características**:
  - ✅ Independente de frameworks
  - ✅ Expressa regras do negócio
  - ✅ Linguagem ubíqua

### 3️⃣ Camada de Infraestrutura
- **Responsabilidade**: Detalhes técnicos
- **Componentes**: Repositories, Services concretos
- **Características**:
  - ✅ Implementações de interfaces
  - ✅ Acesso a BD, APIs, etc
  - ✅ Substituível

---

## 🐛 Troubleshooting

### ❌ Problema: Erro de compilação

**Sintomas**:
```
[ERROR] Failed to execute goal
```

**Solução**:
```bash
# Verificar versão do Java
java -version

# Deve ser 11 ou superior
mvn clean install
```

---

### ❌ Problema: Testes falhando

**Solução**:
```bash
# Rodar testes individuais
mvn test -Dtest=NomeDaClasse

# Ver stack trace completo
mvn test -X
```

---

## 📚 Recursos de Aprendizado

### 📖 Livros Recomendados
- **Domain-Driven Design** - Eric Evans (Blue Book)
- **Implementing Domain-Driven Design** - Vaughn Vernon (Red Book)
- **Domain-Driven Design Distilled** - Vaughn Vernon
- **Patterns, Principles, and Practices of DDD** - Scott Millett

### 🎓 Curso de Referência
- **[Java e Domain Driven Design: Apresentando os conceitos](https://www.alura.com.br/)** - Alura
  - Linguagem Ubíqua
  - Aggregates e Entities
  - Value Objects
  - Domain Events
  - Bounded Contexts
  - Shared Kernel
  - Camada Anticorrupção

### 📺 Recursos Adicionais
- [DDD Community](https://github.com/ddd-crew)
- [Domain-Driven Design Weekly](https://www.dddweekly.com/)
- [Context Mapping](https://github.com/ddd-crew/context-mapping)

---

## 🗺️ Roadmap de Melhorias

### 🎯 Curto Prazo
- [x] Implementar Value Objects
- [x] Criar Aggregates
- [x] Adicionar Domain Events
- [ ] Implementar validações completas
- [ ] Adicionar mais testes unitários

### 🎯 Médio Prazo
- [ ] Implementar persistência com JPA
- [ ] Adicionar API REST
- [ ] Implementar CQRS (Command Query Responsibility Segregation)
- [ ] Event Sourcing
- [ ] Documentar Context Map

### 🎯 Longo Prazo
- [ ] Microsserviços por Bounded Context
- [ ] Event Store
- [ ] Saga Pattern para transações distribuídas
- [ ] Implementar mais Bounded Contexts

---

## 📝 Notas Importantes

### ⚠️ Avisos
- 🔴 **Projeto Educacional**: Foco em conceitos DDD
- 🟡 **Sem Persistência Real**: Usa repositórios em memória
- 🟠 **MD5 para senha**: Não recomendado para produção

### ✅ Pontos Fortes
- ✔️ Bounded Contexts bem definidos
- ✔️ Linguagem Ubíqua consistente
- ✔️ Separação clara de camadas
- ✔️ Padrões DDD aplicados corretamente

### 📦 Informações Técnicas
- **Java Version**: 11
- **Maven**: 3.6+
- **Teste Framework**: JUnit 5
- **Total de Classes**: 32

---

## 📄 Licença

Projeto educacional para fins de aprendizado de Domain-Driven Design.

---

## 🤝 Suporte e Contato

Para dúvidas sobre DDD:
- 📧 [DDD Community](https://github.com/ddd-crew)
- 💬 Stack Overflow (tag: domain-driven-design)
- 📚 Alura - Plataforma de Cursos

---

**Desenvolvido com Domain-Driven Design** 🎯 | **Arquitetura Focada no Domínio**  
**Baseado no curso**: [Java e Domain Driven Design - Alura](https://www.alura.com.br/) 🎓  
*Última atualização: Abril 2024*
