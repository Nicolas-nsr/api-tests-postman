– Testes Automatizados da API Serverest (/usuarios)
📌 Descrição do Projeto

Este projeto contém uma suíte de testes automatizados desenvolvida no Postman e executável via Newman e GitHub Actions, com o objetivo de validar os endpoints relacionados ao recurso /usuarios da API pública Serverest (https://serverest.dev/).

Os testes cobrem:

Criação de usuários

Listagem

Consulta por ID

Atualização

Exclusão

Validações negativas e cenários de erro

Autenticação JWT

Execução encadeada (cada requisição alimenta a próxima)

O conjunto garante 100% de cobertura funcional dos endpoints /usuarios.

📂 Estrutura do Projeto
├─ postman/
│   ├─ usuarios_collection.json
│   └─ serverest_environment.json
│
├─ .github/
│   └─ workflows/
│       └─ postman-newman.yml
│
├─ reports/  (gerado pelo Newman)
│   └─ newman-report.html
│
└─ README.md

🛠️ Requisitos
Para rodar localmente

Node.js 16+

NPM

Postman (opcional)

Newman

Reporter HTML Extra

Para CI/CD

Repositório GitHub

🚀 Como executar os testes
✔️ 1. Rodar usando o Postman (Collection Runner)

Importe os arquivos:

usuarios_collection.json

serverest_environment.json

Selecione o Environment Serverest Tests

Vá até a Collection → Run Collection

Execute

A execução já está configurada de forma encadeada:

Login → cria token

Criação de usuário → salva id

Atualizar → usa o id

Buscar → usa o id

Excluir → usa o id

✔️ 2. Rodar via Newman (CLI)
Instale o Newman:
npm install -g newman newman-reporter-htmlextra

Execute:
newman run postman/usuarios_collection.json \
  -e postman/serverest_environment.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/newman-report.html


O relatório será gerado em:

reports/newman-report.html

✔️ 3. Execução automática via GitHub Actions

A pipeline está no arquivo:

.github/workflows/postman-newman.yml


Ela faz:

Instala Node

Instala Newman

Executa a collection

Gera relatório

Publica como artefato

Basta realizar push para main, e os testes são executados automaticamente.

🧪 Testes Implementados

A suíte é dividida em:

Caminho feliz (cenários positivos)

Validações negativas (erros controlados)

Todos os testes estão organizados em ordem lógica para permitir execução automática.

✅ Cenários POSITIVOS – (Caminho Feliz)
1. Login – Obter Token JWT

Envia credenciais válidas

Valida:

status 200

campo authorization

Armazena o token em:

authToken

2. Criar usuário (POST /usuarios)

Envia corpo JSON válido:

{
  "nome": "Usuário Teste",
  "email": "teste{{timestamp}}@qa.com",
  "password": "123456",
  "administrador": "true"
}


Valida:

status 201

Mensagem Cadastro realizado com sucesso

Armazena:

testUserId

testUserEmail

3. Listar usuários (GET /usuarios)

Valida:

status 200

Estrutura da lista

Tempo de resposta aceitável

4. Buscar usuário por ID (GET /usuarios/{{testUserId}})

Valida:

status 200

Nome/email iguais aos dados enviados

Response contém o ID salvo

5. Atualizar usuário (PUT /usuarios/{{testUserId}})

Envia JSON atualizando nome/email.

Valida:

status 200

Mensagem "Registro alterado com sucesso"

6. Deletar usuário (DELETE /usuarios/{{testUserId}})

Valida:

status 200

Mensagem "Registro excluído com sucesso"

❌ Cenários NEGATIVOS
1. Criar usuário com email já existente

Valida:

status 400

Mensagem "Este email já está sendo usado"

2. Criar usuário com campos faltando

Corpo sem e-mail ou sem senha

Valida:

status 400

Mensagens de validação

3. Buscar usuário inexistente

Valida:

status 400 ou 404

Mensagem "Usuário não encontrado"

4. Atualizar com ID inválido

Valida:

status 400

5. Acessar endpoints sem token (quando aplicável)

Headers sem Authorization

Valida:

status 401 ou 403

🔗 Execução Encadeada (Automática)

As variáveis de ambiente permitem que cada requisição dependa da anterior, sem intervenção manual.

Exemplos de variáveis preenchidas automaticamente:

Token:
pm.environment.set("authToken", json.authorization);

ID do usuário:
pm.environment.set("testUserId", json._id);

Email de teste:
pm.environment.set("testUserEmail", json.email);


Isso faz com que:

O teste de "Buscar por ID" só rode após a criação

O "Atualizar" use o ID válido

O "Deletar" elimine o usuário correto

📊 Relatórios

Ao rodar via Newman, o relatório HTML é gerado automaticamente em:

reports/newman-report.html


Exibe:

Resultados individuais por request

Tempo de execução

Logs

Assertions

Erros

Tabela geral da suíte

🤝 Contribuição

Pull requests são bem-vindos.

📄 Licença

MIT License.
