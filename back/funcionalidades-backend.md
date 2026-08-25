# Funcionalidades do Back-End

Este documento descreve as rotas ("endpoints") que o Back-End precisa implementar e as
regras de negócio aplicadas a cada uma delas, com base nos requisitos levantados com o
cliente.

---

## Rota: `/cadastrarCliente`

**Método:** POST
**Recebe:** Nome, CPF, Data_Nascimento, Email, Telefone (enviados pelo Front-End)

**Regras de Negócio:**
1. O Back-End valida se o CPF já existe no Banco de Dados.
   - Se já existir → retorna erro `409 Conflict` ("CPF já cadastrado").
2. O Back-End calcula a idade a partir da Data_Nascimento e faz um `IF` verificando se é
   maior ou igual a 18 anos.
   - Se for **menor** de 18 anos → retorna erro `403 Forbidden` ("Cadastro não permitido
     para menores de idade").
   - Se **passar** na validação → salva o registro na tabela `Clientes` do Banco de Dados.
3. Em caso de sucesso, retorna `201 Created` com os dados do cliente cadastrado.

---

## Rota: `/listarClientes`

**Método:** GET
**Recebe:** (nenhum parâmetro obrigatório; pode receber filtros opcionais, ex: nome)

**Regras de Negócio:**
1. Retorna a lista de todos os clientes cadastrados no Banco de Dados.
2. Campos sensíveis (como CPF completo) podem ser parcialmente ocultados na resposta
   por segurança, dependendo da política definida com o cliente.

---

## Rota: `/cadastrarProduto`

**Método:** POST
**Recebe:** Nome_Produto, Categoria, Preco, Estoque

**Regras de Negócio:**
1. O Back-End verifica se o Preco é maior que zero.
   - Se **Preco <= 0** → retorna erro `400 Bad Request` ("Preço inválido").
2. O Back-End verifica se o Estoque é um número inteiro maior ou igual a zero.
   - Se inválido → retorna erro `400 Bad Request` ("Estoque inválido").
3. Se todas as validações passarem, salva o produto na tabela `Produtos` do Banco de Dados.
4. Em caso de sucesso, retorna `201 Created` com os dados do produto cadastrado.

---

## Rota: `/listarProdutos`

**Método:** GET
**Recebe:** (nenhum parâmetro obrigatório; pode receber filtros opcionais, ex: categoria)

**Regras de Negócio:**
1. Retorna a lista de todos os produtos cadastrados, incluindo nome, categoria, preço e
   estoque disponível.
2. Produtos com Estoque = 0 podem ser sinalizados como "Indisponível" na resposta.

---

## Rota: `/atualizarEstoque`

**Método:** PUT
**Recebe:** ID do produto, quantidade a subtrair (ex: após uma venda)

**Regras de Negócio:**
1. O Back-End verifica se o produto existe.
   - Se não existir → retorna erro `404 Not Found`.
2. O Back-End verifica se há estoque suficiente para a operação.
   - Se **quantidade solicitada > estoque disponível** → retorna erro `400 Bad Request`
     ("Estoque insuficiente").
3. Se passar nas validações, atualiza o campo Estoque no Banco de Dados e retorna
   `200 OK`.

---

## Resumo das Regras de Negócio (visão geral)

| Rota | Regra principal | Erro em caso de falha |
|---|---|---|
| /cadastrarCliente | Idade >= 18 anos | 403 Forbidden |
| /cadastrarCliente | CPF único | 409 Conflict |
| /cadastrarProduto | Preço > 0 | 400 Bad Request |
| /cadastrarProduto | Estoque >= 0 | 400 Bad Request |
| /atualizarEstoque | Estoque suficiente | 400 Bad Request |
| /atualizarEstoque | Produto existente | 404 Not Found |
