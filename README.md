# 📚 Sistema de Gerenciamento de Biblioteca

[![Java 17](https://img.shields.io/badge/Java-17-orange?logo=java)](https://www.oracle.com/java/)
[![Maven 3.9](https://img.shields.io/badge/Maven-3.9-C71A36?logo=apache-maven)](https://maven.apache.org/)
[![PostgreSQL 16](https://img.shields.io/badge/PostgreSQL-16-336791?logo=postgresql)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker)](https://www.docker.com/)
[![License](https://img.shields.io/badge/License-Academic-blue)](#)

## 📖 Sobre o Projeto

**Sistema de Gerenciamento de Biblioteca** é uma aplicação web desenvolvida em **Java** para gerenciar as operações de uma biblioteca educacional. Permite que administradores gerenciem o catálogo de livros, autores, alunos e aluguéis, enquanto alunos podem consultar o catálogo disponível e solicitar o aluguel de livros.

### Contexto Acadêmico

> Projeto desenvolvido como **trabalho avaliativo** da disciplina de **Programação Web** do curso de **Análise e Desenvolvimento de Sistemas**.

---

## Funcionalidades

### Área do Administrador

- Gerenciar catálogo de livros (CRUD completo)
- Gerenciar autores (CRUD completo)
- Gerenciar alunos (CRUD completo)
- Administrar aluguéis (visualizar, aprovar, finalizar)
- Dashboard com visão geral do sistema

### Área do Aluno

- Visualizar catálogo de livros disponíveis
- Solicitar aluguel de livros
- Acompanhar meus aluguéis (ativo/histórico)
- Painel pessoal com informações da conta
- Autenticação por CPF

### Segurança

- Hash de senhas com **BCrypt**
- Autenticação baseada em sessão HTTP
- Token de "Lembrar-me" persistido em banco de dados
- Filtro de autenticação protegendo rotas `/admin/*` e `/aluno/*`

---

## Tecnologias Utilizadas

| Categoria            | Tecnologia                          | Versão |
| -------------------- | ----------------------------------- | ------ |
| **Linguagem**        | Java                                | 17     |
| **Framework Web**    | Jakarta Servlet API                 | 6.0    |
| **View**             | Jakarta JSP (JavaServer Pages)      | 3.1    |
| **Tag Library**      | JSTL (Jakarta Standard Tag Library) | 3.0    |
| **Frontend**         | Bootstrap                           | 5.3.3  |
| **Ícones**           | Bootstrap Icons                     | 1.11.3 |
| **Servidor Web**     | Apache Tomcat                       | 10.1   |
| **Banco de Dados**   | PostgreSQL                          | 16     |
| **Build Tool**       | Maven                               | 3.9    |
| **Connection Pool**  | HikariCP                            | 5.1.0  |
| **Hashing de Senha** | jBCrypt                             | 0.4    |
| **Logging**          | SLF4J                               | —      |
| **Containerização**  | Docker & Docker Compose             | —      |

---

## Arquitetura do Projeto

O projeto segue o **padrão MVC (Model-View-Controller)** combinado com o **padrão DAO (Data Access Object)** para acesso aos dados:

- **Controllers (Servlets):** Recebem e processam requisições HTTP, aplicam regras de negócio e redirecionam para as views
- **Views (JSP):** Renderizam as páginas HTML com dados do servidor usando JSTL
- **Models:** Representam as entidades do domínio (Livro, Aluno, Autor, Aluguel, etc.)
- **DAO:** Abstraem toda a comunicação com o banco de dados PostgreSQL via JDBC + HikariCP

### Estrutura de Diretórios

```
src/
├── main/
│   ├── java/com/biblioteca/
│   │   ├── config/
│   │   │   └── ConnectionFactory.java          → Pool de conexões HikariCP
│   │   ├── controller/
│   │   │   ├── AlunoServlet.java               → CRUD de alunos (admin)
│   │   │   ├── AutorServlet.java               → CRUD de autores
│   │   │   ├── CatalogoServlet.java            → Catálogo para alunos
│   │   │   ├── DashboardServlet.java           → Dashboard admin
│   │   │   ├── GerenciarAluguelServlet.java    → Gerenciar aluguéis
│   │   │   ├── LivroServlet.java               → CRUD de livros
│   │   │   ├── LoginAdminServlet.java          → Login admin
│   │   │   ├── LoginAlunoServlet.java          → Login aluno
│   │   │   ├── LogoutServlet.java              → Logout
│   │   │   ├── PainelAlunoServlet.java         → Painel do aluno
│   │   │   └── SolicitacaoServlet.java         → Solicitar aluguel
│   │   ├── dao/
│   │   │   ├── AluguelDAO.java
│   │   │   ├── AlunoDAO.java
│   │   │   ├── AutorDAO.java
│   │   │   ├── LivroDAO.java
│   │   │   ├── TokenDAO.java
│   │   │   └── UsuarioDAO.java
│   │   ├── filter/
│   │   │   ├── AuthFilter.java                 → Validação de autenticação em todas as rotas
│   │   │   └── EncodingFilter.java             → Codificação UTF-8
│   │   ├── listener/
│   │   │   └── StartupListener.java            → Seed de dados na inicialização
│   │   └── model/
│   │       ├── Aluguel.java
│   │       ├── Aluno.java
│   │       ├── Autor.java
│   │       ├── Livro.java
│   │       ├── TokenLogin.java
│   │       ├── Usuario.java
│   │       └── regra/
│   │           ├── RegraAluno.java             → Regras de negócio do aluno
│   │           └── RegraLivro.java             → Regras de negócio do livro
│   └── webapp/
│       ├── index.jsp                           → Página inicial
│       ├── dashboard.jsp
│       ├── login-admin.jsp
│       ├── login-aluno.jsp
│       ├── aluguel/        → formulario.jsp, lista.jsp
│       ├── aluno/          → catalogo.jsp, meus-alugueis.jsp, painel.jsp
│       ├── aluno-admin/    → formulario.jsp, lista.jsp
│       ├── autor/          → formulario.jsp, lista.jsp
│       ├── livro/          → formulario.jsp, lista.jsp
│       ├── layout/         → header.jsp, header-aluno.jsp, footer.jsp
│       ├── css/            → style.css
│       └── WEB-INF/        → web.xml
├── docker-compose.yml
├── Dockerfile              → Build multi-stage (Maven → Tomcat)
├── init.sql                → Schema e dados iniciais
└── pom.xml
```

---

## 🌐 Rotas da Aplicação

| Rota               | Descrição                      | Acesso      |
| ------------------ | ------------------------------ | ----------- |
| `/`                | Página inicial                 | Público     |
| `/loginAdmin`      | Login do administrador         | Público     |
| `/loginAluno`      | Login do aluno                 | Público     |
| `/logout`          | Fazer logout                   | Autenticado |
| `/admin/dashboard` | Dashboard administrativo       | Admin       |
| `/admin/livros`    | CRUD de livros                 | Admin       |
| `/admin/autores`   | CRUD de autores                | Admin       |
| `/admin/alunos`    | CRUD de alunos                 | Admin       |
| `/admin/alugueis`  | Gerenciar aluguéis             | Admin       |
| `/aluno/painel`    | Painel do aluno                | Aluno       |
| `/aluno/catalogo`  | Catálogo de livros disponíveis | Aluno       |
| `/aluno/alugueis`  | Meus aluguéis                  | Aluno       |

---

## 🚀 Como Executar

### ✅ Pré-requisitos

- [Docker](https://www.docker.com/) e Docker Compose instalados
- Porta `8080` disponível

### Execução com Docker (recomendado)

1. **Clone o repositório:**

   ```bash
   git clone <url-do-repositorio>
   cd Projeto-sistema-biblioteca-main
   ```

2. **Inicie os containers:**

   ```bash
   docker-compose up --build
   ```

3. **Acesse a aplicação:**

   ```
   http://localhost:8080
   ```

4. **Para parar os containers:**
   ```bash
   docker-compose down
   ```

### 🔧 Build Manual (sem Docker)

1. **Compile com Maven:**

   ```bash
   mvn clean package
   ```

2. **Deploy no Tomcat:**
   - Copie o arquivo `target/sistema-biblioteca.war` para `$CATALINA_HOME/webapps/`
   - Inicie o Tomcat e acesse `http://localhost:8080/sistema-biblioteca`

---

## Banco de Dados

### PostgreSQL 16

| Tabela         | Descrição                                  |
| -------------- | ------------------------------------------ |
| `usuario`      | Administradores do sistema                 |
| `autor`        | Autores dos livros                         |
| `livro`        | Catálogo de livros com controle de estoque |
| `aluno`        | Dados cadastrais dos alunos                |
| `aluguel`      | Registros de aluguéis com status de fluxo  |
| `tokens_login` | Tokens persistidos para "Lembrar-me"       |

O arquivo `init.sql` contém o schema completo e dados de seed (autores e livros clássicos da literatura brasileira e mundial). O `StartupListener` insere os dados padrão de admin e alunos com senhas hasheadas via BCrypt na inicialização da aplicação.

---

## Credenciais de Acesso Padrão

### Administrador

- Credenciais definidas em `StartupListener.java`

### 👨‍🎓 Aluno (dados de seed)

- **Usuário:** CPF do aluno
- **Senha:** `aluno123`

> ⚠️ **Aviso:** Altere as credenciais padrão antes de qualquer uso em ambiente de produção.

---

_Projeto desenvolvido como trabalho avaliativo da disciplina de **Programação Web** — Curso de **Análise e Desenvolvimento de Sistemas**._
