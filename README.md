# DSList - API de Lista de Jogos 🎮

[![Java](https://img.shields.io/badge/Java-21-orange)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.4-brightgreen)](https://spring.io/projects/spring-boot)

Uma API RESTful desenvolvida com Spring Boot para gerenciar listas de jogos. Este projeto foi criado durante o **Intensivão Java Spring** da DevSuperior, demonstrando conceitos fundamentais do desenvolvimento backend com Spring Boot.

## 📋 Sobre o Projeto

O DSList é uma aplicação que permite:

- Listar jogos com informações detalhadas
- Organizar jogos em listas categorizadas
- Reordenar jogos dentro das listas
- Consultar detalhes específicos de cada jogo

## 🛠️ Tecnologias Utilizadas

### Backend

- **Java 21** - Linguagem de programação
- **Spring Boot 3.5.4** - Framework principal
- **Spring Data JPA** - Persistência de dados
- **Spring Web** - Desenvolvimento de APIs REST

### Banco de Dados

- **H2 Database** - Banco em memória para desenvolvimento e testes
- **PostgreSQL** - Banco de dados para produção

### Ferramentas

- **Maven** - Gerenciamento de dependências
- **Git** - Controle de versão

## 🏗️ Arquitetura do Projeto

O projeto segue uma arquitetura em camadas bem definida:

```
src/main/java/com/devsuperior/dslist/
├── config/          # Configurações (CORS, etc.)
├── controllers/     # Controladores REST
├── dto/            # Data Transfer Objects
├── entities/       # Entidades JPA
├── projections/    # Interfaces de projeção
├── repositories/   # Repositórios de dados
└── services/       # Lógica de negócio
```

## 🎯 Funcionalidades

### 🎮 Jogos

- **GET** `/games` - Lista todos os jogos (resumo)
- **GET** `/games/{id}` - Busca jogo por ID (detalhes completos)

### 📋 Listas de Jogos

- **GET** `/lists` - Lista todas as listas de jogos
- **GET** `/lists/{listId}/games` - Jogos de uma lista específica
- **POST** `/lists/{listId}/replacement` - Reordena jogos na lista

## 💾 Modelo de Dados

### Entidades Principais

#### Game (Jogo)

- `id` - Identificador único
- `title` - Título do jogo
- `year` - Ano de lançamento
- `genre` - Gênero
- `platforms` - Plataformas disponíveis
- `score` - Pontuação
- `imgUrl` - URL da imagem
- `shortDescription` - Descrição resumida
- `longDescription` - Descrição detalhada

#### GameList (Lista de Jogos)

- `id` - Identificador único
- `name` - Nome da lista

#### Belonging (Pertencimento)

- Entidade de associação entre Game e GameList
- `position` - Posição do jogo na lista
- Chave composta: `game_id` + `list_id`

## 🚀 Como Executar

### Pré-requisitos

- Java 21 ou superior
- Maven 3.6+
- PostgreSQL (para produção)

### Executando Localmente

1. **Clone o repositório**

```bash
git clone <url-do-repositorio>
cd dslist
```

2. **Execute com Maven**

```bash
./mvnw spring-boot:run
```

3. **Acesse a aplicação**

- API: `http://localhost:8080`
- H2 Console: `http://localhost:8080/h2-console`

### Perfis de Execução

#### Desenvolvimento (dev)

- Banco PostgreSQL local
- Configuração em `application-dev.properties`

#### Teste (test)

- Banco H2 em memória
- Console H2 habilitado
- SQL formatado no log

#### Produção (prod)

- Banco PostgreSQL via variáveis de ambiente
- Configuração em `application-prod.properties`

## 🔧 Configuração

### Variáveis de Ambiente (Produção)

```properties
DB_URL=jdbc:postgresql://localhost:5432/dslist
DB_USERNAME=seu_usuario
DB_PASSWORD=sua_senha
CORS_ORIGINS=http://localhost:3000,https://seudominio.com
APP_PROFILE=prod
```

### Banco de Dados

O projeto inclui um script `import.sql` com dados de exemplo:

- 2 listas: "Aventura e RPG" e "Jogos de plataforma"
- 10 jogos populares com informações completas
- Associações entre jogos e listas

## 📡 Exemplos de API

### Listar todos os jogos

```bash
GET /games
```

### Buscar jogo específico

```bash
GET /games/1
```

### Listar todas as listas

```bash
GET /lists
```

### Jogos de uma lista

```bash
GET /lists/1/games
```

### Reordenar jogos na lista

```bash
POST /lists/1/replacement
Content-Type: application/json

{
  "sourceIndex": 3,
  "destinationIndex": 1
}
```

## 🌐 CORS

O projeto está configurado para aceitar requisições de:

- `http://localhost:3000` (React)
- `http://localhost:5173` (Vite)

Para produção, configure a variável `CORS_ORIGINS`.

## 🏷️ Padrões Utilizados

- **DTO Pattern** - Transferência de dados entre camadas
- **Repository Pattern** - Abstração do acesso a dados
- **Service Layer** - Lógica de negócio centralizada
- **Projection Pattern** - Consultas otimizadas com JPA
- **RESTful API** - Arquitetura de serviços web

## 📝 Estrutura do Banco

```sql
-- Tabela de jogos
CREATE TABLE tb_game (
    id BIGSERIAL PRIMARY KEY,
    title VARCHAR(255),
    game_year INTEGER,
    genre VARCHAR(255),
    platforms VARCHAR(255),
    score DOUBLE PRECISION,
    img_url VARCHAR(255),
    short_description TEXT,
    long_description TEXT
);

-- Tabela de listas
CREATE TABLE tb_game_list (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(255)
);

-- Tabela de associação (muitos-para-muitos com posição)
CREATE TABLE tb_belonging (
    list_id BIGINT REFERENCES tb_game_list(id),
    game_id BIGINT REFERENCES tb_game(id),
    position INTEGER,
    PRIMARY KEY (list_id, game_id)
);
```

## 🤝 Contribuição

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 👨‍💻 Autor

Projeto desenvolvido durante o **Intensivão Java Spring** da [DevSuperior](https://devsuperior.com.br/).

---

⭐ Se este projeto te ajudou, considere dar uma estrela!
