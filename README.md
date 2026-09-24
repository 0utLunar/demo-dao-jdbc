# Demo DAO JDBC

![Java](https://img.shields.io/badge/Java-25-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-4-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![JDBC](https://img.shields.io/badge/JDBC-yes-silver?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green.svg)

Aplicação Java que implementa o padrão **DAO** (Data Access Object) com **JDBC** e **MySQL**, desenvolvida durante a seção de JDBC do curso [*Java COMPLETO: Programação Orientada a Objetos + Projetos*](https://www.udemy.com/course/java-curso-completo/) do Prof. Nélio Alves.

## Funcionalidades

CRUD completo para as entidades `Seller` e `Department`:

| Operação | `SellerDao` | `DepartmentDao` |
|---|---|---|
| Inserir | `insert` | `insert` |
| Atualizar | `update` | `update` |
| Deletar | `deleteById` | `deleteById` |
| Buscar por id | `findById` | `findById` |
| Listar todos | `findAll` | `findAll` |
| Buscar por departamento | `findByDepartment` | — |

Os programas `Program` (Seller) e `Program2` (Department) executam uma bateria de testes exercitando cada operação. O teste de `deleteById` pede um id pelo console.

## Técnicas e conceitos

- Padrão **DAO** com interfaces e implementações separadas
- **JDBC** com `DriverManager` e `PreparedStatement` (proteção contra SQL injection)
- Mapeamento manual de `ResultSet` → objeto, com JOIN e reuso de instâncias via `HashMap`
- **DaoFactory** para desacoplar a instanciação dos DAOs
- Conexão única reutilizada via classe utilitária `DB`
- Exceções customizadas: `DbException` e `DbIntegrityException`

## Estrutura do projeto

```
demo-dao-jdbc/
├── db.properties.example
├── pom.xml
└── src/main/java/
    ├── application/
    │   ├── Program.java          # testes CRUD de Seller
    │   └── Program2.java         # testes CRUD de Department
    ├── db/
    │   ├── DB.java               # conexão e fechamento de recursos
    │   ├── DbException.java
    │   └── DbIntegrityException.java
    └── model/
        ├── dao/
        │   ├── DaoFactory.java
        │   ├── SellerDao.java
        │   ├── DepartmentDao.java
        │   └── impl/
        │       ├── SellerDaoJDBC.java
        │       └── DepartmentDaoJDBC.java
        └── entities/
            ├── Seller.java
            └── Department.java
```

## Pré-requisitos

- JDK 25+
- Maven
- MySQL Server instalado e rodando na porta `3306`

## Configuração do banco

1. Crie o schema `coursejdbc` e execute o script abaixo:

```sql
CREATE TABLE department (
  Id int NOT NULL AUTO_INCREMENT,
  Name varchar(60) DEFAULT NULL,
  PRIMARY KEY (Id)
);

CREATE TABLE seller (
  Id int NOT NULL AUTO_INCREMENT,
  Name varchar(60) NOT NULL,
  Email varchar(100) NOT NULL,
  BirthDate datetime NOT NULL,
  BaseSalary double NOT NULL,
  DepartmentId int NOT NULL,
  PRIMARY KEY (Id),
  FOREIGN KEY (DepartmentId) REFERENCES department (Id)
);
```

2. (Opcional) Popule com dados de exemplo:

```sql
INSERT INTO department (Name) VALUES
  ('Computers'),
  ('Electronics'),
  ('Fashion'),
  ('Books');

INSERT INTO seller (Name, Email, BirthDate, BaseSalary, DepartmentId) VALUES
  ('Bob Brown',   'bob@gmail.com',   '1998-04-21 00:00:00', 1000, 1),
  ('Maria Green', 'maria@gmail.com', '1979-12-31 00:00:00', 3500, 2),
  ('Alex Grey',   'alex@gmail.com',  '1988-01-15 00:00:00', 2200, 3),
  ('Martha Red',  'martha@gmail.com','1993-11-30 00:00:00', 3000, 4),
  ('Donald Blue', 'donald@gmail.com','2000-01-09 00:00:00', 4000, 3),
  ('Alex Pink',   'alex@gmail.com',  '1997-03-04 00:00:00', 3000, 2);
```

3. Configure as credenciais de acesso:

```bash
cp db.properties.example db.properties
```

Edite o `db.properties` com seu usuário e senha do MySQL.

## Como executar

Rode a classe `application.Program` (teste de `Seller`) ou `application.Program2` (teste de `Department`) pela sua IDE de preferência.

## Créditos

Projeto didático desenvolvido como exercício da seção de **JDBC** do curso *Java COMPLETO: Programação Orientada a Objetos + Projetos* do **Prof. Nélio Alves** (DevSuperior).

## Licença

Distribuído sob a licença MIT. Veja `LICENSE` para mais informações.