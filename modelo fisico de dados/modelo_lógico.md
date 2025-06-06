# Modelo Lógico do DW Xodó

Este documento descreve o **modelo lógico** do Data Warehouse (DW) após decidirmos unificar as informações de pedidos e itens em uma única tabela de fato (**fato_vendasitens**). O objetivo é simplificar a análise e evitar a complexidade de relacionamentos entre duas tabelas de fato.

---

## Visão Geral

- **Fato principal**: **fato_vendasitens**  
  - Contém as informações de **itens** de pedido, com colunas do cabeçalho (vendedor, cliente, natureza do pedido, datas etc.) inseridas em cada linha.
- **Dimensões**:
  - **dim_pessoa**: informações de clientes (pessoas)  
  - **dim_colaborador**: informações de vendedores (colaboradores)  
  - **dim_filial**: informações de filiais  
  - **dim_produto**: informações de produtos  
  - **dim_motdevolucao**: informações de motivos de devolução  
  - (Opcional) **dim_atividade**: se quisermos manter a atividade do cliente separada  
  - (Opcional) **dim_data**: para análise temporal mais detalhada

---

# Diagrama Lógico (Mermaid)

```mermaid
erDiagram
    dim_filial ||--|{ fato_vendasitens : "FK"
    dim_pessoa ||--|{ fato_vendasitens : "FK"
    dim_colaborador ||--|{ fato_vendasitens : "FK"
    dim_produto ||--|{ fato_vendasitens : "FK"
    dim_motdevolucao ||--|{ fato_vendasitens : "FK"

    dim_filial {
        int fil_codigo PK
        string fil_nome
        %% outros atributos ...
    }

    dim_pessoa {
        int pes_codigo PK
        string pes_razao
        string pes_fantasia
        int pes_atvcodigo FK
        %% se quiser relacionar c/ dim_atividade
        boolean pes_ativo
        %% outros atributos ...
    }

    dim_colaborador {
        int clb_codigo PK
        string clb_razao
        %% outros atributos ...
    }

    dim_produto {
        int pro_codigo PK
        string pro_desc
        %% outros atributos ...
    }

    dim_motdevolucao {
        int mtd_codigo PK
        string mtd_desc
        %% outros atributos ...
    }

    fato_vendasitens {
        string Id_ItemPedido PK
        int ped_filcodigo FK
        int ped_pescodigo FK
        int ped_vencodigo FK
        int ipv_ProCodigo FK
        int ipv_mtdcodigo FK
        %% se for nulo ou 999999, indica "sem motivo"
        string ped_natcodigo
        int ped_numero
        date ped_dtEmissao
        date ped_dtEntrega
        double ipv_Quantidade
        double ipv_propbruto
        %% outros campos ...
    }
