# Projeto - Elaboração, Implantação, Governança e Uso de Banco de Dados em Estudo de Caso

## MATA60 - Banco de Dados
Prof. Robespierre Pita
## 1. Modelando a Base de Dados

### 1.1 PROBLEMA

O COREN-BA, Conselho Regional de Enfermagem da Bahia, é uma autarquia federal essencial para o funcionamento da enfermagem na Bahia. Ele tem o papel de garantir a qualidade e segurança dos serviços prestados, sendo responsável por regulamentar e fiscalizar a prática da enfermagem, certificando que todos os profissionais de enfermagem, ou seja, enfermeiros, técnicos e auxiliares, sigam padrões éticos, legais e técnicos ao exercerem suas funções.

Para exercer seus cargos, os profissionais precisam estar com o seu **"COREN ATIVO"**, o que significa comprovar o diploma de formação, especialidades e também o pagamento da anuidade em dia.

O COREN-BA enfrenta alguns desafios na sua gestão, especialmente no atendimento aos profissionais. Os atendentes que dão suporte precisam otimizar seu trabalho, pois existe alta demanda na gestão dos profissionais de enfermagem. O processo de cadastro gera muito retrabalho, com informações que já poderiam constar no sistema precisando ser preenchidas sempre que um atendimento ao profissional é realizado (inclusão/exclusão). No processo de pagamento da anuidade, por exemplo, os cálculos de multas, parcelas e descontos precisam ser feitos toda vez que há atendimento ao profissional da enfermagem.

Além dos dados profissionais e de ensino, existem processos administrativos, entre outros, para controle e fiscalização. Para os atendimentos, utilizam-se pelo menos três sistemas terceirizados para realizar os procedimentos de gestão do profissional. Além dessa falta de otimização de tempo e trabalho, os programas (sistemas) utilizados já estão defasados, e outros não realizam atualizações periódicas, o que consequentemente prejudica o atendimento. Ferramentas necessárias, como o pagamento via PIX, não foram implantadas, pois não conseguiram corrigir os bugs. O API do Banco do Brasil não funciona corretamente.

Há programas que precisam ter integração com o COFEN - Conselho Federal de Enfermagem. Todos os dados dos profissionais precisam ser repassados para o COFEN, então centralizar essas informações otimizaria a gestão de dados. Acredito que um sistema único, escalável, com integração com o COFEN-BA e as instituições de ensino seja a melhor opção para otimizar o atendimento dos profissionais de enfermagem.
## 1.2 REQUISITOS DO SISTEMA DE INFORMAÇÃO

O sistema tem como objetivo fazer a gestão das informações dos profissionais cadastrados, assim como ser capaz de incluir novos profissionais. Deve também obter informações das instituições de ensino vinculadas aos profissionais para comprovar a veracidade dos diplomas apresentados. Além disso, o sistema deve fazer a gestão do pagamento da anuidade da carteira profissional de enfermagem, verificando parcelas pendentes, renovação, quitação, e também a verificação de processos que o profissional pode estar envolvido.

### Requisitos Funcionais (RF)

**RF1: Cadastro de Profissionais**  
O sistema deve permitir o cadastro de enfermeiros, técnicos e auxiliares de enfermagem com informações como:
- Dados pessoais (nome completo, CPF, data de nascimento, sexo, email, telefone, foto).
- Formação profissional (instituição de ensino, diploma, curso, tipo de diploma, data de concessão).
- Registro no COREN (número do COREN, data de inscrição, conselho regional).
- Situação profissional (ativo, suspenso, inativo, cancelado).
- Especialidades (áreas de atuação, data de conclusão).
- Histórico de pagamentos de anuidade (ano de referência, valor pago, data de pagamento, forma de pagamento, status do pagamento).

**RF2: Verificação e Atualização do Status do COREN**  
O sistema deve permitir a verificação do status do COREN dos profissionais e possibilitar a atualização do status conforme a comprovação de diploma, especialidades e pagamento da anuidade.

**RF3: Controle de Pagamento de Anuidades**  
O sistema deve gerenciar o pagamento de anuidades, calcular automaticamente multas, parcelas e descontos, e registrar os pagamentos feitos:
- Valor base da anuidade.
- Multas por atraso no pagamento.
- Descontos por pagamento em dia ou antecipado.
- Parcelamento da anuidade em até X parcelas (definível pelo sistema).
- Integração com gateways de pagamento para recebimento online.
- Registro automático dos pagamentos realizados.
- Confirmação da efetivação dos pagamentos realizados.
- Atualização automática do status dos pagamentos no sistema.
- Identificação e solução de divergências entre os dados do sistema e os extratos bancários.
- Emissão de comprovantes de pagamento da anuidade.
- O sistema deve suportar pagamentos via PIX, garantindo uma opção de pagamento rápida e eficiente.

**RF4: Histórico de Atendimento**  
O sistema deve manter um histórico de todos os atendimentos realizados para cada profissional, incluindo detalhes de cada interação e as informações fornecidas ou atualizadas.

**RF5: Integração com o COFEN**  
O sistema deve ser capaz de integrar-se com o COFEN para enviar e receber informações dos profissionais de enfermagem em tempo real:
- Sincronização de dados de profissionais em tempo real.
- Recebimento de notificações sobre alterações no status dos profissionais no COFEN-BA.
- Envio de informações sobre os profissionais do COREN-BA para o COFEN-BA, conforme exigências.

**RF6: Integração com Instituições de Ensino**  
O sistema deve integrar-se com instituições de ensino para verificar e validar automaticamente diplomas e certificados de especialização dos profissionais.

**RF7: Gerenciamento de Processos**  
O sistema deve ser capaz de gerar relatórios detalhados sobre a situação dos profissionais, incluindo status do COREN, pagamentos pendentes, atendimentos realizados, e outras métricas relevantes. Deve também permitir o cadastro, consulta e edição de processos administrativos, incluindo:
- Tipo de processo (ético, administrativo, disciplinar).
- Número do processo.
- Data de abertura do processo.
- Profissional envolvido.
- Conselho regional responsável.
- Descrição do caso.
- Anexação de documentos e arquivos relevantes.

**RF8: Agendamento de Atendimentos**  
O sistema deve permitir o agendamento de atendimentos para os profissionais, facilitando a organização e evitando filas e esperas prolongadas.

**RF9: Segurança e Acesso**  
O sistema deve garantir a segurança dos dados com controle de acesso baseado em perfis de usuário, permitindo apenas aos usuários autorizados realizar determinadas ações. Deve também incluir:
- Autenticação e autorização de usuários.
- Criação e gerenciamento de perfis de usuário com diferentes níveis de acesso.
- Definição de permissões de acesso para cada funcionalidade do sistema.
- Registro de logs de acesso, incluindo data, hora, usuário, ação realizada e IP de origem.
# 1.3 Delimitação do Mini-Mundo para o Banco de Dados

## Tabelas

### 1. Tabela `Profissional`
- **`ce_id_profissional INT AUTO_INCREMENT PRIMARY KEY`**: Identificador único do profissional (chave primária).
- **`nome VARCHAR(255) NOT NULL`**: Nome do profissional (obrigatório).
- **`cpf CHAR(11) UNIQUE NOT NULL`**: CPF do profissional (obrigatório e único).
- **`data_nascimento DATE`**: Data de nascimento do profissional.
- **`email VARCHAR(255)`**: Email do profissional.
- **`telefone VARCHAR(20)`**: Telefone de contato do profissional.
- **`endereco TEXT`**: Endereço do profissional.

### 2. Tabela `Especialidade`
- **`cp_id_especialidade INT AUTO_INCREMENT PRIMARY KEY`**: Identificador único da especialidade (chave primária).
- **`descricao VARCHAR(255) NOT NULL`**: Descrição da especialidade (obrigatório).

### 3. Tabela `Diploma`
- **`cp_id_diploma INT AUTO_INCREMENT PRIMARY KEY`**: Identificador único do diploma (chave primária).
- **`nome_instituicao VARCHAR(255) NOT NULL`**: Nome da instituição emissora do diploma (obrigatório).
- **`data_conclusao DATE`**: Data de conclusão do curso ou formação.
- **`documento TEXT`**: Documento ou descrição do diploma.

### 4. Tabela `Instituicao`
- **`cp_id_instituicao INT AUTO_INCREMENT PRIMARY KEY`**: Identificador único da instituição (chave primária).
- **`nome VARCHAR(255) NOT NULL`**: Nome da instituição (obrigatório).
- **`endereco TEXT`**: Endereço da instituição.

### 5. Tabela `Pagamento`
- **`cp_id_pagamento INT AUTO_INCREMENT PRIMARY KEY`**: Identificador único do pagamento (chave primária).
- **`descricao VARCHAR(255) NOT NULL`**: Descrição do pagamento (obrigatório).
- **`detalhes TEXT`**: Detalhes adicionais sobre o pagamento.

### 6. Tabela `Processo_Pagamento`
- **`cp_id_processo_pagamento INT AUTO_INCREMENT PRIMARY KEY`**: Identificador único do processo de pagamento (chave primária).
- **`ce_id_profissional INT`**: Chave estrangeira que referencia um profissional.
- **`cp_id_pagamento INT`**: Chave estrangeira que referencia um pagamento.
- **`status ENUM('Pendente', 'Pago', 'Cancelado') NOT NULL`**: Status do processo de pagamento (obrigatório).
- **`FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(ce_id_profissional)`**: Chave estrangeira que referencia a tabela `Profissional`.
- **`FOREIGN KEY (cp_id_pagamento) REFERENCES Pagamento(cp_id_pagamento)`**: Chave estrangeira que referencia a tabela `Pagamento`.

### 7. Tabela `Triagem`
- **`cp_id_triagem INT AUTO_INCREMENT PRIMARY KEY`**: Identificador único da triagem (chave primária).
- **`ce_id_profissional INT NULL`**: Chave estrangeira opcional que referencia um profissional.
- **`ce_id_usuario INT NOT NULL`**: Chave estrangeira que referencia um usuário.
- **`data_hora_triagem DATETIME NOT NULL`**: Data e hora da triagem (obrigatório).
- **`tipo_atendimento ENUM('Cadastro', 'Renovação', 'Regularização') NOT NULL`**: Tipo de atendimento (obrigatório).
- **`detalhes TEXT`**: Detalhes adicionais sobre a triagem.
- **`status_triagem ENUM('Pendente', 'Concluída', 'Cancelada') NOT NULL`**: Status da triagem (obrigatório).
- **`FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(ce_id_profissional)`**: Chave estrangeira que referencia a tabela `Profissional`.
- **`FOREIGN KEY (ce_id_usuario) REFERENCES Usuario(ce_id_usuario)`**: Chave estrangeira que referencia a tabela `Usuario`.

### 8. Tabela `Atendimento`
- **`cp_id_atendimento INT AUTO_INCREMENT PRIMARY KEY`**: Identificador único do atendimento (chave primária).
- **`ce_id_profissional INT NOT NULL`**: Chave estrangeira que referencia um profissional (obrigatório).
- **`ce_id_usuario INT NOT NULL`**: Chave estrangeira que referencia um usuário (obrigatório).
- **`data_hora_atendimento DATETIME NOT NULL`**: Data e hora do atendimento (obrigatório).
- **`tipo_serviço ENUM('Cadastro', 'Renovação', 'Regularização') NOT NULL`**: Tipo de serviço prestado (obrigatório).
- **`detalhes TEXT`**: Detalhes adicionais sobre o atendimento.
- **`status_atendimento ENUM('Pendente', 'Concluída', 'Cancelada') NOT NULL`**: Status do atendimento (obrigatório).
- **`FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(ce_id_profissional)`**: Chave estrangeira que referencia a tabela `Profissional`.
- **`FOREIGN KEY (ce_id_usuario) REFERENCES Usuario(ce_id_usuario)`**: Chave estrangeira que referencia a tabela `Usuario`.

### 9. Tabela `Profissional_Especialidade`
- **`ce_id_profissional INT NOT NULL`**: Chave estrangeira que referencia um profissional (obrigatório).
- **`cp_id_especialidade INT NOT NULL`**: Chave estrangeira que referencia uma especialidade (obrigatório).
- **`PRIMARY KEY (ce_id_profissional, cp_id_especialidade)`**: Combinação das chaves como única (relacionamento N:N).
- **`FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(ce_id_profissional)`**: Chave estrangeira que referencia a tabela `Profissional`.
- **`FOREIGN KEY (cp_id_especialidade) REFERENCES Especialidade(cp_id_especialidade)`**: Chave estrangeira que referencia a tabela `Especialidade`.

### 10. Tabela `Profissional_Diploma`
- **`ce_id_profissional INT NOT NULL`**: Chave estrangeira que referencia um profissional (obrigatório).
- **`cp_id_diploma INT NOT NULL`**: Chave estrangeira que referencia um diploma (obrigatório).
- **`PRIMARY KEY (ce_id_profissional, cp_id_diploma)`**: Combinação das chaves como única (relacionamento N:N).
- **`FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(ce_id_profissional)`**: Chave estrangeira que referencia a tabela `Profissional`.
- **`FOREIGN KEY (cp_id_diploma) REFERENCES Diploma(cp_id_diploma)`**: Chave estrangeira que referencia a tabela `Diploma`.

### 11. Tabela `Profissional_Instituicao`
- **`ce_id_profissional INT NOT NULL`**: Chave estrangeira que referencia um profissional (obrigatório).
- **`cp_id_instituicao INT NOT NULL`**: Chave estrangeira que referencia uma instituição (obrigatório).
- **`PRIMARY KEY (ce_id_profissional, cp_id_instituicao)`**: Combinação das chaves como única (relacionamento N:N).
- **`FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(ce_id_profissional)`**: Chave estrangeira que referencia a tabela `Profissional`.
- **`FOREIGN KEY (cp_id_instituicao) REFERENCES Instituicao(cp_id_instituicao)`**: Chave estrangeira que referencia a tabela `Instituicao`.

### 12. Tabela `Usuario`
- **`ce_id_usuario INT AUTO_INCREMENT PRIMARY KEY`**: Identificador único do usuário (chave primária).
- **`nome VARCHAR(255) NOT NULL`**: Nome do usuário (obrigatório).
- **`email VARCHAR(255) NOT NULL`**: Email do usuário (obrigatório).
- **`senha VARCHAR(255) NOT NULL`**: Senha do usuário (obrigatório).
- **`perfil ENUM('Admin', 'Usuario') NOT NULL`**: Perfil do usuário (obrigatório).


## RELAÇÕES ENTRE AS ENTIDADES

### 1. Profissional e Especialidade
- **Relação:** Muitos para Muitos (N:N)
- **Descrição:** 
  - Um profissional pode ter uma ou várias especialidades.
  - Uma especialidade pode ser atribuída a um ou vários profissionais.
- **Tabela Intermediária:** Profissional_Especialidade

### 2. Profissional e Diploma
- **Relação:** Muitos para Muitos (N:N)
- **Descrição:**
  - Um profissional pode ter um ou vários diplomas.
  - Um diploma pode ser associado a um ou vários profissionais.
- **Tabela Intermediária:** Profissional_Diploma

### 3. Profissional e Instituição
- **Relação:** Muitos para Muitos (N:N)
- **Descrição:**
  - Um profissional pode estar associado a uma ou várias instituições.
  - Uma instituição pode estar associada a um ou vários profissionais.
- **Tabela Intermediária:** Profissional_Instituicao

### 4. Profissional e Processo_Pagamento
- **Relação:** Um para Muitos (1:N)
- **Descrição:**
  - Um profissional pode ter um ou vários processos de pagamento.
  - Cada processo de pagamento é atribuído a um único profissional.
- **Chave Estrangeira:** ce_id_profissional na tabela Processo_Pagamento

### 5. Pagamento e Processo_Pagamento
- **Relação:** Um para Muitos (1:N)
- **Descrição:**
  - Um pagamento pode ser registrado em um ou vários processos de pagamento.
  - Um processo de pagamento está relacionado a um único pagamento.
- **Chave Estrangeira:** cp_id_pagamento na tabela Processo_Pagamento

### 6. Triagem e Profissional
- **Relação:** Um para Muitos (1:N)
- **Descrição:**
  - Uma triagem pode estar associada a um único profissional ou nenhum.
  - Um profissional pode passar por uma ou várias triagens.
- **Chave Estrangeira:** ce_id_profissional (opcional) na tabela Triagem

### 7. Triagem e Usuario
- **Relação:** Um para Muitos (1:N)
- **Descrição:**
  - Uma triagem é sempre realizada por um único usuário.
  - Um usuário pode realizar várias triagens.
- **Chave Estrangeira:** ce_id_usuario na tabela Triagem

### 8. Atendimento e Profissional
- **Relação:** Um para Muitos (1:N)
- **Descrição:**
  - Um atendimento é sempre relacionado a um único profissional.
  - Um profissional pode ser atendido várias vezes.
- **Chave Estrangeira:** ce_id_profissional na tabela Atendimento

### 9. Atendimento e Usuario
- **Relação:** Um para Muitos (1:N)
- **Descrição:**
  - Um atendimento é realizado por um único usuário.
  - Um usuário pode realizar vários atendimentos.
- **Chave Estrangeira:** ce_id_usuario na tabela Atendimento

### 10. Usuario
- **Relação:** Um para Muitos (1:N) com as tabelas Triagem e Atendimento
- **Descrição:**
  - Um usuário pode realizar várias triagens e atendimentos.
  - Cada triagem ou atendimento é registrado por um único usuário.


*Diagrama conceitual*
![Diagrama Conceitual](./img/diagrama_inicial/conceitual.png)

*Diagrama lógico*
![Diagrama Conceitual](./img/diagrama_inicial/logico.png)

### *Modificação no diagrama lógico após iniciar no SQL*
![Modificação no diagrama lógico](./img/DER_corenba.png)
## 2. Criando a Estrutura Relacional no SGBD

Objetivo:

Criar a estrutura de um banco de dados relacional utilizando um Sistema de Gerenciamento de Banco de Dados (SGBD).
O projeto Utiliza o MySQL para gerenciamneto do banco de dados e para administrá-lo estou usando o phpMyAdmin, junto com o DBeaver para ter uma melhor visualização.

**Código SQL:**

### *Criando o banco de dados:*
```
sql
CREATE DATABASE corenba_db;
USE corenba_db;
```
### *Tela de Login*
Será a tela que o usuário irá fazer o login para iniciar o procedimento de atendimento ao profissional.
Nessa tela irá digitar:
|		|				|	
-------|--------------|
|Login|  *usuário* |
|Senha|************|

**|ENTRAR|**

> [!NOTE]
*Botão "ENTRAR" verifica usuário e acesso no sistema. Se as credenciais estiverem corretas, o sistema redireciona o usuário para a Tela Inicial.*
*os usuários do sistema são cadastrados pelo Departamento de Tecnologia*

- *Tabela Usuário*
```
CREATE TABLE tbl_usuario (
    cp_id_usuario INT AUTO_INCREMENT PRIMARY KEY,
    nome_usuario VARCHAR(100) NOT NULL,
    senha VARCHAR(255) NOT NULL,
    perfil_acesso VARCHAR(50) NOT NULL
);
```
---
### *Tela Inicial*
Após acesso ao sistema, uma nova tela irá aparecer com um *menu* na parte superior:

```mermaid
flowchart TD
    A[Profissional] --> B(cadastro) --> C(consulta)
    D[Agendamento] --> E(novo) --> F(consulta)
    G[Processos] --> H(cadastro) --> I(consulta)
```
>Menu `Profissional` terá submenus para acesso as telas de `consulta` e `cadastro`.

Tela de Consulta e Edição de Profissionais

Componentes:

    Campo de Busca: Um campo para buscar profissionais pelo nome ou CPF.
    Lista de Resultados: Exibe uma lista de profissionais encontrados com base nos critérios de busca.
    Botões de Ação: Botões de "Editar" e "Excluir" ao lado de cada profissional listado.

Fluxo:

    O usuário insere um nome ou CPF no campo de busca e clica em "Buscar".
    O sistema exibe os resultados na lista de resultados.
    Ao clicar em "Editar", o usuário é levado para a Tela de Edição de Profissionais com os dados do profissional pré-preenchidos.
    Ao clicar em "Excluir", o sistema solicita uma confirmação antes de excluir o profissional do banco de dados.


- Tabela Profissional
```
CREATE TABLE tbl_profissional (
    cp_id_profissional INT AUTO_INCREMENT PRIMARY KEY,
    nome_completo VARCHAR(200) NOT NULL,
    cpf VARCHAR(11) UNIQUE NOT NULL,
    data_nascimento DATE NOT NULL,
    sexo CHAR(1) NOT NULL,
    email VARCHAR(100),
    telefone VARCHAR(15),
    registro_coren VARCHAR(20) UNIQUE NOT NULL,
    data_inscricao DATE NOT NULL,
    conselho_regional INT NOT NULL,
    situacao_profissional VARCHAR(20) NOT NULL,
    FOREIGN KEY (conselho_regional) REFERENCES tbl_conselho_regional(cp_id_conselho)
);
```
- Tabela Instituição
```
CREATE TABLE tbl_instituicao (
    cp_id_instituicao INT AUTO_INCREMENT PRIMARY KEY,
    nome_instituicao VARCHAR(100) NOT NULL,
    endereco VARCHAR(200),
    telefone VARCHAR(15),
    email VARCHAR(100)
);
```
- Tabela Diploma
```
CREATE TABLE tbl_diploma (
    cp_id_diploma INT AUTO_INCREMENT PRIMARY KEY,
    ce_id_profissional INT NOT NULL,
    ce_id_instituicao INT NOT NULL,
    curso VARCHAR(100) NOT NULL,
    tipo_diploma VARCHAR(50) NOT NULL,
    data_concessao DATE NOT NULL,
    FOREIGN KEY (ce_id_profissional) REFERENCES tbl_profissional(cp_id_profissional),
    FOREIGN KEY (ce_id_instituicao) REFERENCES tbl_instituicao(cp_id_instituicao)
);
```
- Tabela Pagamento
```
CREATE TABLE tbl_pagamento (
    cp_id_pagamento INT AUTO_INCREMENT PRIMARY KEY,
    ce_id_profissional INT NOT NULL,
    ano_referencia INT NOT NULL,
    valor_pago DECIMAL(10, 2) NOT NULL,
    data_pagamento DATE NOT NULL,
    forma_pagamento VARCHAR(50),
    status_pagamento VARCHAR(20),
    FOREIGN KEY (ce_id_profissional) REFERENCES tbl_profissional(cp_id_profissional)
);
```
- Tabela Processo
```
CREATE TABLE tbl_processo (
    cp_id_processo INT AUTO_INCREMENT PRIMARY KEY,
    tipo_processo VARCHAR(50) NOT NULL,
    numero_processo VARCHAR(50) NOT NULL,
    data_abertura DATE NOT NULL,
    conselho_responsavel INT,
    descricao TEXT,
    FOREIGN KEY (conselho_responsavel) REFERENCES tbl_conselho_regional(cp_id_conselho)
);
```
- Tabela Profissional_Processo (N:M entre Profissional e Processo)
```
CREATE TABLE tbl_profissional_processo (
    ce_id_profissional INT NOT NULL,
    ce_id_processo INT NOT NULL,
    data_inclusao DATE NOT NULL,
    funcao_no_processo VARCHAR(100),
    PRIMARY KEY (ce_id_profissional, ce_id_processo),
    FOREIGN KEY (ce_id_profissional) REFERENCES tbl_profissional(cp_id_profissional),
    FOREIGN KEY (ce_id_processo) REFERENCES tbl_processo(cp_id_processo)
);
```
- Tabela Especialidade
```
CREATE TABLE tbl_especialidade (
    cp_id_especialidade INT AUTO_INCREMENT PRIMARY KEY,
    ce_id_profissional INT NOT NULL,
    area_atuacao VARCHAR(100) NOT NULL,
    data_conclusao DATE,
    FOREIGN KEY (ce_id_profissional) REFERENCES tbl_profissional(cp_id_profissional)
);
```

- Tabela Atendimento
```
CREATE TABLE tbl_atendimento (
    cp_id_atendimento INT AUTO_INCREMENT PRIMARY KEY,
    ce_id_profissional INT NOT NULL,
    data_hora DATETIME NOT NULL,
    detalhes TEXT,
    ce_id_usuario INT NOT NULL,
    FOREIGN KEY (ce_id_profissional) REFERENCES tbl_profissional(cp_id_profissional),
    FOREIGN KEY (ce_id_usuario) REFERENCES tbl_usuario(cp_id_usuario)
);
```
- Tabela Agendamento
```
CREATE TABLE tbl_agendamento (
    cp_id_agendamento INT AUTO_INCREMENT PRIMARY KEY,
    ce_id_profissional INT NOT NULL,
    data_hora DATETIME NOT NULL,
    motivo TEXT,
    ce_id_usuario INT NOT NULL,
    FOREIGN KEY (ce_id_profissional) REFERENCES tbl_profissional(cp_id_profissional),
    FOREIGN KEY (ce_id_usuario) REFERENCES tbl_usuario(cp_id_usuario)
);
```
- Tabela Conselho Regional
```
CREATE TABLE tbl_conselho_regional (
    cp_id_conselho INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    endereco VARCHAR(200),
    telefone VARCHAR(15),
    email VARCHAR(100),
    website VARCHAR(100),
    cofen_id INT,
    FOREIGN KEY (cofen_id) REFERENCES tbl_cofen(cp_id_cofen)
);
```
- Tabela COFEN
```
CREATE TABLE tbl_cofen (
    cp_id_cofen INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    endereco VARCHAR(200),
    telefone VARCHAR(15),
    email VARCHAR(100),
    website VARCHAR(100)
);
```
## 3. Definindo as Constraints

No código SQL já foram denifidas. Abaixo um esquema explicativo:

- **`tbl_profissional`**: Conecta a tabela `tbl_profissional` com `tbl_conselho_regional` através da constraint `fk_profissional_conselho_regional`.
- **`tbl_diploma`**: Conecta a tabela `tbl_diploma` com `tbl_profissional` e `tbl_instituicao` através das constraints `fk_diploma_profissional` e `fk_diploma_instituicao`.
- **`tbl_pagamento`**: Conecta a tabela `tbl_pagamento` com `tbl_profissional` através da constraint `fk_pagamento_profissional`.
- **`tbl_processo`**: Conecta a tabela `tbl_processo` com `tbl_conselho_regional` através da constraint `fk_processo_conselho_regional`.
- **`tbl_profissional_processo`**: Conecta a tabela `tbl_profissional_processo` com `tbl_profissional` e `tbl_processo` através das constraints `fk_profissional_processo_profissional` e `fk_profissional_processo_processo`.
- **`tbl_especialidade`**: Conecta a tabela `tbl_especialidade` com `tbl_profissional` através da constraint `fk_especialidade_profissional`.
- **`tbl_atendimento`**: Conecta a tabela `tbl_atendimento` com `tbl_profissional` e `tbl_usuario` através das constraints `fk_atendimento_profissional` e `fk_atendimento_usuario`.
- **`tbl_agendamento`**: Conecta a tabela `tbl_agendamento` com `tbl_profissional` e `tbl_usuario` através das constraints `fk_agendamento_profissional` e `fk_agendamento_usuario`.
- **`tbl_conselho_regional`**: Conecta a tabela `tbl_conselho_regional` com `tbl_cofen` através da constraint `fk_conselho_regional_cofen`.

## 4. OLTP 1: Populando a base de dados

- Inserir dados na tabela tbl_cofen
```
INSERT INTO tbl_cofen (nome, endereco, telefone, email, website)
VALUES
    ('Conselho Federal de Enfermagem', 'Rua do Cofen, 123', '21 99999-0000', 'contato@cofen.gov.br', 'www.cofen.gov.br');
```
- Inserir dados na tabela tbl_conselho_regional
```
INSERT INTO tbl_conselho_regional (nome, endereco, telefone, email, website, ce_id_cofen)
VALUES
    ('COREN-Bahia', 'R. Gen. Labatut, 273', '71 88888-0000', 'contato@corenba.gov.br', 'www.corenba.gov.br', 1);
```
- Inserir dados na tabela tbl_profissional
```
INSERT INTO tbl_profissional (nome_completo, cpf, data_nascimento, sexo, email, telefone, registro_coren, data_inscricao, conselho_regional, situacao_profissional)
VALUES
    ('Maria Silva', '12345678901', '1985-05-15', 'F', 'maria.silva@example.com', '71 98765-4321', 'COREN-1234', '2024-01-10', 1, 'Ativo'),
    ('João Souza', '23456789012', '1990-10-20', 'M', 'joao.souza@example.com', '71 91234-5678', 'COREN-5678', '2024-02-15', 1, 'Ativo'),
    ('Ana Oliveira', '34567890123', '1978-03-30', 'F', 'ana.oliveira@example.com', '71 99876-5432', 'COREN-9101', '2024-03-20', 1, 'Suspenso'),
    ('Carlos Pereira', '45678901234', '1982-07-12', 'M', 'carlos.pereira@example.com', '71 91123-4567', 'COREN-1112', '2024-04-25', 1, 'Ativo'),
    ('Fernanda Lima', '56789012345', '1995-12-05', 'F', 'fernanda.lima@example.com', '71 95432-1234', 'COREN-1314', '2024-05-10', 1, 'Inativo');
```
- Inserir dados na tabela tbl_instituicao
```
INSERT INTO tbl_instituicao (nome_instituicao, endereco, telefone, email)
VALUES
    ('Universidade Federal da Bahia', 'Rua Barão de Jeremoabo, s/n', '71 3283-6500', 'ufba@ufba.br'),
    ('Centro Universitário Jorge Amado', 'Av. Luís Viana Filho, 3146', '71 4009-9000', 'unijorge@unijorge.edu.br'),
    ('Faculdade Bahiana de Medicina', 'Av. Dom João VI, 275', '71 3276-8266', 'bahiana@bahiana.edu.br'),
    ('Faculdade de Tecnologia e Ciências', 'Av. Paralela, 3170', '71 3206-8000', 'ftc@ftc.edu.br'),
    ('Universidade Estadual de Feira de Santana', 'Av. Transnordestina, s/n', '75 3161-8000', 'uefs@uefs.br');
```
- Inserir dados na tabela tbl_diploma
```
INSERT INTO tbl_diploma (ce_id_profissional, ce_id_instituicao, curso, tipo_diploma, data_concessao)
VALUES
    (1, 1, 'Enfermagem', 'Bacharelado', '2007-12-15'),
    (2, 2, 'Enfermagem', 'Bacharelado', '2012-07-20'),
    (3, 3, 'Enfermagem Obstétrica', 'Especialização', '2005-09-10'),
    (4, 4, 'Enfermagem Pediátrica', 'Especialização', '2009-05-05'),
    (5, 5, 'Enfermagem de Saúde Pública', 'Mestrado', '2017-11-25');
```
- Inserir dados na tabela tbl_pagamento
```
INSERT INTO tbl_pagamento (ce_id_profissional, ano_referencia, valor_pago, data_pagamento, forma_pagamento, status_pagamento)
VALUES
    (1, 2024, 150.00, '2024-06-20', 'Boleto', 'Pago'),
    (2, 2024, 150.00, '2024-06-21', 'Cartão de Crédito', 'Pago'),
    (3, 2024, 150.00, '2024-06-22', 'Transferência Bancária', 'Pago'),
    (4, 2024, 150.00, '2024-06-23', 'Boleto', 'Pendente'),
    (5, 2024, 150.00, '2024-06-24', 'Cartão de Débito', 'Pago');
```
- Inserir dados na tabela tbl_processo
```
INSERT INTO tbl_processo (tipo_processo, numero_processo, data_abertura, conselho_responsavel, descricao)
VALUES
    ('Processo Disciplinar', '12345/2024', '2024-07-01', 1, 'Investigação de conduta profissional.'),
    ('Processo Administrativo', '67890/2024', '2024-07-05', 1, 'Revisão de documentos.'),
    ('Processo Ético', '11223/2024', '2024-07-10', 1, 'Denúncia de prática antiética.'),
    ('Processo Disciplinar', '44556/2024', '2024-07-15', 1, 'Falta grave cometida.'),
    ('Processo Judicial', '77889/2024', '2024-07-20', 1, 'Ação judicial em andamento.');
```
- Inserir dados na tabela tbl_profissional_processo
```
INSERT INTO tbl_profissional_processo (ce_id_profissional, ce_id_processo, data_inclusao, funcao_no_processo)
VALUES
    (1, 1, '2024-07-02', 'Requerido');
```
- Inserir dados na tabela tbl_especialidade
```
INSERT INTO tbl_especialidade (ce_id_profissional, area_atuacao, data_conclusao)
VALUES
    (1, 'Enfermagem Pediátrica', '2023-05-01'),
    (2, 'Enfermagem Obstétrica', '2019-10-10'),
    (3, 'Enfermagem de Saúde Pública', '2018-03-20'),
    (4, 'Enfermagem Psiquiátrica', '2021-12-15'),
    (5, 'Enfermagem em Urgência e Emergência', '2022-06-30');
```
- Inserir dados na tabela tbl_usuario
```
INSERT INTO tbl_usuario (nome_usuario, senha, perfil_acesso)
VALUES
    ('admin', 'senha123', 'Administrador'),
    ('josé_silva', 'senha456', 'Usuário'),
    ('marlo_oliveira', 'senha789', 'Usuário'),
    ('carlos_santos', 'senha321', 'Moderador'),
    ('rafael_lima', 'senha654', 'Usuário');
```
- Inserir dados na tabela tbl_agendamento
```
INSERT INTO tbl_agendamento (ce_id_profissional, data_hora, motivo, ce_id_usuario)
VALUES
    (1, '2024-08-20 09:00:00', 'Emissão de nova carteira profissional.', 1),
    (2, '2024-08-21 10:00:00', 'Verificação de vencimento de carteira.', 2),
    (3, '2024-08-22 11:00:00', 'Pagamento de anuidade.', 3),
    (4, '2024-08-23 12:00:00', 'Revisão de processo disciplinar.', 4),
    (5, '2024-08-24 13:00:00', 'Consulta sobre atualização cadastral.', 5);
```
- Inserir dados na tabela tbl_atendimento
```
INSERT INTO tbl_atendimento (ce_id_profissional, data_hora, detalhes, ce_id_usuario)
VALUES
    INSERT INTO tbl_atendimento (ce_id_profissional, data_hora, detalhes, ce_id_usuario)
VALUES
    (1, '2024-08-20 09:30:00', 'Atendimento para emissão de nova carteira profissional.', 1),
    (2, '2024-08-21 10:30:00', 'Atendimento para verificação do vencimento da carteira.', 2),
    (3, '2024-08-22 11:30:00', 'Atendimento para pagamento de anuidade.', 3),
    (4, '2024-08-23 12:30:00', 'Atendimento para revisão de processo disciplinar.', 4),
    (5, '2024-08-24 13:30:00', 'Atendimento para consulta sobre atualização cadastral.', 5);
```
## 5. OLTP 2: Alterando e excluindo itens:

Comandos SQL para Exclusão, Alteração e Inclusão de Registros

### 1. Exclusão de Registros

sql
```
-- Exclusão de um profissional específico
DELETE FROM tbl_profissional WHERE cpf = '56789012345';

-- Exclusão de um diploma específico
DELETE FROM tbl_diploma WHERE tipo_diploma = 'Mestrado' AND curso = 'Enfermagem de Saúde Pública';

-- Exclusão de um agendamento específico
DELETE FROM tbl_agendamento WHERE data_hora = '2024-08-24 13:00:00';

-- Exclusão de um usuário específico
DELETE FROM tbl_usuario WHERE nome_usuario = 'rafael_lima';

-- Exclusão de um processo específico
DELETE FROM tbl_processo WHERE numero_processo = '77889/2024';
```