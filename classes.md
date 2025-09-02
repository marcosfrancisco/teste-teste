```mermaid
classDiagram
direction LR

%% ===== BOUNDARY (mínimo)
class EstoqueBoundary {
  <<Boundary>>
  +listarProdutos(): List~Produto~
  +criarProduto(nome:String, preco:double, qtd:int): Produto
  +repor(id:int, qtd:int): void
  +vender(id:int, qtd:int): void
  +criarVenda(clienteId:int, itens: List~ItemVenda~): Venda
}

%% ===== CONTROL (mínimo)
class EstoqueControl {
  <<Control>>
  +listarProdutos(): List~Produto~
  +cadastrarProduto(p:Produto): Produto
  +reporEstoque(id:int, qtd:int): void
  +venderProduto(id:int, qtd:int): void
  +registrarVenda(clienteId:int, itens: List~ItemVenda~): Venda
}

%% ===== ENTIDADES (essenciais)
class Produto {
  <<Entity>>
  -id:int
  -nome:String
  -preco:double
  -quantidadeEstoque:int
  +vender(q:int): void
  +repor(q:int): void
  +getQuantidadeEstoque(): int
}

class Instrumento {
  <<Entity>>
  -tipo:String  "corda|percussao|metal"
}
class Acessorio {
  <<Entity>>
  -categoria:String  "palheta|cabo|estojo|..."
}
Produto <|-- Instrumento
Produto <|-- Acessorio

class Usuario {
  <<Entity>>
  -id:int
  -nome:String
  -email:String
}
class Cliente {
  <<Entity>>
  -telefone:String
}
class Funcionario {
  <<Entity>>
  -cargo:String
}
Usuario <|-- Cliente
Usuario <|-- Funcionario

class Venda {
  <<Entity>>
  -id:int
  -data:Date
  -total:double
  +calcularTotal(): double
}
class ItemVenda {
  <<Entity>>
  -produtoId:int
  -quantidade:int
  -precoUnit:double
  +subtotal(): double
}

%% ===== RELAÇÕES ENTRE ENTIDADES
Venda "1" *-- "1..*" ItemVenda : compõe
ItemVenda --> Produto : referencia
Venda --> Cliente : cliente

%% ===== FLUXO BÁSICO
EstoqueBoundary ..> EstoqueControl
EstoqueControl ..> Produto
EstoqueControl ..> Venda
EstoqueControl ..> ItemVenda
EstoqueControl ..> Cliente
