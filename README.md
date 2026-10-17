# Checkpoint 2 – Aplicação CRUD com Spring Boot (SOA e Web Services)

API ReST desenvolvida com Spring Boot, expondo um CRUD para cadastrar e gerenciar
pedidos de clientes. Checkpoint 2 da disciplina **Arquitetura SOA e Web Services**.

## Aluno
- Belton Lee Carr De Muzio Meira - RM560760

## Tecnologias
- Java 21
- Spring Boot 3.5.16
- Spring Web, Spring Data JPA, Validation, Spring Boot DevTools, Lombok
- H2 Database (com console web)
- Maven

## Estrutura do projeto

```
src/main/java/br/com/fiap/checkpoint2/
├── Checkpoint2Application.java
├── model/OrderModel.java
├── repository/OrderRepository.java
├── service/OrderService.java
└── controller/OrderController.java

src/main/resources/
└── application.properties
```

## Entidade OrderModel (tabela `Pedidos`)

| Atributo    | Tipo       | Regra                                          |
|-------------|------------|------------------------------------------------|
| id          | Long       | chave primária, gerada automaticamente          |
| clientName  | String     | obrigatório (`@NotEmpty`)                       |
| orderDate   | LocalDate  | preenchida com a data atual (`@PrePersist`)     |
| totalValue  | BigDecimal | não pode ser negativo (`@DecimalMin`, `@Positive`) |

## Endpoints

| Método | Rota            | Descrição                    | Resposta             |
|--------|-----------------|------------------------------|----------------------|
| GET    | /orders         | Lista todos os pedidos       | 200 OK               |
| GET    | /orders/{code}  | Busca um pedido pelo id      | 200 OK / 404         |
| POST   | /orders         | Cria um novo pedido          | 201 Created / 400    |
| PUT    | /orders/{code}  | Atualiza um pedido existente | 200 OK / 404         |
| DELETE | /orders/{code}  | Remove um pedido pelo id     | 204 No Content       |

## Como executar

Abra a pasta do projeto no IntelliJ, espere o Maven baixar as dependências e rode a
classe `Checkpoint2Application`. Ou, pelo terminal:

```
mvn spring-boot:run
```

A aplicação sobe em `http://localhost:8085`.

## Console do H2

Acesse `http://localhost:8085/h2-console`:

| Campo     | Valor                    |
|-----------|--------------------------|
| JDBC URL  | `jdbc:h2:file:~/testdb`  |
| User Name | `sa`                     |
| Password  | `password`               |

## Testando a API

Importe `postman/checkpoint2.postman_collection.json` no Postman. A coleção traz os
cinco testes obrigatórios e dois testes de validação.

```bash
# 1. Criar um novo pedido
curl -X POST http://localhost:8085/orders -H "Content-Type: application/json" -d '{"clientName":"Maria Silva","totalValue":249.90}'

# 2. Buscar todos os pedidos
curl http://localhost:8085/orders

# 3. Buscar um pedido pelo ID
curl http://localhost:8085/orders/1

# 4. Atualizar um pedido
curl -X PUT http://localhost:8085/orders/1 -H "Content-Type: application/json" -d '{"clientName":"Maria Silva Souza","totalValue":310.50}'

# 5. Excluir um pedido
curl -X DELETE http://localhost:8085/orders/1
```
