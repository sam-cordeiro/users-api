# 🧩 Users API

Uma API REST desenvolvida em **TypeScript** utilizando **Express** e **Prisma**, voltada para o gerenciamento de usuários.  
A aplicação implementa operações CRUD completas, com tratamento robusto de erros e validações.

---

## 📖 Descrição Geral

A **Users API** permite cadastrar, listar, atualizar e excluir usuários, garantindo unicidade de e-mail e retornos padronizados em JSON.  
O projeto segue uma arquitetura organizada em **Controllers** e **Routes**, visando clareza e separação de responsabilidades.

---

## 🧱 Estrutura da API

O projeto segue a seguinte estrutura de diretórios:

```
src/
 ├── controllers/
 │    └── UserController.ts    # Lógica dos endpoints
 ├── routes/
 │    └── userRoutes.ts        # Definição das rotas
 ├── prisma/
 │    └── prisma.service.ts    # Conexão e manipulação com banco de dados via Prisma
 ├── index.ts                  # Ponto de entrada da aplicação
 └── server.ts                 # Inicialização do servidor Express
```

### 🧩 Organização

- **Controllers:** Contêm a lógica de negócio e interação com o Prisma.  
- **Routes:** Definem os endpoints e associam cada rota ao seu respectivo controller.

---

## 🌐 Endpoints Implementados

| Método | Rota | Descrição | Função |
|:------:|:------|:-----------|:--------|
| **POST** | `/users` | Cria um novo usuário | `createUser()` |
| **GET** | `/users` | Retorna todos os usuários cadastrados | `getUsers()` |
| **GET** | `/users/:id` | Retorna um usuário específico pelo ID | `getUserById()` |
| **PUT** | `/users/:id` | Atualiza os dados de um usuário existente | `updateUser()` |
| **DELETE** | `/users/:id` | Remove um usuário existente | `deleteUser()` |

### 📦 Exemplo de corpo JSON para `POST /users`
```json
{
  "name": "João Silva",
  "email": "joao.silva@exemplo.com"
}
```

### 📦 Exemplo de corpo JSON para `PUT /users/:id`
```json
{
  "name": "João Atualizado",
  "email": "joao.atualizado@exemplo.com"
}
```

---

## ⚙️ Comandos de Execução

### 1️⃣ Clonar o repositório
```bash
git clone https://github.com/sam-cordeiro/users-api.git
cd users-api
```

### 2️⃣ Instalar dependências
```bash
npm install
```

### 3️⃣ Executar em modo desenvolvimento
```bash
npm run dev
```

### 4️⃣ Compilar TypeScript (opcional)
```bash
npm run build
```

### 5️⃣ Iniciar servidor
```bash
npm start
```

> O servidor roda por padrão em **http://localhost:3000**.

---

## 🚨 Tratamento de Erros

A API possui tratamento de erros padronizado, retornando mensagens claras com códigos HTTP adequados.

| Código | Tipo de Erro | Situação | Exemplo de Resposta |
|:------:|:-------------|:----------|:--------------------|
| **404** | Not Found | ID não encontrado no banco de dados | `{ "error": "Usuário não encontrado" }` |
| **409** | Conflict | E-mail duplicado (violação de campo único) | `{ "error": "Email já existe" }` |
| **500** | Internal Server Error | Falha inesperada no servidor | `{ "error": "Internal server error" }` |

### 💡 Implementação Interna

Os erros são tratados com blocos `try/catch` dentro dos controllers.  
O Prisma lança erros com códigos específicos, interceptados pela API:

- `P2025` → Registro não encontrado → **404 Not Found**  
- `P2002` → Violação de campo único (e-mail) → **409 Conflict**  

Exemplo simplificado:

```ts
try {
  const user = await prisma.user.findUniqueOrThrow({ where: { id } });
  res.json(user);
} catch (error: any) {
  if (error.code === 'P2025') {
    return res.status(404).json({ error: 'Usuário não encontrado' });
  }
  res.status(500).json({ error: 'Internal server error' });
}
```

---

## 👨‍💻 Autor

**Samuel Assis Cordeiro**  
Desenvolvedor de Software • Estagiário na Direcional Engenharia  
📍 GitHub: [sam-cordeiro](https://github.com/sam-cordeiro)

---

> 💡 *A Users API é um projeto de aprendizado e prática de desenvolvimento backend em TypeScript, demonstrando boas práticas de estrutura, rotas RESTful e tratamento de erros.*
