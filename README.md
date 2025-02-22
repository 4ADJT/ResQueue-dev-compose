# 🛠️ Resqueue Development Environment

## 📖 Sobre o Projeto

Este arquivo configura um **ambiente de desenvolvimento** completo para o sistema **Resqueue** utilizando **Docker Compose**. Ele inclui todos os serviços necessários para rodar a aplicação localmente, como **Keycloak**, **Eureka Server**, **API Gateway** e o **User Service**.

O ambiente é executado com **network_mode: host**, garantindo comunicação entre os serviços sem necessidade de mapeamento de portas.

---

## ⚠️ **IMPORTANTE**

> Como todos os serviços rodam com `network_mode: host`, é necessário habilitar essa opção no **Docker Desktop** antes de iniciar os containers.

### 🛠️ **Habilitando o Network Mode: Host no Docker Desktop**

1. **Abra o Docker Desktop**
2. **Acesse as Configurações** clicando no ícone de engrenagem no canto superior direito
3. **Navegue até `Settings` > `Resources` > `Network`**
4. **Habilite a opção** `Enable host networking`
5. **Salve as configurações e reinicie o Docker Desktop**

---

## 🚀 **Serviços Configurados**

### 🏢 **Keycloak**

- **Imagem:** `quay.io/keycloak/keycloak:26.1`
- **Porta:** `9000`
- **Admin:** `admin/admin`
- **Comando:** `start-dev --import-realm`

### 🌍 **Eureka Server**

- **Imagem:** `rodrigobrocchi/resqueue-server:latest`
- **Porta:** `8761`
- **Health Check:** `http://localhost:8761/actuator/health`

### 🔀 **API Gateway**

- **Imagem:** `rodrigobrocchi/resqueue-gateway:latest`
- **Porta:** `8080`
- **Variáveis de Ambiente:**
  - `KC_BASE_ISSUER_URL=http://localhost:9000`
  - `EUREKA_URL=http://localhost:8761/eureka`

### 👤 **User Service**

- **Imagem:** `rodrigobrocchi/resqueue-user:latest`
- **Porta:** `9001`
- **Variáveis de Ambiente:**
  - `KC_BASE_ISSUER_URL=http://localhost:9000`
  - `EUREKA_URL=http://localhost:8761/eureka`
  - `AUTH_RESQUEUE_CLIENT_SECRET=0eaQY7UIJ7PSc482osmIXMsO8RbPRksT`
  - `AUTH_BASE_URL=http://localhost:9000`

---

## 📦 **Rodando o Ambiente**

Para iniciar todos os serviços, execute:

```sh
docker-compose up -d
```

Para verificar os logs de um serviço específico:

```sh
docker logs -f keycloak_dev
```

Para parar o ambiente:

```sh
docker-compose down
```

---

## 🖥️ **Acessando os Serviços**

| Serviço      | URL                     |
| ------------ | ----------------------- |
| **Keycloak** | `http://localhost:9000` |
| **Eureka**   | `http://localhost:8761` |
| **Gateway**  | `http://localhost:8080` |
| **User API** | `http://localhost:9001` |

---

## 🏗️ **Dependências entre Serviços**

- O **Keycloak** precisa estar rodando antes de qualquer outro serviço.
- O **Eureka Server** precisa estar pronto antes do **Gateway** e dos **serviços de negócio**.
- O **User Service** depende do **Eureka Server** e do **Gateway**.

---

## ResQueue-dev-compose no Postman (Exemplo)

Collection do Postman https://www.postman.com/imaginer-postman/workspace/resqueue
