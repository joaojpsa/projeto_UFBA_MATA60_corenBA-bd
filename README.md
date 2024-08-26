# Projeto - Elaboração, Implantação, Governança e Uso de Banco de Dados em Estudo de Caso

## MATA60 - Banco de Dados
Prof. Robespierre Pita
## 1 Modelando a Base de Dados

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
## 1.3 Delimitação do Mini-Mundo para o Banco de Dados

### **tbl_profissional**
Tabela que concentra informações sobre os profissionais de enfermagem cadastrados.

- **cp_id_profissional** `[int, incremental]`: Código identificador do profissional.
- **nome_completo** `[str, 200 caracteres]`: Nome completo do profissional.
- **cpf** `[str, 11 caracteres]`: CPF do profissional. Único.
- **data_nascimento** `[date]`: Data de nascimento do profissional.
- **sexo** `[str, 1 caractere]`: Sexo do profissional.
- **email** `[str, 100 caracteres]`: Email do profissional.
- **telefone** `[str, 15 caracteres]`: Telefone do profissional.
- **registro_coren** `[str, 20 caracteres]`: Número de registro no COREN. Único.
- **data_inscricao** `[date]`: Data de inscrição no COREN.
- **conselho_regional** `[str, 50 caracteres]`: Conselho regional do profissional.
- **situacao_profissional** `[str, 20 caracteres]`: Situação profissional (ativo, suspenso, inativo, cancelado).

### **tbl_instituicao**
Tabela que concentra informações sobre as instituições de ensino.

- **cp_id_instituicao** `[int, incremental]`: Código identificador da instituição. Único e incremental.
- **nome_instituicao** `[str, 100 caracteres]`: Nome da instituição de ensino.
- **endereco** `[str, 200 caracteres]`: Endereço da instituição de ensino.
- **telefone** `[str, 15 caracteres]`: Telefone da instituição de ensino.
- **email** `[str, 100 caracteres]`: Email da instituição de ensino.

### **tbl_diploma**
Tabela que concentra informações sobre os diplomas dos profissionais de enfermagem.

- **cp_id_diploma** `[int, incremental]`: Código identificador do diploma. Único e incremental.
- **ce_id_profissional** `[int, 8 bytes]`: Chave estrangeira que define o profissional de enfermagem.
- **ce_id_instituicao** `[int, 8 bytes]`: Chave estrangeira que define a instituição de ensino.
- **curso** `[str, 100 caracteres]`: Nome do curso.
- **tipo_diploma** `[str, 50 caracteres]`: Tipo de diploma.
- **data_concessao** `[date]`: Data de concessão do diploma.

### **tbl_pagamento**
Tabela que concentra informações sobre os pagamentos realizados pelos profissionais.

- **cp_id_pagamento** `[int, incremental]`: Código identificador do pagamento. Único e incremental.
- **ce_id_profissional** `[int, 8 bytes]`: Chave estrangeira que define o profissional de enfermagem.
- **ano_referencia** `[int]`: Ano de referência do pagamento.
- **valor_pago** `[decimal, 10, 2]`: Valor pago pelo profissional.
- **data_pagamento** `[date]`: Data do pagamento.
- **forma_pagamento** `[str, 50 caracteres]`: Forma de pagamento.
- **status_pagamento** `[str, 20 caracteres]`: Status do pagamento (pago, pendente, atrasado).

### **tbl_processo**
Tabela que concentra informações sobre os processos envolvendo profissionais de enfermagem.

- **cp_id_processo** `[int, incremental]`: Código identificador do processo. Único e incremental.
- **ce_id_profissional** `[int, 8 bytes]`: Chave estrangeira que define o profissional de enfermagem.
- **tipo_processo** `[str, 50 caracteres]`: Tipo de processo (ético, administrativo, disciplinar).
- **numero_processo** `[str, 50 caracteres]`: Número do processo.
- **data_abertura** `[date]`: Data de abertura do processo.
- **conselho_responsavel** `[str, 50 caracteres]`: Conselho regional responsável pelo processo.
- **descricao** `[text]`: Descrição do caso.

### **tbl_profissional_processo**
- **ce_id_profissional** `[int, 8 bytes]`: Chave estrangeira que define o profissional de enfermagem.
- **ce_id_processo** `[int, 8 bytes]`: Chave estrangeira que define o processo.
- **data_inclusao** `[date]`: Data de inclusão do profissional no processo.
- **funcao_no_processo** `[str, 100 caracteres]`: Função do profissional no processo.

  Detalha a participação do profissional em um processo específico:
  - **Testemunha**: O profissional pode ser chamado para prestar depoimento sobre um determinado caso.
  - **Acusado**: O profissional está sendo investigado ou acusado de alguma infração ou má conduta.
  - **Representante Legal**: O profissional atua como representante legal ou advogado de outro profissional envolvido no processo.
  - **Investigador**: O profissional faz parte de uma comissão que investiga o caso.
  - **Perito**: O profissional atua como perito, fornecendo uma análise técnica ou especializada sobre o caso em questão.
  - **Parte Interessada**: O profissional tem um interesse direto no desfecho do processo, mas não necessariamente está acusado ou testemunhando.

### **tbl_especialidade**
Tabela que concentra informações sobre as especialidades dos profissionais de enfermagem.

- **cp_id_especialidade** `[int, incremental]`: Código identificador da especialidade. Único e incremental.
- **ce_id_profissional** `[int, 8 bytes]`: Chave estrangeira que define o profissional de enfermagem.
- **area_atuacao** `[str, 100 caracteres]`: Área de atuação.
- **data_conclusao** `[date]`: Data de conclusão da especialidade.

### **tbl_usuario**
Tabela que concentra informações sobre os usuários que controlam o sistema.

- **cp_id_usuario** `[int, incremental]`: Código identificador do usuário. Único.
- **nome_usuario** `[str, 100 caracteres]`: Nome do usuário.
- **senha** `[str, 255 caracteres]`: Senha do usuário.
- **perfil_acesso** `[str, 50 caracteres]`: Perfil de acesso do usuário.

### **tbl_atendimento**
Tabela para manutenção do histórico de atendimentos realizados para cada profissional.

- **cp_id_atendimento** `[int, incremental]`: Código identificador do atendimento. Único e incremental.
- **ce_id_profissional** `[int, 8 bytes]`: Chave estrangeira que define o profissional de enfermagem.
- **data_hora** `[datetime]`: Data e hora do atendimento.
- **detalhes** `[text]`: Detalhes do atendimento realizado.
- **ce_id_usuario** `[int, 8 bytes]`: Chave estrangeira que define o usuário (atendente) que realizou o atendimento.

### **tbl_agendamento**
- **cp_id_agendamento** `[int, incremental]`: Código identificador do agendamento. Único e incremental.
- **ce_id_profissional** `[int, 8 bytes]`: Chave estrangeira que define o profissional de enfermagem.
- **data_hora** `[datetime]`: Data e hora agendada para o atendimento.
- **motivo** `[text]`: Motivo do agendamento.
- **ce_id_usuario** `[int, 8 bytes]`: Chave estrangeira que define o usuário que realizou o agendamento.

### **tbl_conselho_regional**
Tabela que concentra informações sobre o conselho regional de enfermagem (COREN-BA).

- **cp_id_conselho** `[int, incremental]`: Código identificador do conselho. Único.
- **nome** `[str, 100 caracteres]`: Nome do conselho.
- **endereco** `[str, 200 caracteres]`: Endereço do conselho.
- **telefone** `[str, 15 caracteres]`: Telefone do conselho.
- **email** `[str, 100 caracteres]`: Email do conselho.
- **website** `[str, 100 caracteres]`: Website do conselho.

### **tbl_cofen**
Tabela que concentra informações sobre o Conselho Federal de Enfermagem (COFEN).

- **cp_id_cofen** `[int, incremental]`: Código identificador do COFEN. Único.
- **nome** `[str, 100 caracteres]`: Nome do COFEN.
- **endereco** `[str, 200 caracteres]`: Endereço do COFEN.
- **telefone** `[str, 15 caracteres]`: Telefone do COFEN.
- **email** `[str, 100 caracteres]`: Email do COFEN.
- **website** `[str, 100 caracteres]`: Website do COFEN.

## Relações entre as Entidades

### Relação entre `tbl_profissional` e `tbl_diploma`
- Um profissional pode ter um ou vários diplomas.
- Cada diploma pertence a um único profissional.

### Relação entre `tbl_diploma` e `tbl_instituicao`
- Um diploma é emitido por uma única instituição.
- Uma instituição pode emitir vários diplomas.
- As relações entre diplomas e instituições devem registrar o curso e a data de concessão.

### Relação entre `tbl_pagamento` e `tbl_profissional`
- Um profissional pode ter nenhum ou vários pagamentos.
- Cada pagamento pertence a um único profissional.
- As relações entre pagamentos e profissionais devem registrar o ano de referência, valor pago, data de pagamento, forma de pagamento e status do pagamento.

### Relação entre `tbl_processo` e `tbl_profissional_processo`
- Um profissional pode estar envolvido em vários processos.
- Um processo pode envolver um ou mais profissionais.
- As relações entre processos e profissionais devem registrar a função do profissional no processo e a data de inclusão.

### Relação entre `tbl_profissional` e `tbl_especialidade`
- Um profissional pode ter várias especialidades.
- Cada especialidade pertence a um único profissional.

### Relação entre `tbl_profissional` e `tbl_atendimento`
- Um profissional pode ter um ou vários atendimentos.
- Cada atendimento pertence a um único profissional.
- As relações entre atendimentos e profissionais devem registrar a data, hora e detalhes do atendimento.

### Relação entre `tbl_atendimento` e `tbl_usuario`
- Um atendimento é realizado por um único usuário.
- Um usuário pode realizar vários atendimentos.

### Relação entre `tbl_agendamento` e `tbl_profissional`
- Um profissional pode ter um ou vários agendamentos.
- Cada agendamento pertence a um único profissional.
- As relações entre agendamentos e profissionais devem registrar a data, hora e motivo do agendamento.

### Relação entre `tbl_agendamento` e `tbl_usuario`
- Um agendamento é realizado por um único usuário.
- Um usuário pode realizar vários agendamentos.

### Relação entre `tbl_conselho_regional` e `tbl_profissional`
- Um profissional pertence a um único conselho regional.
- Um conselho regional pode ter vários profissionais associados.
