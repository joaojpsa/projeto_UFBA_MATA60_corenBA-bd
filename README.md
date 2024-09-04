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

## Estrutura das Telas do Programa

### 1. Tela de Login
- **Descrição:** 
  - Tela de entrada onde os usuários devem inserir suas credenciais para acessar o sistema.
- **Componentes:**
  - Campo para Email.
  - Campo para Senha.
  - Botão "Entrar".
  - Link para recuperação de senha.

### 2. Tela Principal (Dashboard e Relatórios)
- **Descrição:** 
  - Tela inicial após o login, com um painel de controle e acesso aos principais relatórios.
- **Componentes:**
  - **Dashboard:**
    - Resumo de Atendimentos Pendentes.
    - Resumo de Triagens Pendentes.
    - Indicadores de Status de Pagamentos e Processos.
    - Acesso rápido para iniciar novos atendimentos e triagens.
  - **Relatórios:**
    - Campos de filtro para definir parâmetros dos relatórios (Data, Tipo de Atendimento, Status, etc.).
    - Botão "Gerar Relatório" para exibir dados filtrados.
    - Opção para exportar relatórios em formato PDF ou Excel.

### 3. Tela de Triagem de Profissional
- **Descrição:** 
  - Tela onde todos os profissionais passam para verificar se já possuem cadastro no sistema e se têm pendências antes de prosseguir para o atendimento.
- **Componentes:**
  - Campo de busca por CPF ou Nome do profissional.
  - Exibição dos detalhes do profissional, caso já esteja cadastrado.
  - Indicadores de pendências, se houver.
  - Botão "Registrar Triagem" para finalizar essa etapa e encaminhar para o atendimento.

### 4. Tela de Atendimento
- **Descrição:** 
  - Tela para registrar um novo atendimento após a triagem, onde também é possível cadastrar o profissional se ele ainda não estiver no sistema.
- **Componentes:**
  - Exibição dos detalhes do profissional, incluindo informações verificadas na triagem.
  - Campo para selecionar o usuário responsável pelo atendimento.
  - Campo para Data e Hora do Atendimento.
  - Campo para selecionar o Tipo de Serviço:
    - **Cadastro Inicial e Pagamento da Carteira** (caso o profissional não tenha cadastro)
    - **Renovação**
    - **Quitar Dívidas**
    - **Alteração de Cadastro**
    - **Verificar Processos**
  - **Seção para Cadastro do Profissional** (caso não esteja cadastrado):
    - Campos de entrada para Nome, CPF, Data de Nascimento, Email, Telefone, e Endereço.
    - Seção para associar Especialidades ao profissional.
    - Seção para associar Diplomas ao profissional.
  - Campo para Detalhes adicionais.
  - Botão "Salvar" para registrar o atendimento e/ou cadastro.

### 5. Tela de Processos
- **Descrição:** 
  - Tela para visualizar e gerenciar os processos relacionados aos profissionais, como pendências financeiras ou outras solicitações.
- **Componentes:**
  - Campo de busca por CPF ou Nome do profissional.
  - Lista de processos associados ao profissional, incluindo ID do Processo, Tipo, Status, e Detalhes.
  - Opções para atualizar o status do processo, adicionar notas, ou anexar documentos.
  - Botão "Adicionar Novo Processo" para criar um novo registro de processo.

### 6. Tela de Ajuda
- **Descrição:** 
  - Tela para fornecer suporte e instruções de uso do sistema.
- **Componentes:**
  - Seção de FAQs.
  - Link para documentação do sistema.
  - Opção de contato para suporte técnico.

### Observações
- **Gerenciamento de Usuários:** O gerenciamento de usuários do sistema é feito internamente pela equipe de TI e não está acessível através das telas do programa.

## 2.1Código SQL:

## *Criando o banco de dados:*
```
sql
CREATE DATABASE corenba_db;
USE corenba_db;
```

- *Tabela Usuário*
```
CREATE TABLE Usuario (
    ce_id_usuario INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária (Primary Key)
    nome VARCHAR(255) NOT NULL,  -- Restrição NOT NULL para o campo 'nome'
    email VARCHAR(255) UNIQUE NOT NULL,  -- Restrição UNIQUE e NOT NULL para o campo 'email'
    senha VARCHAR(255) NOT NULL,  -- Restrição NOT NULL para o campo 'senha'
    papel ENUM('Atendente', 'Administrador') NOT NULL  -- Restrição ENUM e NOT NULL para o campo 'papel'
);

```
*
- *Tabela Profissional*
```
CREATE TABLE Profissional (
    id INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária (Primary Key)
    nome VARCHAR(255) NOT NULL,  -- Restrição NOT NULL para o campo 'nome'
    cpf CHAR(11) UNIQUE NOT NULL,  -- Restrição UNIQUE e NOT NULL para o campo 'cpf'
    data_nascimento DATE,  -- Campo opcional (não possui NOT NULL)
    email VARCHAR(255),  -- Campo opcional
    telefone VARCHAR(20),  -- Campo opcional
    endereco TEXT,  -- Campo opcional
    id_corenba INT,  -- Chave estrangeira (Foreign Key) referenciando a tabela CorenBA
    FOREIGN KEY (id_corenba) REFERENCES tbl_conselho_regional(cp_id_conselho)  -- Cria a Foreign Key
);
```

- *Tabela Processo*
```
CREATE TABLE tbl_processo (
    cp_id_processo INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária
    ce_id_profissional INT,  -- Chave estrangeira referenciando Profissional
    tipo_processo VARCHAR(50),  -- Tipo de processo
    numero_processo VARCHAR(50),  -- Número do processo
    data_abertura DATE,  -- Data de abertura do processo
    conselho_responsavel VARCHAR(50),  -- Conselho responsável
    descricao TEXT,  -- Descrição do processo
    FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(ce_id_profissional)  -- Constraint de chave estrangeira
);

```
- *Tabela Instituição*
```
CREATE TABLE Profissional_Instituicao (
    ce_id_profissional INT NOT NULL,  -- Chave estrangeira (Foreign Key) referenciando a tabela Profissional
    cp_id_instituicao INT NOT NULL,  -- Chave estrangeira (Foreign Key) referenciando a tabela Instituicao
    PRIMARY KEY (ce_id_profissional, cp_id_instituicao),  -- Chave primária composta
    FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id),  -- Cria a Foreign Key
    FOREIGN KEY (cp_id_instituicao) REFERENCES Instituicao(cp_id_instituicao)  -- Cria a Foreign Key
);

```
- *Tabela Diploma*
```
CREATE TABLE Diploma (
    cp_id_diploma INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária (Primary Key)
    descricao VARCHAR(255) NOT NULL,  -- Restrição NOT NULL para o campo 'descricao'
    instituicao VARCHAR(255) NOT NULL  -- Restrição NOT NULL para o campo 'instituicao'
);

```
- *Tabela Profissional_Diploma*
```
CREATE TABLE Profissional_Diploma (
    ce_id_profissional INT NOT NULL,  -- Chave estrangeira (Foreign Key) referenciando a tabela Profissional
    cp_id_diploma INT NOT NULL,  -- Chave estrangeira (Foreign Key) referenciando a tabela Diploma
    PRIMARY KEY (ce_id_profissional, cp_id_diploma),  -- Chave primária composta
    FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id),  -- Cria a Foreign Key
    FOREIGN KEY (cp_id_diploma) REFERENCES Diploma(cp_id_diploma)  -- Cria a Foreign Key
);

```
- *Tabela Pagamento*
```
CREATE TABLE Pagamento (
    cp_id_pagamento INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária (Primary Key)
    descricao VARCHAR(255) NOT NULL,  -- Restrição NOT NULL para o campo 'descricao'
    detalhes TEXT  -- Campo opcional
);

```

- *Tabela Processo_Pagamento*
```
CREATE TABLE Processo_Pagamento (
    cp_id_processo_pagamento INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária (Primary Key)
    ce_id_profissional INT,  -- Chave estrangeira (Foreign Key) referenciando a tabela Profissional
    cp_id_pagamento INT,  -- Chave estrangeira (Foreign Key) referenciando a tabela Pagamento
    status ENUM('Pendente', 'Pago', 'Cancelado') NOT NULL,  -- Restrição ENUM e NOT NULL para o campo 'status'
    FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id),  -- Cria a Foreign Key
    FOREIGN KEY (cp_id_pagamento) REFERENCES Pagamento(cp_id_pagamento)  -- Cria a Foreign Key
);

```
- *Tabela Triagem*
```
CREATE TABLE Triagem (
    cp_id_triagem INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária (Primary Key)
    ce_id_profissional INT,  -- Chave estrangeira (Foreign Key) referenciando a tabela Profissional
    ce_id_usuario INT NOT NULL,  -- Chave estrangeira (Foreign Key) referenciando a tabela Usuario
    data_hora_triagem DATETIME NOT NULL,  -- Restrição NOT NULL para o campo 'data_hora_triagem'
    tipo_atendimento ENUM('Cadastro', 'Renovação', 'Regularização') NOT NULL,  -- Restrição ENUM e NOT NULL para o campo 'tipo_atendimento'
    detalhes TEXT,  -- Campo opcional
    status_triagem ENUM('Pendente', 'Concluída', 'Cancelada') NOT NULL,  -- Restrição ENUM e NOT NULL para o campo 'status_triagem'
    FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id),  -- Cria a Foreign Key
    FOREIGN KEY (ce_id_usuario) REFERENCES Usuario(ce_id_usuario)  -- Cria a Foreign Key
);

```
- *Tabela Especialidade*
```
CREATE TABLE Profissional_Especialidade (
    ce_id_profissional INT NOT NULL,  -- Chave estrangeira (Foreign Key) referenciando a tabela Profissional
    cp_id_especialidade INT NOT NULL,  -- Chave estrangeira (Foreign Key) referenciando a tabela Especialidade
    PRIMARY KEY (ce_id_profissional, cp_id_especialidade),  -- Chave primária composta
    FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id),  -- Cria a Foreign Key
    FOREIGN KEY (cp_id_especialidade) REFERENCES Especialidade(cp_id_especialidade)  -- Cria a Foreign Key
);

```

- *Tabela Atendimento*
```
CREATE TABLE Atendimento (
    cp_id_atendimento INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária (Primary Key)
    ce_id_profissional INT NOT NULL,  -- Chave estrangeira (Foreign Key) referenciando a tabela Profissional
    ce_id_usuario INT NOT NULL,  -- Chave estrangeira (Foreign Key) referenciando a tabela Usuario
    data_hora_atendimento DATETIME NOT NULL,  -- Restrição NOT NULL para o campo 'data_hora_atendimento'
    tipo_servico ENUM('Cadastro', 'Renovação', 'Regularização') NOT NULL,  -- Restrição ENUM e NOT NULL para o campo 'tipo_servico'
    detalhes TEXT,  -- Campo opcional
    status_atendimento ENUM('Pendente', 'Concluída', 'Cancelada') NOT NULL,  -- Restrição ENUM e NOT NULL para o campo 'status_atendimento'
    FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id),  -- Cria a Foreign Key
    FOREIGN KEY (ce_id_usuario) REFERENCES Usuario(ce_id_usuario)  -- Cria a Foreign Key
);

```
- *Tabela CorenBA*
```
CREATE TABLE tbl_conselho_regional (
    cp_id_conselho INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária (Primary Key)
    nome VARCHAR(100) NOT NULL,  -- Restrição NOT NULL para o campo 'nome'
    endereco VARCHAR(200),  -- Campo opcional
    telefone VARCHAR(15),  -- Campo opcional
    email VARCHAR(100),  -- Campo opcional
    website VARCHAR(100),  -- Campo opcional
    cofen_id INT,  -- Chave estrangeira (Foreign Key) referenciando a tabela COFEN
    FOREIGN KEY (cofen_id) REFERENCES tbl_cofen(cp_id_cofen)  -- Cria a Foreign Key
);

```
- *Tabela COFEN*
```
CREATE TABLE tbl_cofen (
    cp_id_cofen INT AUTO_INCREMENT PRIMARY KEY,  -- Chave primária (Primary Key)
    nome VARCHAR(100) NOT NULL,  -- Restrição NOT NULL para o campo 'nome'
    endereco VARCHAR(200),  -- Campo opcional
    telefone VARCHAR(15),  -- Campo opcional
    email VARCHAR(100),  -- Campo opcional
    website VARCHAR(100)  -- Campo opcional
);

```
## 3. Definindo as Constraints

No código SQL já foram denifidas. Abaixo um esquema explicativo:

### Tabela `Usuario`
- **`ce_id_usuario INT AUTO_INCREMENT PRIMARY KEY`**
  - **AUTO_INCREMENT**: Garante que o valor da coluna `ce_id_usuario` é único e gerado automaticamente para cada novo registro.
  - **PRIMARY KEY**: Define a coluna `ce_id_usuario` como chave primária, assegurando unicidade e indexação eficiente.
- **`nome VARCHAR(255) NOT NULL`**
  - **NOT NULL**: Garante que a coluna `nome` não pode ter valores nulos.
- **`email VARCHAR(255) UNIQUE NOT NULL`**
  - **UNIQUE**: Garante que todos os valores na coluna `email` são únicos.
  - **NOT NULL**: Garante que a coluna `email` não pode ter valores nulos.
- **`senha VARCHAR(255) NOT NULL`**
  - **NOT NULL**: Garante que a coluna `senha` não pode ter valores nulos.
- **`papel ENUM('Atendente', 'Administrador') NOT NULL`**
  - **ENUM**: Limita os valores da coluna `papel` a um conjunto específico de opções.
  - **NOT NULL**: Garante que a coluna `papel` não pode ter valores nulos.

### Tabela `Profissional`
- **`id INT AUTO_INCREMENT PRIMARY KEY`**
  - **AUTO_INCREMENT**: Garante que o valor da coluna `id` é único e gerado automaticamente para cada novo registro.
  - **PRIMARY KEY**: Define a coluna `id` como chave primária.
- **`cpf CHAR(11) UNIQUE NOT NULL`**
  - **UNIQUE**: Garante que todos os valores na coluna `cpf` são únicos.
  - **NOT NULL**: Garante que a coluna `cpf` não pode ter valores nulos.
- **`id_corenba INT`**
  - **FOREIGN KEY (id_corenba) REFERENCES tbl_conselho_regional(cp_id_conselho)**
    - Define a coluna `id_corenba` como chave estrangeira que referencia a coluna `cp_id_conselho` da tabela `tbl_conselho_regional`.

### Tabela `Processo`
- **`cp_id_processo INT AUTO_INCREMENT PRIMARY KEY`**
  - **AUTO_INCREMENT**: Garante que o valor da coluna `cp_id_processo` é único e gerado automaticamente.
  - **PRIMARY KEY**: Define a coluna `cp_id_processo` como chave primária.
- **`ce_id_profissional INT`**
  - **FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id)**
    - Define a coluna `ce_id_profissional` como chave estrangeira que referencia a coluna `id` da tabela `Profissional`.

### Tabela `Profissional_Instituicao`
- **`ce_id_profissional INT NOT NULL`**
  - **FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id)**
    - Define a coluna `ce_id_profissional` como chave estrangeira que referencia a coluna `id` da tabela `Profissional`.
- **`cp_id_instituicao INT NOT NULL`**
  - **FOREIGN KEY (cp_id_instituicao) REFERENCES Instituicao(cp_id_instituicao)**
    - Define a coluna `cp_id_instituicao` como chave estrangeira que referencia a coluna `cp_id_instituicao` da tabela `Instituicao`.
- **PRIMARY KEY (ce_id_profissional, cp_id_instituicao)**
  - Define uma chave primária composta usando as colunas `ce_id_profissional` e `cp_id_instituicao`.

### Tabela `Diploma`
- **`cp_id_diploma INT AUTO_INCREMENT PRIMARY KEY`**
  - **AUTO_INCREMENT**: Garante que o valor da coluna `cp_id_diploma` é único e gerado automaticamente.
  - **PRIMARY KEY**: Define a coluna `cp_id_diploma` como chave primária.
- **`descricao VARCHAR(255) NOT NULL`**
  - **NOT NULL**: Garante que a coluna `descricao` não pode ter valores nulos.
- **`instituicao VARCHAR(255) NOT NULL`**
  - **NOT NULL**: Garante que a coluna `instituicao` não pode ter valores nulos.

### Tabela `Profissional_Diploma`
- **`ce_id_profissional INT NOT NULL`**
  - **FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id)**
    - Define a coluna `ce_id_profissional` como chave estrangeira que referencia a coluna `id` da tabela `Profissional`.
- **`cp_id_diploma INT NOT NULL`**
  - **FOREIGN KEY (cp_id_diploma) REFERENCES Diploma(cp_id_diploma)**
    - Define a coluna `cp_id_diploma` como chave estrangeira que referencia a coluna `cp_id_diploma` da tabela `Diploma`.
- **PRIMARY KEY (ce_id_profissional, cp_id_diploma)**
  - Define uma chave primária composta usando as colunas `ce_id_profissional` e `cp_id_diploma`.

### Tabela `Pagamento`
- **`cp_id_pagamento INT AUTO_INCREMENT PRIMARY KEY`**
  - **AUTO_INCREMENT**: Garante que o valor da coluna `cp_id_pagamento` é único e gerado automaticamente.
  - **PRIMARY KEY**: Define a coluna `cp_id_pagamento` como chave primária.
- **`descricao VARCHAR(255) NOT NULL`**
  - **NOT NULL**: Garante que a coluna `descricao` não pode ter valores nulos.

### Tabela `Processo_Pagamento`
- **`cp_id_processo_pagamento INT AUTO_INCREMENT PRIMARY KEY`**
  - **AUTO_INCREMENT**: Garante que o valor da coluna `cp_id_processo_pagamento` é único e gerado automaticamente.
  - **PRIMARY KEY**: Define a coluna `cp_id_processo_pagamento` como chave primária.
- **`ce_id_profissional INT`**
  - **FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id)**
    - Define a coluna `ce_id_profissional` como chave estrangeira que referencia a coluna `id` da tabela `Profissional`.
- **`cp_id_pagamento INT`**
  - **FOREIGN KEY (cp_id_pagamento) REFERENCES Pagamento(cp_id_pagamento)**
    - Define a coluna `cp_id_pagamento` como chave estrangeira que referencia a coluna `cp_id_pagamento` da tabela `Pagamento`.
- **`status ENUM('Pendente', 'Pago', 'Cancelado') NOT NULL`**
  - **ENUM**: Limita os valores da coluna `status` a um conjunto específico de opções.
  - **NOT NULL**: Garante que a coluna `status` não pode ter valores nulos.

### Tabela `Triagem`
- **`cp_id_triagem INT AUTO_INCREMENT PRIMARY KEY`**
  - **AUTO_INCREMENT**: Garante que o valor da coluna `cp_id_triagem` é único e gerado automaticamente.
  - **PRIMARY KEY**: Define a coluna `cp_id_triagem` como chave primária.
- **`ce_id_profissional INT`**
  - **FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id)**
    - Define a coluna `ce_id_profissional` como chave estrangeira que referencia a coluna `id` da tabela `Profissional`.
- **`ce_id_usuario INT NOT NULL`**
  - **FOREIGN KEY (ce_id_usuario) REFERENCES Usuario(ce_id_usuario)**
    - Define a coluna `ce_id_usuario` como chave estrangeira que referencia a coluna `ce_id_usuario` da tabela `Usuario`.
- **`data_hora_triagem DATETIME NOT NULL`**
  - **NOT NULL**: Garante que a coluna `data_hora_triagem` não pode ter valores nulos.
- **`tipo_atendimento ENUM('Cadastro', 'Renovação', 'Regularização') NOT NULL`**
  - **ENUM**: Limita os valores da coluna `tipo_atendimento` a um conjunto específico de opções.
  - **NOT NULL**: Garante que a coluna `tipo_atendimento` não pode ter valores nulos.
- **`status_triagem ENUM('Pendente', 'Concluída', 'Cancelada') NOT NULL`**
  - **ENUM**: Limita os valores da coluna `status_triagem` a um conjunto específico de opções.
  - **NOT NULL**: Garante que a coluna `status_triagem` não pode ter valores nulos.

### Tabela `Profissional_Especialidade`
- **`ce_id_profissional INT NOT NULL`**
  - **FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(id)**
    - Define a coluna `ce_id_profissional` como chave estrangeira que referencia a coluna `id` da tabela `Profissional`.
- **`cp_id_especialidade INT NOT NULL`**
  - **FOREIGN KEY (cp_id_especialidade) REFERENCES Especialidade(cp_id_especialidade)**
    - Define a coluna `cp_id_especialidade` como chave estrangeira que referencia a coluna `cp_id_especialidade` da tabela `Especialidade`.
- **PRIMARY KEY (ce_id_profissional, cp_id_especialidade)**
  - Define uma chave primária composta usando as colunas `ce_id_profissional` e `cp_id_especialidade`.

### Tabela `Atendimento`
- **`cp_id_atendimento INT AUTO_INCREMENT PRIMARY KEY`**
  - **AUTO_INCREMENT**: Garante que o valor da coluna `cp_id_atendimento` é único e gerado automaticamente.
  - **PRIMARY KEY**: Define a coluna `cp_id_atendimento` como chave primária.
- **`cp_id_processo INT`**
  - **FOREIGN KEY (cp_id_processo) REFERENCES Processo(cp_id_processo)**
    - Define a coluna `cp_id_processo` como chave estrangeira que referencia a coluna `cp_id_processo` da tabela `Processo`.
- **`data_hora_atendimento DATETIME NOT NULL`**
  - **NOT NULL**: Garante que a coluna `data_hora_atendimento` não pode ter valores nulos.
- **`observacoes TEXT`**
  - **TEXT**: Permite armazenar grandes quantidades de texto.
- **`status_atendimento ENUM('Agendado', 'Realizado', 'Cancelado') NOT NULL`**
  - **ENUM**: Limita os valores da coluna `status_atendimento` a um conjunto específico de opções.
  - **NOT NULL**: Garante que a coluna `status_atendimento` não pode ter valores nulos.


## 4. OLTP 1: Populando a base de dados

- Inserir dados na tabela COFEN
```
INSERT INTO `Cofen` (`id`, `id_corenba`, `nome`, `endereco`, `telefone`, `email`, `website`) VALUES
(1, NULL, 'Conselho Federal de Enfermagem', 'SCLN Qd. 304, Lote 09, Bl. E, Asa Norte, Brasília – DF', '61 3329-5800', 'contato@cofen.gov.br', 'www.cofen.gov.br');

```
- Inserir dados na tabela COREN-BA
```
INSERT INTO `CorenBA` (`id`, `nome`, `endereco`, `telefone`, `email`, `website`, `cofen_id`) VALUES
(5, 'Coren-BA', 'R. Gen. Labatut, 273 - Barris, Salvador - BA, 40070-100', '(71) 3277-3100', 'contato@coren-ba.org.br', 'www.coren-ba.org.br', 1);
```
- Inserir dados na tabela PROFISSIONAL
```
INSERT INTO `Profissional` (`ce_id_profissional`, `nome`, `cpf`, `data_nascimento`, `email`, `telefone`, `endereco`, `id_corenba`) VALUES
(1, 'Hillary Kincaid', '2491776324', '2024-08-26', 'hkincaid0@odnoklassniki.ru', '198-886-9805', 'Suite 90', NULL),
(2, 'Hillel Letson', '6367815961', '2023-11-24', 'hletson1@ed.gov', '954-476-2910', 'Room 1194', NULL),
(3, 'Shir Forsey', '8349493658', '2023-09-21', 'sforsey2@admin.ch', '731-826-4766', 'PO Box 50438', NULL),
(4, 'Bobina Gulk', '1437516866', '2024-01-31', 'bgulk3@netvibes.com', '332-377-0613', 'Suite 3', NULL),
(5, 'Fremont Trimbey', '2370561688', '2024-01-17', 'ftrimbey4@vinaora.com', '430-911-4702', 'PO Box 5751', NULL),
(6, 'Cicely Gerlack', '9917096418', '2024-04-18', 'cgerlack5@auda.org.au', '991-880-8335', 'Room 359', NULL),
(7, 'Clemente Jinkins', '8102622288', '2024-07-14', 'cjinkins6@china.com.cn', '197-309-2615', 'Room 1125', NULL),
(8, 'Pierce Jakubovsky', '3234483396', '2024-04-25', 'pjakubovsky7@prweb.com', '974-714-6095', 'PO Box 53236', NULL),
(9, 'Bronnie Tichelaar', '0196815649', '2024-08-02', 'btichelaar8@hostgator.com', '137-978-4023', '1st Floor', NULL),
(10, 'Paddy Keenor', '9153508203', '2023-12-29', 'pkeenor9@google.es', '868-646-6066', 'Room 1633', NULL),
(11, 'Korella Larciere', '2136310380', '2023-10-03', 'klarcierea@si.edu', '684-286-3153', 'Suite 31', NULL),
(12, 'Stillmann Rickis', '5403515380', '2024-07-04', 'srickisb@cdc.gov', '358-383-8910', 'Suite 55', NULL),
(13, 'Julia Radborn', '9155971458', '2024-04-07', 'jradbornc@craigslist.org', '431-735-9616', '4th Floor', NULL),
(14, 'Yves Lammenga', '7235938765', '2024-05-08', 'ylammengad@51.la', '706-554-9603', '5th Floor', NULL),
(15, 'Emelina Baytrop', '9003250103', '2024-03-16', 'ebaytrope@adobe.com', '378-884-0387', '19th Floor', NULL),
(16, 'Ryley Dudeney', '9047432347', '2023-12-26', 'rdudeneyf@gnu.org', '205-376-1420', '14th Floor', NULL),
(17, 'Demetre Tixier', '1542919495', '2024-01-20', 'dtixierg@cloudflare.com', '733-476-2660', '18th Floor', NULL),
(18, 'Thurston Coytes', '5658871662', '2023-12-23', 'tcoytesh@prlog.org', '243-124-6901', '19th Floor', NULL),
(19, 'Madison Crees', '4221451920', '2024-06-21', 'mcreesi@networksolutions.com', '736-705-0069', 'Apt 1596', NULL),
(20, 'Rollin Caldairou', '9957201131', '2024-01-06', 'rcaldairouj@csmonitor.com', '560-756-8815', '5th Floor', NULL),
(21, 'Kaitlyn Thurstan', '3436068101', '2024-05-15', 'kthurstank@nyu.edu', '662-891-0313', '10th Floor', NULL),
(22, 'Idelle Studeart', '0538140313', '2024-03-07', 'istudeartl@boston.com', '231-894-4807', 'Room 1715', NULL),
(23, 'Eba Holdron', '1112502297', '2024-06-13', 'eholdronm@squarespace.com', '167-618-4301', 'PO Box 97217', NULL),
(24, 'Penn Cona', '0607073314', '2023-10-09', 'pconan@abc.net.au', '549-643-4032', '8th Floor', NULL),
(25, 'Julie Donoghue', '0461806770', '2024-02-08', 'jdonoghueo@sitemeter.com', '935-655-4468', '18th Floor', NULL),
(26, 'Alvira Sweeny', '0551569328', '2023-09-06', 'asweenyp@nymag.com', '755-636-3889', '18th Floor', NULL),
(27, 'Claudio Renyard', '4421322636', '2023-11-14', 'crenyardq@gravatar.com', '449-291-1907', 'Apt 1601', NULL),
(28, 'Charis Deal', '6980447033', '2024-04-17', 'cdealr@bloglovin.com', '112-325-9568', 'Apt 716', NULL),
(29, 'Aluin McClenaghan', '2991826398', '2024-08-30', 'amcclenaghans@reddit.com', '464-313-0227', 'Room 365', NULL),
(30, 'Lesya Cridlin', '4435260522', '2024-02-17', 'lcridlint@cafepress.com', '412-732-9775', 'Room 452', NULL),
(31, 'Clarie Sudell', '8831104829', '2024-01-06', 'csudellu@bigcartel.com', '790-487-4063', 'PO Box 25154', NULL),
(32, 'Alicea Wallach', '6959822157', '2024-05-08', 'awallachv@purevolume.com', '101-447-9806', 'PO Box 52397', NULL),
(33, 'Bette-ann Hands', '3614901785', '2024-06-26', 'bhandsw@deviantart.com', '239-996-8345', 'Room 1499', NULL),
(34, 'Joanie Nipper', '5365312074', '2024-05-01', 'jnipperx@unesco.org', '129-472-5137', 'Apt 1320', NULL),
(35, 'Esme Livingstone', '7380762478', '2024-05-03', 'elivingstoney@nih.gov', '484-211-5375', 'Room 346', NULL),
(36, 'Shandy Fontanet', '5863139536', '2023-12-18', 'sfontanetz@archive.org', '779-612-2222', '10th Floor', NULL),
(37, 'Hamlen Chetwind', '9957893688', '2023-09-06', 'hchetwind10@senate.gov', '524-895-8643', 'Suite 39', NULL),
(38, 'Sashenka Ramsbotham', '6231750469', '2023-11-10', 'sramsbotham11@netscape.com', '567-954-9646', 'Suite 65', NULL),
(39, 'Roddy Ancell', '2520671645', '2023-12-03', 'rancell12@oakley.com', '208-702-4459', 'Apt 1641', NULL),
(40, 'Elbertina Isakovitch', '3604269814', '2023-12-29', 'eisakovitch13@tamu.edu', '297-648-2706', 'Room 207', NULL),
(41, 'Goldia Groll', '0331997290', '2023-11-11', 'ggroll14@samsung.com', '510-661-9580', 'Suite 54', NULL),
(42, 'Trudey Zucker', '8570449763', '2024-01-02', 'tzucker15@oracle.com', '343-160-2792', '11th Floor', NULL),
(43, 'Micki Redier', '4562400293', '2024-01-23', 'mredier16@yolasite.com', '952-412-8323', 'Suite 55', NULL),
(44, 'Rad Brickstock', '5916852126', '2024-08-17', 'rbrickstock17@vimeo.com', '264-554-3820', 'PO Box 46714', NULL),
(45, 'Benjamen Beadon', '8013680398', '2024-01-12', 'bbeadon18@sfgate.com', '812-432-9436', 'PO Box 56393', NULL),
(46, 'Wright Oaks', '9604588338', '2024-02-03', 'woaks19@hexun.com', '390-871-6616', 'Suite 23', NULL),
(47, 'Cary Addionisio', '4128829463', '2023-10-14', 'caddionisio1a@google.com.hk', '108-500-0459', 'Apt 253', NULL),
(48, 'Domenic Fulcher', '1161013598', '2024-07-02', 'dfulcher1b@shutterfly.com', '680-556-9962', 'Suite 33', NULL),
(49, 'Ajay Lohering', '7122399621', '2024-06-04', 'alohering1c@bbc.co.uk', '853-600-1530', '2nd Floor', NULL),
(50, 'Hermione Agget', '4468867728', '2023-11-07', 'hagget1d@privacy.gov.au', '991-682-0570', 'Room 962', NULL),
(51, 'Catharine Adger', '6163006690', '2023-11-08', 'cadger1e@goo.gl', '984-672-8265', '13th Floor', NULL),
(52, 'Joey Speedy', '2865450287', '2024-06-18', 'jspeedy1f@addtoany.com', '616-582-8807', 'Suite 80', NULL),
(53, 'Allissa Fiske', '8154623889', '2024-01-06', 'afiske1g@issuu.com', '106-996-1807', 'Suite 60', NULL),
(54, 'Cherish Girauld', '1153658720', '2024-02-27', 'cgirauld1h@house.gov', '572-242-9988', 'Suite 48', NULL),
(55, 'Ailis Orgee', '1699882193', '2023-10-18', 'aorgee1i@booking.com', '732-264-7989', 'Apt 633', NULL),
(56, 'Ermanno Vatini', '3022806310', '2023-09-05', 'evatini1j@nature.com', '303-774-7107', 'Suite 79', NULL),
(57, 'Mari Strongitharm', '2576235247', '2023-09-10', 'mstrongitharm1k@theatlantic.com', '890-941-3561', '11th Floor', NULL),
(58, 'Datha Jermyn', '5084220338', '2024-04-18', 'djermyn1l@boston.com', '315-844-1542', 'Suite 71', NULL),
(59, 'Miles Mazzilli', '1924778564', '2024-02-02', 'mmazzilli1m@friendfeed.com', '624-567-6350', 'Room 56', NULL),
(60, 'Karia Greiser', '3339159165', '2024-03-19', 'kgreiser1n@imageshack.us', '623-151-9125', '18th Floor', NULL),
(61, 'Chlo Broadnicke', '2516581602', '2023-12-24', 'cbroadnicke1o@whitehouse.gov', '253-117-0976', 'Apt 277', NULL),
(62, 'Laurie Jamieson', '7540373954', '2024-04-17', 'ljamieson1p@aol.com', '659-235-1547', 'Apt 1407', NULL),
(63, 'Bradford Heasman', '8371821719', '2024-01-29', 'bheasman1q@intel.com', '365-515-8606', '6th Floor', NULL),
(64, 'Hildegarde Bodicum', '1584003944', '2023-12-16', 'hbodicum1r@foxnews.com', '923-365-6727', 'Apt 1346', NULL),
(65, 'Richmond Skittreal', '4963431548', '2023-12-20', 'rskittreal1s@php.net', '776-387-5203', 'Suite 50', NULL),
(66, 'Winnah Lesper', '7158397823', '2024-06-19', 'wlesper1t@oakley.com', '648-788-0575', 'Apt 1493', NULL),
(67, 'Loydie Gauche', '4917551005', '2023-09-27', 'lgauche1u@yahoo.co.jp', '206-180-0404', 'Room 1110', NULL),
(68, 'Kat Oguz', '3232001920', '2023-09-14', 'koguz1v@canalblog.com', '969-884-8853', 'PO Box 12539', NULL),
(69, 'Keen Ivimy', '0859159736', '2023-09-09', 'kivimy1w@diigo.com', '218-520-6785', 'Room 617', NULL),
(70, 'Alie Meron', '8866957364', '2024-02-01', 'ameron1x@bravesites.com', '112-226-5213', 'Suite 11', NULL),
(71, 'Warde Stennes', '5144324495', '2024-04-01', 'wstennes1y@amazon.de', '440-800-6120', 'Suite 11', NULL),
(72, 'Abbe Stopher', '4323971028', '2023-11-19', 'astopher1z@bing.com', '528-208-9693', 'PO Box 73564', NULL),
(73, 'Fransisco Benner', '8137697608', '2024-06-27', 'fbenner20@businesswire.com', '681-441-4953', 'Apt 173', NULL),
(74, 'Simonne Leap', '0744093708', '2024-05-23', 'sleap21@sun.com', '940-742-6716', 'Room 1154', NULL),
(75, 'Catarina Jakolevitch', '5598309424', '2024-02-07', 'cjakolevitch22@livejournal.com', '469-845-0961', 'Suite 54', NULL),
(76, 'Hilton Nudde', '3623977276', '2023-11-06', 'hnudde23@sciencedaily.com', '270-813-1382', 'PO Box 50529', NULL),
(77, 'Stanislaus Genty', '9159759291', '2024-01-06', 'sgenty24@naver.com', '631-491-5939', 'PO Box 15263', NULL),
(78, 'Tami Butting', '7466686885', '2024-02-20', 'tbutting25@narod.ru', '839-688-7268', 'Suite 32', NULL),
(79, 'Alister Dove', '4071068574', '2024-08-10', 'adove26@newsvine.com', '260-102-4794', 'Apt 982', NULL),
(80, 'Port Bewshea', '7866509204', '2024-03-09', 'pbewshea27@deviantart.com', '413-866-4984', 'PO Box 62147', NULL),
(81, 'Rochella Flipek', '1800421435', '2024-01-10', 'rflipek28@weibo.com', '248-913-4237', '11th Floor', NULL),
(82, 'Ashlee Vernazza', '9046545490', '2024-05-23', 'avernazza29@nhs.uk', '452-184-5827', 'Suite 76', NULL),
(83, 'Cornelle Neilson', '3306436224', '2024-04-07', 'cneilson2a@mediafire.com', '308-628-9248', 'Apt 12', NULL),
(84, 'Linell Richemont', '4361909067', '2024-08-28', 'lrichemont2b@cam.ac.uk', '708-169-8650', 'Suite 39', NULL),
(85, 'Rab Juris', '0549631224', '2023-10-06', 'rjuris2c@usgs.gov', '502-717-2575', '10th Floor', NULL),
(86, 'Shelia Demanche', '5805277034', '2024-02-27', 'sdemanche2d@china.com.cn', '246-241-4502', 'PO Box 21620', NULL),
(87, 'Tadeas Drever', '2158929650', '2023-11-10', 'tdrever2e@usatoday.com', '824-499-5155', '5th Floor', NULL),
(88, 'Towny Rivard', '4216728588', '2024-07-28', 'trivard2f@hexun.com', '735-297-2488', 'PO Box 7107', NULL),
(89, 'Raphaela Dumper', '9008953803', '2024-02-26', 'rdumper2g@nifty.com', '429-491-1353', 'Room 1415', NULL),
(90, 'Karlen Banbrook', '0585458081', '2024-04-17', 'kbanbrook2h@telegraph.co.uk', '804-553-8186', 'Suite 80', NULL),
(91, 'Abagail Pickvance', '4976797104', '2024-02-04', 'apickvance2i@quantcast.com', '426-117-8167', 'Suite 97', NULL),
(92, 'Sebastiano Ord', '5287337213', '2024-04-04', 'sord2j@irs.gov', '662-310-8242', 'Room 514', NULL),
(93, 'Jonas McCrae', '2941163549', '2023-12-15', 'jmccrae2k@examiner.com', '863-486-3471', 'Room 539', NULL),
(94, 'Micheline Rea', '5142908767', '2024-05-30', 'mrea2l@live.com', '464-587-8403', 'Apt 1227', NULL),
(95, 'Bax De Normanville', '2830399250', '2024-02-17', 'bde2m@t.co', '161-579-9211', '17th Floor', NULL),
(96, 'Hyacinthe Le Galle', '6293118200', '2023-12-20', 'hle2n@nature.com', '976-270-8868', 'PO Box 79542', NULL),
(97, 'Damita Romayne', '7267113109', '2024-07-07', 'dromayne2o@marriott.com', '641-685-3866', '6th Floor', NULL),
(98, 'George Cunningham', '7198730337', '2024-08-10', 'gcunningham2p@cisco.com', '887-778-0975', 'Room 1817', NULL),
(99, 'Steward Sommerlie', '2757121987', '2023-09-28', 'ssommerlie2q@microsoft.com', '105-537-3977', '11th Floor', NULL),
(100, 'Min Le Fleming', '8531887607', '2023-12-27', 'mle2r@google.co.uk', '787-335-0766', 'PO Box 78872', NULL);
```
- Inserir dados na tabela INSTITUICAO
```
IINSERT INTO `Instituicao` (`cp_id_instituicao`, `nome`, `endereco`) VALUES
(1, 'Instituto São João', 'Rua das Flores, 123, Centro'),
(2, 'Escola Técnica do Porto', 'Avenida Brasil, 456, Bairro Jardim'),
(3, 'Colégio Universitário', 'Praça da República, 789, Centro Histórico'),
(4, 'Centro Educacional Nova Era', 'Rua das Palmeiras, 101, Jardim das Acácias'),
(5, 'Instituto de Ensino Moderno', 'Avenida dos Estados, 202, Vila Nova'),
(6, 'Escola de Tecnologia Avançada', 'Rua das Oliveiras, 303, Bairro dos Pinheiros'),
(7, 'Colégio São Pedro', 'Rua das Orquídeas, 404, Parque das Rosas'),
(8, 'Instituto de Ciências Sociais', 'Avenida das Nações, 505, Centro'),
(9, 'Escola Profissionalizante Norte', 'Rua do Sol, 606, Bairro Esperança'),
(10, 'Centro de Educação e Cultura', 'Rua das Margaridas, 707, Jardim das Flores'),
(11, 'Instituto de Pesquisa Tecnológica', 'Avenida da Inovação, 808, Vila Verde'),
(12, 'Colégio da Juventude', 'Rua dos Lírios, 909, Centro'),
(13, 'Instituto de Estudos Avançados', 'Avenida dos Trabalhadores, 1010, Bairro Industrial'),
(14, 'Escola Secundária de São Paulo', 'Rua do Progresso, 1111, Vila São Jorge'),
(15, 'Centro de Formação Profissional', 'Rua das Aroeiras, 1212, Jardim do Sol'),
(16, 'Instituto de Educação e Tecnologia', 'Avenida das Indústrias, 1313, Bairro dos Navegantes'),
(17, 'Colégio Municipal do Futuro', 'Rua das Palmas, 1414, Centro'),
(18, 'Instituto de Ensino Superior', 'Rua da Proclamação, 1515, Bairro Esperança'),
(19, 'Escola de Formação Técnica', 'Avenida da Tecnologia, 1616, Jardim das Acácias'),
(20, 'Centro Educacional São Jorge', 'Rua do Comércio, 1717, Vila Nova'),
(21, 'Instituto de Ensino Completo', 'Rua do Desenvolvimento, 1818, Centro Histórico'),
(22, 'Colégio da Inovação', 'Avenida das Flores, 1919, Bairro Jardim'),
(23, 'Instituto de Formação Profissional', 'Rua do Conhecimento, 2020, Jardim das Rosas'),
(24, 'Escola Técnica de São Pedro', 'Rua dos Anjos, 2121, Centro'),
(25, 'Centro de Estudos Avançados', 'Avenida das Águas, 2222, Vila Verde'),
(26, 'Instituto de Ensino e Cultura', 'Rua das Begônias, 2323, Bairro dos Pinheiros'),
(27, 'Colégio São Rafael', 'Rua da Alegria, 2424, Parque das Flores'),
(28, 'Instituto de Tecnologia e Pesquisa', 'Avenida da Ciência, 2525, Bairro Jardim'),
(29, 'Escola Secundária de São Luiz', 'Rua da Prosperidade, 2626, Vila Nova'),
(30, 'Centro Educacional do Saber', 'Rua dos Eucaliptos, 2727, Jardim das Acácias'),
(31, 'Instituto de Formação Técnica', 'Avenida da Tecnologia, 2828, Bairro dos Navegantes'),
(32, 'Colégio da Esperança', 'Rua do Horizonte, 2929, Centro'),
(33, 'Instituto de Estudos Técnicos', 'Rua dos Lírios, 3030, Jardim das Rosas'),
(34, 'Escola de Ensino Profissional', 'Avenida da Inovação, 3131, Vila São Jorge'),
(35, 'Centro de Educação de Qualidade', 'Rua das Orquídeas, 3232, Parque das Rosas'),
(36, 'Instituto Técnico do Futuro', 'Avenida das Indústrias, 3333, Bairro dos Pinheiros'),
(37, 'Colégio da Juventude', 'Rua das Flores, 3434, Centro Histórico'),
(38, 'Instituto de Tecnologia e Educação', 'Rua dos Anjos, 3535, Vila Verde'),
(39, 'Escola de Formação Completa', 'Avenida das Nações, 3636, Bairro Jardim'),
(40, 'Centro de Estudos São Pedro', 'Rua da Proclamação, 3737, Jardim das Acácias'),
(41, 'Instituto de Ensino do Brasil', 'Rua das Palmeiras, 3838, Centro'),
(42, 'Colégio do Saber', 'Avenida do Progresso, 3939, Bairro Esperança'),
(43, 'Instituto de Educação Superior', 'Rua dos Eucaliptos, 4040, Jardim das Flores'),
(44, 'Escola Técnica de São Jorge', 'Rua da Alegria, 4141, Vila Nova'),
(45, 'Centro Educacional da Juventude', 'Avenida das Águas, 4242, Centro'),
(46, 'Instituto de Tecnologia Avançada', 'Rua do Desenvolvimento, 4343, Parque das Rosas'),
(47, 'Colégio Profissionalizante', 'Rua das Oliveiras, 4444, Bairro dos Navegantes'),
(48, 'Instituto de Formação e Cultura', 'Avenida do Comércio, 4545, Vila São Jorge'),
(49, 'Escola de Tecnologia e Ciência', 'Rua das Aroeiras, 4646, Jardim das Acácias'),
(50, 'Centro de Ensino e Pesquisa', 'Rua dos Lírios, 4747, Centro Histórico');
```

- Inserir dados na tabela DIPLOMA
```
INSERT INTO Diploma (cp_id_diploma, nome_instituicao, data_conclusao, documento, curso) VALUES
(1, 'Universidade Federal da Bahia', '2007-12-15', '12345', 'Enfermagem'),
(2, 'Universidade de São Paulo', '2012-07-20', '67890', 'Enfermagem'),
(3, 'Universidade Estadual de Campinas', '2005-09-10', '11223', 'Enfermagem Obstétrica'),
(4, 'Universidade Federal do Rio de Janeiro', '2009-05-05', '44556', 'Enfermagem Pediátrica'),
(5, 'Universidade Estadual Paulista', '2017-11-25', '77889', 'Enfermagem de Saúde Pública'),
(6, 'Universidade Federal de Minas Gerais', '2010-06-18', '99001', 'Enfermagem'),
(7, 'Universidade Federal do Paraná', '2015-03-22', '22334', 'Enfermagem'),
(8, 'Universidade Federal do Rio Grande do Sul', '2013-04-15', '55667', 'Enfermagem Obstétrica'),
(9, 'Universidade Federal de Pernambuco', '2011-09-30', '88990', 'Enfermagem Pediátrica'),
(10, 'Universidade Federal do Ceará', '2016-08-20', '11212', 'Enfermagem de Saúde Pública');
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
-iuigi
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

```
ALTER TABLE Pagamento 
ADD COLUMN tipo_pagamento ENUM('PIX', 'Crédito', 'Débito', 'Boleto') NOT NULL;

ALTER TABLE Pagamento 
ADD COLUMN tipo_servico ENUM('Renovação de Carteira', 'Pagamento de Dívidas', 'Emissão de Carteira') NOT NULL;

```

```
ALTER TABLE Processo_Pagamento

ADD FOREIGN KEY (ce_id_profissional) REFERENCES Profissional(ce_id_profissional),
ADD FOREIGN KEY (cp_id_pagamento) REFERENCES Pagamento(cp_id_pagamento);
```
```
INSERT INTO Pagamento (descricao, detalhes, tipo_servico, tipo_pagamento)
VALUES ('Renovação de carteira', 'Pagamento referente à renovação de carteira profissional', 'Renovação de Carteira', 'Boleto');
```
```
DELETE FROM Processo_Pagamento
WHERE cp_id_processo_pagamento = 4;
```
```
UPDATE Processo_Pagamento
SET cp_id_pagamento = 6
WHERE cp_id_processo_pagamento = 3;
```
```
DELETE FROM Pagamento
WHERE cp_id_pagamento IN (49, 50);
```
```
DELETE FROM Instituicao
WHERE cp_id_instituicao IN (49, 50);
```
```DELETE FROM Profissional
WHERE ce_id_profissional IN (1, 2, 3);
```
```
DELETE FROM Profissional
WHERE ce_id_profissional = 4;
```
```
DELETE FROM Profissional
WHERE ce_id_profissional IN (1, 2, 3, 4, 5, 6);
```

## 6. Consultas SQL 1: Primeiras consultas

### Simples

```
-- Buscar todos os profissionais na tabela Profissional:
SELECT * FROM Profissional;

-- Buscar todos os pagamentos do tipo 'Boleto' na tabela Pagamento:
SELECT * FROM Pagamento
WHERE tipo_pagamento = 'Boleto';

-- Buscar todos os processos de pagamento com status 'Pago':
SELECT * FROM Processo_Pagamento
WHERE status = 'Pago';

-- Buscar todos os cursos na tabela Diploma:
SELECT curso FROM Diploma;

```
### Intermediária

```
-- Buscar todos os profissionais e seus pagamentos associados usando joins entre Profissional e Processo_Pagamento:
SELECT p.ce_id_profissional, p.nome, pp.cp_id_pagamento, pp.status
FROM Profissional p
JOIN Processo_Pagamento pp ON p.ce_id_profissional = pp.ce_id_profissional;

-- Buscar todos os pagamentos com descrição e detalhes onde o tipo de serviço é 'Renovação':
SELECT p.descricao, p.detalhes
FROM Pagamento p
WHERE p.tipo_servico = 'Renovação';

-- Buscar todos os profissionais e suas informações de pagamento onde o status do pagamento é 'Pendente':
SELECT p.nome, pp.status
FROM Profissional p
JOIN Processo_Pagamento pp ON p.ce_id_profissional = pp.ce_id_profissional
WHERE pp.status = 'Pendente';

```

### Avançada

```
-- Buscar todos os pagamentos e o status do processo de pagamento para cada profissional, com detalhes adicionais e ordenado por status:
SELECT p.nome AS Profissional, pg.descricao AS Pagamento, pp.status
FROM Profissional p
JOIN Processo_Pagamento pp ON p.ce_id_profissional = pp.ce_id_profissional
JOIN Pagamento pg ON pp.cp_id_pagamento = pg.cp_id_pagamento
ORDER BY pp.status;

-- Buscar o total de pagamentos realizados por cada profissional e a descrição do serviço correspondente:
SELECT p.nome AS Profissional, COUNT(pg.cp_id_pagamento) AS Total_Pagamentos, MAX(pg.descricao) AS Descricao_Ultimo_Pagamento
FROM Profissional p
JOIN Processo_Pagamento pp ON p.ce_id_profissional = pp.ce_id_profissional
JOIN Pagamento pg ON pp.cp_id_pagamento = pg.cp_id_pagamento
GROUP BY p.ce_id_profissional, p.nome;

-- Buscar todos os profissionais que têm pelo menos um pagamento pendente, incluindo os detalhes do pagamento e o tipo de serviço:
SELECT DISTINCT p.nome, pg.descricao, pg.tipo_servico
FROM Profissional p
JOIN Processo_Pagamento pp ON p.ce_id_profissional = pp.ce_id_profissional
JOIN Pagamento pg ON pp.cp_id_pagamento = pg.cp_id_pagamento
WHERE pp.status = 'Pendente';

```

## 7. Desempenho

![imagem1](./img/1.png)
```
EXPLAIN ANALYZE
SELECT * FROM Profissional;
```
![imagem2](./img/2.png)
```
EXPLAIN ANALYZE
SELECT p.nome AS Profissional, pg.descricao AS Pagamento, pp.status
FROM Profissional p
JOIN Processo_Pagamento pp ON p.ce_id_profissional = pp.ce_id_profissional
JOIN Pagamento pg ON pp.cp_id_pagamento = pg.cp_id_pagamento
ORDER BY pp.status;
```

## 8. Tópicos Avançados

Criar uma Stored Procedure
```
DELIMITER //

CREATE PROCEDURE AddProfissional(
    IN p_nome VARCHAR(255),
    IN p_cpf VARCHAR(11),
    IN p_data_nascimento DATE,
    IN p_email VARCHAR(255),
    IN p_telefone VARCHAR(20),
    IN p_endereco VARCHAR(255),
    IN p_id_corenba INT
)
BEGIN
    INSERT INTO Profissional (nome, cpf, data_nascimento, email, telefone, endereco, id_corenba)
    VALUES (p_nome, p_cpf, p_data_nascimento, p_email, p_telefone, p_endereco, p_id_corenba);
END //

DELIMITER ;
```
Backup e preservação de dados

```
1. Backup Completo do Banco de Dados

Para fazer um backup completo de todos os bancos de dados:

bash

mysqldump -u root -p0908 --all-databases > backup_completo.sql

2. Backup de um Banco de Dados Específico

Para fazer um backup de um banco de dados específico, por exemplo, meu_banco:

bash

mysqldump -u root -p0908 meu_banco > backup_meu_banco.sql

3. Backup de uma Tabela Específica

Para fazer um backup de uma tabela específica, por exemplo, minha_tabela no banco meu_banco:

bash

mysqldump -u root -p0908 meu_banco minha_tabela > backup_minha_tabela.sql

4. Restauração de um Banco de Dados

Para restaurar um banco de dados a partir de um backup:

bash

mysql -u root -p0908 meu_banco < backup_meu_banco.sql

5. Restaurar uma Tabela Específica

Para restaurar uma tabela específica a partir de um backup:

bash

mysql -u root -p0908 meu_banco < backup_minha_tabela.sql

6. Backup Incremental

Se você estiver usando logs binários e quiser criar um backup incremental, você precisa de uma configuração adicional. Aqui está um exemplo de como listar os logs binários:

bash

mysqlbinlog --read-from-remote-server --start-datetime="2024-09-01 00:00:00" --stop-datetime="2024-09-02 00:00:00" --user=root --password=0908 > backup_incremental.sql

7. Backup com mysqlpump

Backup completo com mysqlpump:

bash

mysqlpump -u root -p0908 --all-databases > backup_completo_pump.sql

8. Backup com mysqlhotcopy

Se o banco de dados for MyISAM:

bash

mysqlhotcopy meu_banco /caminho/para/backup/

9. Backup com Percona XtraBackup

Para realizar um backup usando Percona XtraBackup:

bash

innobackupex --user=root --password=0908 /caminho/para/backup/


```