# Funcionalidades do Back-End - Petshop

Aqui estão as rotas que o back-end precisa ter e as regras de negócio de cada uma.

Rota: /cadastrarCliente
-> Recebe os dados do cliente vindos do front-end (nome, CPF, data de nascimento, email, telefone).
Regra de Negócio: o back-end faz um IF verificando se a idade é maior ou igual a 18 anos.
Se for menor, retorna erro 403.
Se passar, salva o cliente no banco de dados.

Rota: /cadastrarPet
-> Recebe os dados do pet (nome, espécie, raça, data de nascimento) e o ID do cliente dono do pet.
Regra de Negócio: verifica se esse cliente já existe no banco. Se não existir, retorna erro 404.
Se existir, salva o pet vinculado ao cliente.

Rota: /cadastrarProduto
-> Recebe nome, categoria, preço e estoque do produto.
Regra de Negócio: se o preço for menor ou igual a 0, retorna erro 400.
Se estiver tudo certo, salva o produto no banco.

Rota: /agendarServico
-> Recebe o pet, o serviço escolhido (banho, tosa, etc), a data e o horário.
Regra de Negócio: o back-end verifica se já existe outro agendamento no mesmo dia e horário.
Se já tiver, retorna erro (horário ocupado).
Se estiver livre, salva o agendamento com status "Pendente".

Rota: /atualizarEstoque
-> Usada depois que um produto é vendido.
Regra de Negócio: verifica se tem estoque suficiente para a quantidade vendida.
Se não tiver, retorna erro. Se tiver, desconta a quantidade do estoque.
