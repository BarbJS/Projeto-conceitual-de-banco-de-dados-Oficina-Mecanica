# Projeto Conceitual de Banco de Dados - Oficina Mecânica

Este repositório contém o modelo conceitual e lógico de banco de dados para um Sistema de Gerenciamento de uma Oficina Mecânica: 'PC Oficina Mecanica.mwb'. O projeto foi desenhado para atender a requisitos complexos de ordens de serviço, gestão de equipes e precificação dinâmica de mão-de-obra de serviços e peças.

## 📋 Contexto do Projeto

O sistema visa controlar o fluxo de trabalho de uma oficina, desde a chegada do veículo até a entrega final ao cliente. O modelo resolve problemas de relacionamento entre ordens de serviço, peças e serviços, além de garantir a integridade dos dados cadastrais e financeiros. Este projeto foi criado para cumprimento do Desafio 02 - Esquema Conceitual de Banco de Dados - Oficina Mecânica, proposto pela professora Juliana Mascarenhas, do curso de Formação SQL Database Specialist, da plataforma de ensino DIO (Digital Innovation One).

## 🎯 Objetivos de Negócio Atendidos

Com base nas narrativas de levantamento de requisitos, o modelo cobre:
* **Gestão de Equipes:** Veículos são designados a equipes específicas de mecânicos que avaliam e executam o serviço.
* **Ordem de Serviço (OS):** Controle centralizado contendo status, datas, autorização do cliente e valores.
* **Precificação Dinâmica:** O custo da mão-de-obra é calculado consultando uma tabela de referência, somado ao valor das peças utilizadas.
* **Histórico:** Um cliente pode ter muitos veículos. Um veículo pode ter múltiplas revisões ou consertos ao longo do tempo, mantendo o registro de cada intervenção.

## 🏗 Arquitetura e Modelagem

O diagrama EER foi desenvolvido no **MySQL Workbench** seguindo as seguintes diretrizes e refinamentos:

### 1. Clientes e Veículos
* **Herança (PF/PJ):** Utilização de especialização para clientes, separando `Pessoa Fisica` (CPF) e `Pessoa Juridica` (CNPJ) sob uma entidade pai `Cliente`, garantindo integridade fiscal.
* **Frota:** Relacionamento 1:N, onde um cliente pode possuir múltiplos veículos cadastrados.

### 2. Catálogo de Serviços Inteligente (Refinamento)
Diferente de modelos simples onde a OS define o tipo de serviço, aqui a classificação é feita no nível do **Serviço do Catálogo**:
* **Entidade:** `Serviço`.
* **Categorização:** Atributo `categoria` (Enum: 'Revisão', 'Conserto', 'Manutenção', etc.) incluído diretamente na tabela de referência.
* **Benefício:** Isso permite que uma única Ordem de Serviço seja "mista" (ex: contenha uma Revisão Preventiva e também um Conserto de farol), com o sistema identificando automaticamente a natureza de cada item.

### 3. Núcleo Operacional (OS)
A entidade `Ordem_Servico` atua como o *hub* central do sistema:
* **Status Controlados:** Fluxo definido via ENUM ('Analise', 'Aguardando Autorizacao', 'Em Execucao', 'Concluido', 'Cancelado').
* **Composição:** Uma OS é composta por N Serviços e N Peças através de tabelas associativas (`Serviços dentro da OS` e `Peças dentro da OS`), preservando o valor histórico cobrado no momento do serviço.

### 4. Recursos Humanos
* Mecânicos possuem cadastro detalhado (código, endereço, especialidade).
* A organização é feita através da entidade **Equipe**, facilitando a alocação de tarefas por grupos de trabalho especializados.

### 5. Refinamentos Extras
* **Pagamentos Múltiplos:** Normalização para permitir que um cliente cadastre múltiplas formas de pagamento.
* **Entrega/Retirada:** Entidade dedicada (`Entrega`) para controlar o status de saída do veículo, separando a conclusão técnica da retirada física pelo cliente.

## 🚀 Como Utilizar

1.  Baixe e instale o [MySQL Workbench](https://www.mysql.com/products/workbench/).
2.  Clone o repositório.
3.  Abra o arquivo `.mwb` no MySQL Workbench.
4.  Analise os relacionamentos e os tipos de dados (especialmente os `ENUMs` de categoria e status).
5.  Utilize a função "Forward Engineer" do Workbench para gerar o banco de dados físico.

*Projeto desenvolvido como parte de desafio de modelagem de dados para oficinas mecânicas.*
