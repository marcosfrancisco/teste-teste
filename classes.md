```mermaid
classDiagram
direction LR

class EstoqueBoundary {
  <<Boundary>>
  +listar(): List~Produto~
  +criarProduto(dto:ProdutoDTO): Produto
  +repor(id:int, qtd:int): void
  +baixar(id:int, qtd:int): void
  +detalhar(id:int): Produto
}

class EstoqueControl {
  <<Control>>
  -repo: ProdutoRepository
  +listar(): List~Produto~
  +cadastrar(Produto): Produto
  +repor(id:int, qtd:int): void
  +baixar(id:int, qtd:int): void
  +buscar(id:int): Produto
}

class Produto {
  <<Entity>>
  -id:int
  -nome:String
  -preco:double
  -quantidadeEstoque:int
  +repor(q:int): void
  +baixar(q:int): void
  +getQuantidadeEstoque(): int
}
class Instrumento { <<Entity>> -tipo:String }
class Corda { <<Entity>> -numeroCordas:int -material:String }
class Metal { <<Entity>> -afinacaoPadrao:String }
class Percussao { <<Entity>> -tipoPele:String }
class Acessorio { <<Entity>> -categoria:String }

Produto <|-- Instrumento
Instrumento <|-- Corda
Instrumento <|-- Metal
Instrumento <|-- Percussao
Produto <|-- Acessorio

class ProdutoRepository {
  <<Persistence>>
  +save(Produto): Produto
  +findById(id:int): Produto
  +findAll(): List~Produto~
  +updateEstoque(id:int, novo:int): void
}

EstoqueBoundary ..> EstoqueControl
EstoqueControl ..> ProdutoRepository
