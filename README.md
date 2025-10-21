# Projeto Sprint 4: Operating Systems – Servidor Linux, Docker, SQLite e Anonimização de Dados

## Integrantes do Grupo
1. Bernardo Pinto Rocha - RM99209  
2. Gabriel Diegues - RM550788  
3. Luiza Cristina - RM99367  
4. Pedro Palladino - RM551180  
5. Renato Izumi - RM99242  

---

## 📽️ LINK DO VÍDEO
[Link para o vídeo no YouTube](https://youtu.be/TPuR4ZEh-Eo)

##  LINK DO Github
[Link para o Github](https://github.com/BernardoPRocha/SPRINT4-SO.git)

## Descrição do Projeto

Este projeto demonstra a implementação de um ambiente Linux para gestão de dados de usuários, utilizando:

- Docker e NGINX no servidor Linux (Debian/Ubuntu Server, sem GUI)
- Node.js para endpoints de inserção de usuários e logs
- SQLite em contêiner Docker com volume compartilhado
- Shell Script para anonimização de dados sensíveis (PII)
- Gerenciamento de logs de acesso e operação  

O objetivo é garantir **segurança, rastreabilidade e anonimização de dados**, mostrando boas práticas de administração de sistemas.

---

## Estrutura de Diretórios do Projeto

```
operating_systems_project/
├── node_app/             # Código do servidor Node.js
│   ├── index.js
│   ├── package.json
│   └── node_modules/
├── srv/xp_db/            # Volume compartilhado para o banco SQLite
│   └── xp_users.db
└── var/log/xp/           # Logs de acesso e anonimização
    └── xp_access.log
```

## 🔐 Login 
|   Usuário   |  Senha  |
|-------------|---------|
|   student   | student |
| logger_user | logger  |
|  anon_user  |  anon   |

---

## 🧾 Endpoints

Ação	               | Comando	                          |                                                                                                                                                                                            Descrição

➕ Criar usuário	    |     ```   bash curl -X POST http://localhost:3000/users -H "Content-Type: application/json" -d '{"full_name":"Bernardo Rocha","cpf":"12345678900","email":"rm99209@fiap.com.br"}'	```|   Insere um novo usuário no banco SQLite
 
🪵 Registrar log	    |     ``` bash curl -X POST http://localhost:3000/logs -H "Content-Type: application/json" -d '{"level":"INFO","message":"Teste de log via endpoint"} '```	    |              Grava logs de operação e acesso no diretório /var/log/xp

## 🧍 Usuários do Sistema

Usuário	            Função
student	      -     Usuário administrador do projeto
logger_user	  -    Escreve logs no diretório /var/log/xp
anon_user	  -    Executa o script de anonimização de dados

Permissões

```bash
    sudo chown logger_user:logger_user /var/log/xp
    sudo chmod 750 /var/log/xp
    sudo usermod -aG logger_user anon_user
```

🧹 Script de Anonimização
- Local: /usr/local/bin/anonimize_logs.sh
- Executado por:

```bash
'su - anon_user
/usr/local/bin/anonimize_logs.sh'
```

Funções do Script:

Substitui CPFs, e-mails e nomes nos logs por valores anonimizados

Garante a integridade do arquivo /var/log/xp/xp_access.log

## Docker e SQLite
Subir contêiner SQLite com volume externo:

```bash
docker run -d \
  --name sqlite-container \
  -v /srv/xp_db:/data \
  nouchka/sqlite3
```
O banco SQLite (xp_users.db) é compartilhado entre contêiner e host Linux.

## Node.js
Iniciar servidor Node.js:

```bash
cd node_app
npm install
node index.js
```

Endpoints já configurados para inserção de usuários e logs.

## 🧪 Testes Realizados
✅ Inserção de usuário via endpoint

✅ Inserção de log no banco SQLite

✅ Geração de logs em /var/log/xp/xp_access.log

✅ Execução do script de anonimização

✅ Teste de permissões e acessos restritos

--- 

# Observações Finais

- ## Todos os dados sensíveis são anonimizados automaticamente pelo cronjob.

- ## Os logs são gravados de forma organizada, garantindo segurança e rastreabilidade.

- ## O projeto demonstra integração entre Linux, Docker, Node.js e SQLite, com práticas de segurança para PII.


# Curso: Operating Systems

