## User and Email Microservice

Este projeto consiste em dois microserviços: `user-service` e `email-service`. O `user-service` lida com operações relacionadas a usuários, enquanto o `email-service` gerencia o envio de emails. Esses serviços se comunicam de maneira assíncrona via RabbitMQ.

## Índice

- [Funcionalidades](#funcionalidades)
  - [user-service](#user-service)
  - [email-service](#email-service)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Licença](#licença)

## Funcionalidades
<img src="./screenshot_email .png" alt="example img" style="width: 400px; height: auto; max-width: 100%;">

### user-service

- Responsável por:

    1. Criar Usuários:
        - Endpoint `POST /users`: Recebe os dados de um novo usuário no formato JSON e valida essas informações.
        - O serviço converte os dados recebidos em um objeto de usuário (UserModel) e salva no banco de dados PostgreSQL.
    
    2. Publicação de Mensagens:
        - Utiliza RabbitMQ para enviar mensagens com os detalhes do usuário recém-criado.
        - A mensagem contém informações como ID do usuário, email e uma mensagem de boas-vindas.

- `POST /users`
~~~json
{
	"name": "test",
	"email": "usertest@test.com"
}
~~~

| user_id                              | email             | name      |
|--------------------------------------|-------------------|-----------|
| 425a2251-0206-4f29-9389-f5abd108449b | usertest@test.com | User Test |

- Exemplo da Tabela no Banco de Dados (TB_USERS)

### email-service

- Responsável por:

    1. Consumo de Mensagens:
        - Escuta a fila do RabbitMQ para mensagens enviadas pelo user-service.
        - Quando uma mensagem é recebida, o serviço processa os dados do usuário.
    
    2. Envio de Emails:
        - Utiliza o serviço de email (configurado com Mailtrap) para enviar um email de boas-vindas ao novo usuário.
        - Armazena detalhes do email enviado no banco de dados PostgreSQL, incluindo status de envio e data.


| email_id | email_from | email_t  | send_date_email | status_email |subject |text | user_id  |
|----------|------------|----------|-----------------|--------------|--------|-----|----------|
| 9fe4c62a-d5f3-4953-9f3d-1c1e619ef7aa  | b82be6472ccd50 | usertest@test.com  | 2024-07-16 17:04:23.460796 | SENT | Cadastro realizado com sucesso! | User Test, seja bem vindo(a)! Agradecemos o seu cadastro, aproveite agora todos os recursos da nossa plataforma! | 425a2251-0206-4f29-9389-f5abd108449b |

- Exemplo da Tabela no Banco de Dados (TB_EMAILS)

## Tecnologias Utilizadas

- **Java 21**
- **Spring Boot**
- **RabbitMQ**
- **Docker**
- **PostgreSQL**

## Licença

Este projeto está licenciado sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.
