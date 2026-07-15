# Expense Tracker API

API responsável pelo gerenciamento financeiro do Expense Tracker.

O backend está em fase inicial de desenvolvimento e utiliza um banco H2 persistente para simplificar a execução no ambiente local. Nenhuma instalação externa de banco de dados é necessária.

## Tecnologias

| Tecnologia | Finalidade                            |
|------------|---------------------------------------|
| Java 21 | Linguagem e runtime                   |
| Spring Boot 4.1 | Framework da aplicação                |
| Spring Web MVC | Desenvolvimento da API HTTP           |
| Spring Data JPA | Persistência de dados                 |
| Hibernate | Implementação ORM                     |
| H2 Database | Banco local de desenvolvimento        |
| Maven Wrapper | Build e gerenciamento de dependências |

## Pré-requisitos

Antes de executar o backend, verifique:

```bash
java -version
```
O ambiente deve utilizar Java 21 ou superior.

Não é necessário instalar o Maven globalmente, pois o projeto já inclui o Maven Wrapper.

## Configuração local

A aplicação recebe sua configuração de banco por variáveis de ambiente:


| Variável            | Obrigatória | Exemplo local               | Descrição            |
|---------------------|------------|-----------------------------|----------------------|
| `SPRING_PROFILES_ACTIVE` | Sim | `local` | Ativa a configuração de desenvolvimento local | 
| `DATABASE_URL`      |         Sim | `jdbc:h2:file:./data/appdb` | URL JDBC do banco    |
| `DATABASE_USERNAME` |         Sim | `dbadmin`                   | Usuário do H2        |
| `DATABASE_PASSWORD` |         Não | vazio                       | Senha do banco local |


As credenciais locais não devem ser utilizadas em ambientes de produção.

## Execução pelo IntelliJ IDEA

Crie uma configuração do tipo **Application**:

| Campo | Valor                                              |
|-------|----------------------------------------------------|
| Name | `ExpenseTrackerBackend`                            |
| Main class | `com.user.expensetracker.ExpenseTrackerApplication` |
| Module/classpath | `expense-tracker`                                  |
| Working directory | `<raiz-do-repositorio>/backend`                    |
| JRE | Java 21                                            |

Adicione as variáveis:

```text
SPRING_PROFILES_ACTIVE=local
DATABASE_URL=jdbc:h2:file:./data/appdb
DATABASE_USERNAME=dbadmin
DATABASE_PASSWORD=
```

Execute a configuração e aguarde:

```text
Started ExpenseTrackerApplication
```

A aplicação ficará disponível em:

```text
http://localhost:8080
```

Um `404` na rota raiz é esperado enquanto a API não possuir controllers.

## Execução pelo terminal

Acesse o diretório:

```bash
cd backend
```

Configure o ambiente:

```bash
export SPRING_PROFILES_ACTIVE="local"
export DATABASE_URL="jdbc:h2:file:./data/appdb"
export DATABASE_USERNAME="dbadmin"
export DATABASE_PASSWORD=""
```

Inicie a aplicação:

```bash
./mvnw spring-boot:run
```

## Console do H2

Com a aplicação em execução, acesse:

```text
http://localhost:8080/h2-console
```

Utilize:

| Campo | Valor                       |
|---|-----------------------------|
| Driver Class | `org.h2.Driver`             |
| JDBC URL | `jdbc:h2:file:./data/appdb` |
| User Name | `dbadmin`                   |
| Password | vazio                       |

A URL informada no console deve ser idêntica ao valor de `DATABASE_URL`.

Para validar conexão:

```sql
SELECT 1 AS HEALTH_CHECK;
```

O resultado esperado é:

```text
1
```

## Persistência do banco

O banco local é armazenado em:

```text
backend/data/appdb.mv.db
```

Esse arquivo é específico de cada ambiente de desenvolvimento e não deve ser versionado.

Para recriar o banco local, pare a aplicação e remova manualmente os arquivos existentes em `backend/data/`.

> Essa operação apaga todos os dados locais.

## Build e testes

Execute:

```bash
./mvnw clean verify
```

O build deve terminar com:

```text
BUILD SUCCESS
```

## Solução de problemas

### 1. O módulo do backend não aparece no IntelliJ

Clique com o botão direito em `backend/pom.xml`, selecione **Add as Maven Project** e recarregue os projetos Maven.

### 2. A aplicação não encontra uma variável

Verifique se os nomes configurados no IntelliJ correspondem exatamente a:

```text
DATABASE_URL
DATABASE_USERNAME
DATABASE_PASSWORD
```

### 3. O console H2 não conecta

Confirme que:

- a aplicação está em execução;
- o perfil `local` está ativo;
- a JDBC URL é idêntica a `DATABASE_URL`;
- o diretório de trabalho do IntelliJ é `backend`;
- nenhuma outra instância está usando o arquivo do banco.

### 4. O banco foi criado no diretório errado

O caminho `./data/appdb` é relativo ao diretório de trabalho. Configure o IntelliJ para executar a aplicação a partir de `backend`.

## Limitações do ambiente local

O H2 é utilizado apenas durante o desenvolvimento inicial. O ambiente de produção utilizará PostgreSQL em uma etapa futura.

O console H2, a atualização automática do schema e a exibição de SQL não devem ser habilitados em produção.

## Autor

Desenvolvido por Pedro H. Stucky.