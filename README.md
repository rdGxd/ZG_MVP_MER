# Modelo de Entidade-Relacionamento (MER) - Solução Zero Glosa (ZG)

## Entidades Principais
- **Prestador**
- **Guia prestador**
- **Item de prestador**
- **Relatório de Conciliação**
- **Glosa**
- **Usuário**
- **Beneficiário**

## Relacionamentos
- Um **Prestador** pode ter várias **Guia prestador**.
- Uma **Guia prestador** pode incluir vários **Itens de prestador**.
- Um **Relatório de Conciliação** está associado a uma **Guia prestador**.
- Um **Item de prestador** pode ter zero ou varias **Glosa**.
- Um **Usuário** (por exemplo, analista, administrador) pode acessar e manipular várias **Contas Hospitalares** e **Relatórios de Conciliação**.
- As **Regras de Negócio** são aplicadas aos **Itens de Faturamento** para validação.

## Esboço do MER

```mermaiderDiagram
    HOSPITAL ||--o{ CONTA_HOSPITALAR : possui
    CONTA_HOSPITALAR ||--o{ ITEM_DE_FATURAMENTO : inclui
    CONTA_HOSPITALAR ||--|| RELATORIO_DE_CONCILIACAO : gera
    ITEM_DE_FATURAMENTO ||--o| GLOSA : pode_ter
    USUARIO ||--o{ CONTA_HOSPITALAR : acessa
    USUARIO ||--o{ RELATORIO_DE_CONCILIACAO : acessa
    ITEM_DE_FATURAMENTO ||--o{ REGRA_DE_NEGOCIO : segue

    HOSPITAL {
      ID int PK
      Nome varchar
      Endereco varchar
      Telefone varchar
    }

    CONTA_HOSPITALAR {
      ID int PK
      HospitalID int FK
      Data date
      Status varchar
    }

    ITEM_DE_FATURAMENTO {
      ID int PK
      ContaHospitalarID int FK
      Descricao varchar
      Valor decimal
      Status varchar
    }

    RELATORIO_DE_CONCILIACAO {
      ID int PK
      ContaHospitalarID int FK
      DataEmissao date
      ItensAceitos int
      ItensRejeitados int
      ItensPendentes int
    }

    GLOSA {
      ID int PK
      ItemFaturamentoID int FK
      Motivo varchar
      ValorCorrigido decimal
      DataCorrecao date
    }

    USUARIO {
      ID int PK
      Nome varchar
      Email varchar
      Senha varchar
      TipoUsuario varchar
    }

    REGRA_DE_NEGOCIO {
      ID int PK
      Descricao varchar
      TipoValidacao varchar
    }
```
## Descrição das Entidades
    Hospital: Armazena informações sobre o hospital.

    Guia prestador: Representa uma conta específica de um hospital, incluindo dados como data e status.
    
    Item de Faturamento: Cada conta hospitalar tem diversos itens de faturamento.

    Relatório de Conciliação: Contém informações sobre os resultados da conciliação, incluindo itens aceitos, rejeitados e pendentes.

    Glosa: Relacionada a itens de faturamento, representa itens corrigidos ou rejeitados.
    
    Usuário: Diferentes tipos de usuários que interagem com o sistema (operadores, administradores).
    
    Regra de Negócio: Define as regras aplicadas aos itens de faturamento para validação e conciliação.


## Mermaid syntax
    ||--o{ = Relacionamento Um para Muitos (1:N)
    }o--o{ = Relacionamento Muitos para Muitos (N:N)
    ||--|| = Relacionamento Um para Um (1:1)
    o|--|| = Relacionamento Zero para Um (0:1)

## Resumo dos Símbolos
    ||: Um e somente um (1)
    o|: Zero ou um (0..1)
    |{: Um ou muitos (1..N)
    o{: Zero ou muitos (0..N)
    }o: Muitos, zero ou mais (N)