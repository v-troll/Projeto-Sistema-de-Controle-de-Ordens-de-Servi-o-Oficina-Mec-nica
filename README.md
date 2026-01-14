
# Projeto: Sistema de Controle de Ordens de Serviço – Oficina Mecânica

## Contexto
Este projeto modela um sistema para controle e gerenciamento de execução de ordens de serviço (OS) em uma oficina mecânica. O objetivo é organizar clientes, veículos, equipes de mecânicos, serviços e peças utilizados em cada OS.

### Premissas da Narrativa
- **Clientes** levam veículos à oficina para conserto ou revisão.
- Cada **veículo** é designado a uma **equipe de mecânicos** que identifica os serviços e preenche uma OS com data de entrega.
- A OS calcula valores com base em:
  - **Serviços** (consultando tabela de referência de mão-de-obra).
  - **Peças** utilizadas.
- Cliente autoriza execução dos serviços.
- A mesma equipe avalia e executa os serviços.
- Mecânicos possuem: código, nome, endereço e especialidade.
- Cada OS possui: número, data de emissão, valor total, status e data prevista para conclusão.

---

## Entidades e Relacionamentos
- **Cliente**: dados pessoais e contato.
- **Veículo**: placa, modelo, ano, vinculado a um cliente.
- **OrdemServico (OS)**: número, datas, status, valor total, vinculado a um veículo e a uma equipe.
- **Equipe**: grupo de mecânicos.
- **Mecânico**: código, nome, endereço, especialidade.
- **Serviço**: descrição, valor referência.
- **Peça**: descrição, valor unitário.
- **ItensServiço**: serviços executados na OS.
- **ItensPeça**: peças utilizadas na OS.

---

## Diagrama ER (Conceitual)

```mermaid
erDiagram
    CLIENTE ||--o{ VEICULO : "possui"
    VEICULO ||--o{ ORDEMSERVICO : "tem"
    ORDEMSERVICO ||--o{ ITEM_SERVICO : "inclui"
    ORDEMSERVICO ||--o{ ITEM_PECA : "inclui"
    ORDEMSERVICO }|--|| EQUIPE : "designada"
    EQUIPE ||--o{ MECANICO : "composta por"
    ITEM_SERVICO }|--|| SERVICO : "referencia"
    ITEM_PECA }|--|| PECA : "referencia"

    CLIENTE {
      id UUID PK
      nome TEXT
      telefone TEXT
      endereco TEXT
    }

    VEICULO {
      id UUID PK
      cliente_id UUID FK
      placa TEXT UNIQUE
      modelo TEXT
      ano INT
    }

    ORDEMSERVICO {
      id UUID PK
      veiculo_id UUID FK
      equipe_id UUID FK
      numero TEXT UNIQUE
      data_emissao DATE
      data_conclusao DATE
      status TEXT "ABERTA|EM_EXECUCAO|CONCLUIDA|CANCELADA"
      valor_total NUMERIC
    }

    EQUIPE {
      id UUID PK
      nome TEXT
    }

    MECANICO {
      id UUID PK
      equipe_id UUID FK
      nome TEXT
      endereco TEXT
      especialidade TEXT
    }

    SERVICO {
      id UUID PK
      descricao TEXT
      valor_referencia NUMERIC
    }

    PECA {
      id UUID PK
      descricao TEXT
      valor_unitario NUMERIC
    }

    ITEM_SERVICO {
      id UUID PK
      ordem_servico_id UUID FK
      servico_id UUID FK
      quantidade INT
      valor NUMERIC
    }

    ITEM_PECA {
      id UUID PK
      ordem_servico_id UUID FK
      peca_id UUID FK
      quantidade INT
      valor NUMERIC
    }

