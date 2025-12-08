# 🧪 Testes Automatizados de API — Postman + Newman + GitHub Actions

Este projeto contém um conjunto de testes automatizados desenvolvidos no **Postman** com execução via **Newman** e integração contínua utilizando **GitHub Actions**.

A API utilizada nos testes é a **Serverest**:  
https://serverest.dev

---

# 📌 Objetivo

Garantir **100% de cobertura** dos endpoints relacionados a **usuários (/usuarios)**, incluindo:

- Criação de usuário  
- Leitura de todos os usuários  
- Leitura por ID  
- Atualização  
- Exclusão  
- Cenários negativos  
- Fluxo completo de “caminho feliz”

Os testes são organizados em duas baterias:

1. **Fluxo principal (Happy Path)**  
2. **Validações negativas (erros esperados)**  

---

# 📂 Estrutura do Projeto
testes-api-postman/
│
├── postman/
│ ├── serverest_environment.json
│ └── usuarios_collection.json
│
├── .github/
│ └── workflows/
│ └── api-tests.yml
│
├── report.html # relatório gerado pelo Newman (execução local)
├── package.json # dependências do Newman
└── README.md


---

# 🧰 Ferramentas / Tecnologias

- **Postman**
- **Newman**
- **Node.js**
- **GitHub Actions**
- **Serverest.dev API**

---

# 🚀 Como executar os testes localmente

## 1. Instalar Node.js (se ainda não tiver)
https://nodejs.org/en/

Verifique:
node -v
npm -v


---

## 2. Instalar Newman globalmente
npm install -g newman

---

## 3. Instalar dependências do projeto
(Somente se estiver usando package.json)

npm install

---

## 4. Executar a coleção de testes com o Newman
newman run postman/usuarios_collection.json
-e postman/serverest_environment.json
-r cli,html --reporter-html-export report.html

Após a execução, abra o arquivo:
report.html

---

# 🤖 Execução automática no GitHub Actions

Este projeto possui uma pipeline configurada no arquivo:
.github/workflows/api-tests.yml

A pipeline roda automaticamente em:

- **push**
- **pull_request**

E executa:

- Instalação do Node  
- Instalação do Newman  
- Execução dos testes  
- Geração de relatório  
- Upload do relatório como artefato da pipeline  

O relatório fica disponível no GitHub:  
**Actions → Última execução → Artifacts → report.html**

---

# 🧪 Casos de Teste Implementados

### ✔️ **Caminho feliz**
1. Criar um novo usuário  
2. Buscar todos os usuários  
3. Buscar usuário criado por ID  
4. Atualizar usuário por ID  
5. Excluir usuário  
6. Validar que o usuário excluído não existe mais  

---

### ❌ **Cenários negativos**
1. Criar usuário sem campo obrigatório  
2. Criar usuário com email duplicado  
3. Buscar usuário com ID inválido  
4. Atualizar usuário inexistente  
5. Excluir usuário inexistente  
6. Enviar token inválido  
7. Tentar acesso sem token  

---

# 🔐 Autenticação

A API exige **token JWT**, mas o Serverest permite simular login para obter o token.

O ambiente Postman (`serverest_environment.json`) já contém:

- URL base
- Token dinâmico via script de pré-requisição

---

# 💡 Como importar o projeto no Postman

1. Abra o Postman  
2. Vá em **File → Import**  
3. Importe:
   - `postman/usuarios_collection.json`
   - `postman/serverest_environment.json`

4. Execute a coleção normalmente pelo botão **Run**

---

# 📄 Relatório HTML

Ao rodar localmente, o relatório é salvo em:

report.html


Na pipeline do GitHub, ele aparece como **artifact** após cada execução.

---

# 📫 Contato

Projeto desenvolvido por **Nicolas Seabra**  
QA Analyst | Testes de API | Postman | Automação  
LinkedIn: *adicione o link aqui*

Se tiver dúvidas ou quiser melhorar este projeto, abra uma **issue**!

---

# 🏁 Conclusão

Este projeto demonstra:

- Testes automatizados completos com Postman  
- Execução automatizada via Newman  
- CI no GitHub Actions  
- Cobertura total dos endpoints de /usuarios  
- Testes organizados em fluxo feliz + cenários negativos  





