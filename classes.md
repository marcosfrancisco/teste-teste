```mermaid
classDiagram
direction LR

%% =========================
%%  BOUNDARY (REST / UI)
%% =========================
class ProdutoBoundary <<Boundary>> {
  +listar(): List<Produto>
  +buscar(id:int): Produto
  +criar(dto:ProdutoDTO): Produto
  +repor(id:int, qtd:int): void
  +vender(id:int, qtd:int): void
}
class VendaBoundary <<Boundary>> {
  +listar(): List<Venda>
  +criar(dto:VendaDTO): Venda
  +detalhar(id:int): Venda
}
class UsuarioBoundary <<Boundary>> {
  +listar(): List<Usuario>
  +criar(dto:UsuarioDTO): Usuario
}

%% =========================
%%  CONTROL (USE CASE)
%% =========================
class ProdutoControl <<Control>> {
  -repo: ProdutoRepository
  +listarProdutos(): List<Produto>
  +buscarProduto(id:int): Produto
  +cadastrarProduto(p:Produto): Produto
  +reporEstoque(id:int, qtd:int): void
  +venderProduto(id:int, qtd:int): void
}
class VendaControl <<Control>> {
  -produtoRepo: ProdutoRepository
  -vendaRepo: VendaRepository
  -usuarioRepo: UsuarioRepository
  +criarVenda(clienteId:int, itens: List<ItemVenda>): Venda
  +listarVendas(): List<Venda>
  +detalharVenda(id:int): Venda
}
class UsuarioControl <<Control>> {
  -repo: UsuarioRepository
  +listarUsuarios(): List<Usuario>
  +cadastrarUsuario(u:Usuario): Usuario
}

%% =========================
%%  ENTITY (DOMÍNIO)
%% =========================
class Produto <<Entity>> {
  -id:int
  -nome:String
  -preco:double
  -quantidadeEstoque:int
  +vender(qtd:int): void
  +repor(qtd:int): void
  +getQuantidadeEstoque(): int
}
class Instrumento <<Entity>> {
  -tipo:String  "corda|percussao|metal"
}
class Acessorio <<Entity>> {
  -categoria:String  "palheta|cabo|estojo|..."
}
class Usuario <<Entity>> {
  -id:int
  -nome:String
  -email:String
}
class Cliente <<Entity>> {
  -telefone:String
}
class Funcionario <<Entity>> {
  -cargo:String
  +registrarVenda(v:Venda): void
}
class Venda <<Entity>> {
  -id:int
  -data:Date
  -total:double
  +calcularTotal(): double
}
class ItemVenda <<Entity>> {
  -id:int
  -quantidade:int
  -precoUnit:double
  +subtotal(): double
}

%% =========================
%%  REPOSITORY (PERSISTÊNCIA)
%% =========================
class ProdutoRepository <<Repository>> {
  +save(p:Produto): Produto
  +update(p:Produto): void
  +findById(id:int): Produto
  +findAll(): List<Produto>
  +delete(id:int): void
}
class UsuarioRepository <<Repository>> {
  +save(u:Usuario): Usuario
  +findById(id:int): Usuario
  +findAll(): List<Usuario>
}
class VendaRepository <<Repository>> {
  +save(v:Venda): Venda
  +findById(id:int): Venda
  +findAll(): List<Venda>
}

%% =========================
%%  DTOs (fronteira web)
%% =========================
class ProdutoDTO {
  +nome:String
  +preco:double
  +quantidadeEstoque:int
  +tipoOuCategoria:String
}
class ItemVendaDTO {
  +produtoId:int
  +quantidade:int
}
class VendaDTO {
  +clienteId:int
  +itens: List<ItemVendaDTO>
}
class UsuarioDTO {
  +nome:String
  +email:String
  +telefone:String
  +tipo:String  "cliente|funcionario"
  +cargo:String
}

%% ====== HERANÇA (Entity)
Instrumento --|> Produto
Acessorio --|> Produto
Cliente --|> Usuario
Funcionario --|> Usuario

%% ====== AGREGAÇÕES/COMPOSIÇÕES
Venda "1" *-- "1..*" ItemVenda : compõe
ItemVenda "1" --> "1" Produto : referencia
Venda "1" --> "1" Cliente : "cliente"
Venda "0..*" --> "0..1" Funcionario : "registradaPor"

%% ====== DEPENDÊNCIAS Boundary -> Control
ProdutoBoundary ..> ProdutoControl
VendaBoundary ..> VendaControl
UsuarioBoundary ..> UsuarioControl

%% ====== DEPENDÊNCIAS Control -> Repository
ProdutoControl ..> ProdutoRepository
VendaControl ..> ProdutoRepository
VendaControl ..> VendaRepository
VendaControl ..> UsuarioRepository
UsuarioControl ..> UsuarioRepository

%% ====== Repository -> Entity (retornos/param)
ProdutoRepository ..> Produto
UsuarioRepository ..> Usuario
VendaRepository ..> Venda

%% ====== Boundary usa DTOs
ProdutoBoundary ..> ProdutoDTO
VendaBoundary ..> VendaDTO
VendaDTO ..> ItemVendaDTO
UsuarioBoundary ..> UsuarioDTO
