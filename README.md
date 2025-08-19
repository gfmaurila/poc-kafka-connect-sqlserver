
# Manual de Uso — Integração SQL Server → Kafka → SQL Server com Docker e Kafka Connect

Este manual explica como utilizar o ambiente Docker configurado com Kafka, Kafka Connect e dois bancos SQL Server (origem e destino), além do Kafka UI e do Kafdrop, para realizar replicação de dados.

---

## Pré-requisitos

- Docker instalado e funcionando.
- Docker Compose instalado.
- Arquivos fornecidos no projeto:
  - `docker-compose.yml`
  - `README.md`
  - `origem.sql`
  - `destino.sql`

---

## 1. Subir o ambiente

Abra o terminal na raiz do projeto e execute:

```bash
docker-compose down -v
docker-compose up -d --build
```

Esses comandos removem containers antigos e constroem os novos com base no `docker-compose.yml`.

---

## 2. Validar os containers

Para verificar se todos os serviços estão no ar:

```bash
docker ps
```

Os principais containers esperados são:

- `kafka-poc-sqlserver-connect-1`
- `sqlserver-origem`
- `sqlserver-destino`
- `kafka`
- `zookeeper`
- `kafka-ui` ou `kafdrop` (opcional)

---

## 3. Acessar o container Kafka Connect

Entre no container do Kafka Connect para registrar os conectores:

```bash
docker exec -it kafka-poc-sqlserver-connect-1 bash
```

---

## 4. Registrar o conector de origem

Dentro do container, execute:

```bash
curl -X POST http://localhost:8083/connectors   -H "Content-Type: application/json"   -d @/path/para/origem-connector.json
```

Ou copie diretamente o JSON do conector de origem no terminal, conforme o README.md.

---

## 5. Registrar o conector de destino

Ainda dentro do container:

```bash
curl -X POST http://localhost:8083/connectors   -H "Content-Type: application/json"   -d @/path/para/destino-connector.json
```

Ou copie o JSON do README.md.

---

## 6. Inserir dados no SQL Server de origem

Você pode utilizar alguma ferramenta SQL ou o próprio terminal para inserir os dados. Exemplo:

```sql
USE Origem;
INSERT INTO TB_EXEMPLO_ORIGEM (Nome) VALUES ('Teste Kafka → SQL Server');
```

---

## 7. Validar no SQL Server de destino

Verifique se os dados chegaram corretamente no destino:

```sql
USE Destino;
SELECT * FROM TB_EXEMPLO_ORIGEM;
```

---

## 8. Visualizar com Kafka UI (opcional)

Abra no navegador:

```
http://localhost:8080
```

Você poderá ver os tópicos, conectores e mensagens trafegadas.

---

## 9. Encerrando o ambiente

Para encerrar e remover todos os volumes:

```bash
docker-compose down -v
```

---

## Observações

- A senha dos bancos é definida como `@K18a08f2025` no ambiente.
- O driver JDBC do SQL Server é mapeado no volume `/usr/share/java`.
- A pasta `init/` contém os scripts para criação das tabelas de teste.



## 📫 Como me encontrar
- [![YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/channel/UCjy19AugQHIhyE0Nv558jcQ)
- [![Linkedin Badge](https://img.shields.io/badge/-Guilherme_Figueiras_Maurila-blue?style=flat-square&logo=Linkedin&logoColor=white&link=https://www.linkedin.com/in/guilherme-maurila)](https://www.linkedin.com/in/guilherme-maurila)
- [![Gmail Badge](https://img.shields.io/badge/-gfmaurila@gmail.com-c14438?style=flat-square&logo=Gmail&logoColor=white&link=mailto:gfmaurila@gmail.com)](mailto:gfmaurila@gmail.com)
- 📧 Email: gfmaurila@gmail.com