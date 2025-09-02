```mermaid

flowchart LR

  user[Usuario (HTML/JS)]
  boundary[Boundary: /api/estoque]
  control[Control: EstoqueControl]
  repo[(SQLite: tabela produtos)]

  user -->|HTTP| boundary
  boundary --> control
  control --> repo
  repo --> control
  control --> boundary
  boundary -->|JSON| user

  %% Operacoes basicas
  criar[Criar produto]
  repor[Repor estoque]
  baixar[Baixar estoque]
  listar[Listar produtos]
  detalhar[Detalhar produto]

  %% Vistas (apenas anotacoes visuais)
  boundary -. chama .-> control
  control -. usa .-> repo

  %% Exemplos de fluxo
  user -- POST /produtos --> boundary
  boundary --> control
  control -- INSERT --> repo

  user -- POST /produtos/{id}/repor --> boundary
  boundary --> control
  control -- SELECT/UPDATE --> repo

  user -- GET /produtos --> boundary
  boundary --> control
  control -- SELECT * --> repo
