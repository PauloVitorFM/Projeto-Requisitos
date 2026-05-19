# Levantamento de Requisitos — Sistema de Projetores Fixos por Sala
**Centro de Ciências e Tecnologia (CCT) — Universidade de Fortaleza (UNIFOR)**

> **Versão:** 1.0  
> **Data:** Maio de 2026  
> **Responsável:** Tech Lead — Engenharia de Software  
> **Status:** Em revisão com stakeholders

---

## Sumário

1. [Contexto e Problema](#1-contexto-e-problema)
2. [Stakeholders](#2-stakeholders)
3. [Requisitos Funcionais](#3-requisitos-funcionais)
4. [Requisitos Não-Funcionais](#4-requisitos-não-funcionais)
5. [Regras de Negócio](#5-regras-de-negócio)
6. [Casos de Uso](#6-casos-de-uso)
7. [Roteiro de Entrevistas](#7-roteiro-de-entrevistas)
8. [Restrições e Premissas](#8-restrições-e-premissas)
9. [Glossário](#9-glossário)

---

## 1. Contexto e Problema

### 1.1 Situação atual (AS-IS)

O processo vigente para uso de projetores no CCT funciona da seguinte forma:

1. Professor desloca-se até a sala de controle.
2. Apresenta credenciais a um funcionário.
3. Recebe uma chave física que abre um armário com o projetor.
4. Transporta o equipamento até a sala de aula.
5. Ao término, retorna o projetor e a chave ao funcionário.

### 1.2 Problemas identificados

| # | Problema | Impacto |
|---|----------|---------|
| P1 | Professor carrega equipamento pesado entre salas | Desgaste físico, risco de queda e dano ao equipamento |
| P2 | Dependência de funcionário intermediário | Gargalo operacional; impossível fora do horário de atendimento |
| P3 | Professores perdem a chave física | Custos de reposição; bloqueio de acesso ao equipamento |
| P4 | Ausência de rastreabilidade de uso | Impossível planejar manutenção preventiva |
| P5 | Processo manual e suscetível a erros | Registros inconsistentes; sem auditoria confiável |

### 1.3 Solução proposta (TO-BE)

Instalação de **projetores fixos no teto de cada sala**, controlados remotamente via sistema IoT integrado à base de credenciais institucional da UNIFOR. O professor reserva o acesso à sala (não ao equipamento), e o projetor é ativado e desativado automaticamente. E para manter a integridade do projetor, o mesmo deve ficar dentro de uma "gaiola" com barras metálicas.

---

## 2. Stakeholders

| Papel | Interesse no sistema | Nível de influência |
|---|---|---|
| Professor (usuário primário) | Usar projetor sem burocracia | Alto |
| Coordenador do CCT | Visibilidade de uso e manutenção | Alto |
| Funcionário de TI / infraestrutura | Instalação e manutenção do hardware | Médio |
| Funcionário de atendimento atual | Processo substituído; possível realocação | Alto (resistência) |
| Diretoria da UNIFOR | ROI, conformidade e segurança patrimonial | Alto |
| Alunos | Aulas sem interrupções técnicas | Baixo |

---

## 3. Requisitos Funcionais

### RF-01 — Autenticação via credenciais institucionais

**Prioridade:** Alta  
**Origem:** Coordenação + TI UNIFOR

O sistema deve autenticar professores usando as credenciais já existentes na UNIFOR (login institucional), via protocolo SSO/LDAP. Nenhuma senha adicional deve ser criada.

**Critério de aceite:** Professor consegue autenticar usando o mesmo login do portal acadêmico, sem cadastro separado.

---

### RF-02 — Reserva antecipada de sala com projetor

**Prioridade:** Alta  
**Origem:** Professores

O professor deve poder reservar uma sala equipada com projetor via aplicativo mobile ou portal web, com antecedência mínima de 10 minutos antes do horário de início.

**Critério de aceite:** Sistema confirma a reserva, bloqueia o slot na agenda da sala e envia notificação ao professor.

---

### RF-03 — Acesso presencial por totem na porta

**Prioridade:** Alta  
**Origem:** Professores + Coordenação

Na entrada de cada sala, um totem com leitor NFC deve liberar o projetor ao aproximar o crachá institucional. Como alternativa, o professor pode usar QR code gerado no aplicativo.

**Critério de aceite:** Acesso concedido em menos de 3 segundos após leitura do crachá ou QR code válido.

---

### RF-04 — Ativação automática do projetor

**Prioridade:** Alta  
**Origem:** Professores

Após confirmação do acesso, o projetor fixo instalado no teto da sala deve ligar automaticamente via sinal enviado pelo controlador IoT. O professor não precisa manipular nenhum equipamento físico além do controle remoto para apontar a projeção.

**Critério de aceite:** Projetor ligado e exibindo sinal em até 30 segundos após autenticação.

---

### RF-05 — Encerramento automático por sensor de presença

**Prioridade:** Alta  
**Origem:** TI + Coordenação (economia de energia)

O sistema deve detectar quando a sala permanece vazia por mais de 5 minutos consecutivos e, nesse caso, desligar o projetor automaticamente e registrar a devolução no log.

**Critério de aceite:** Projetor desligado e sessão encerrada automaticamente após 5 minutos sem presença detectada.

---

### RF-06 — Encerramento manual pelo professor

**Prioridade:** Média  
**Origem:** Professores

O professor deve poder encerrar a sessão manualmente via aplicativo, pelo totem da porta ou por botão físico na sala, antes do tempo previsto.

**Critério de aceite:** Sessão encerrada e projetor desligado em até 10 segundos após solicitação manual.

---

### RF-07 — Fallback por código via SMS/e-mail

**Prioridade:** Média  
**Origem:** TI (resiliência)

Caso o totem esteja indisponível, o sistema deve gerar um código temporário (OTP) enviado ao e-mail ou número de celular institucional do professor, válido por 15 minutos, para liberação manual.

**Critério de aceite:** Código entregue em até 60 segundos; acesso liberado corretamente ao informar o código.

---

### RF-08 — Dashboard administrativo

**Prioridade:** Alta  
**Origem:** Coordenação do CCT

Administradores devem ter acesso a painel com: salas em uso em tempo real, histórico de sessões por professor e sala, agendamentos futuros, e alertas de manutenção.

**Critério de aceite:** Dashboard atualiza em tempo real (máximo 30 segundos de defasagem); dados históricos disponíveis com filtro por data, sala e professor.

---

### RF-09 — Alertas automáticos de manutenção

**Prioridade:** Média  
**Origem:** TI + Coordenação

O sistema deve monitorar o total de horas de uso de cada projetor e emitir alerta ao responsável de manutenção quando o equipamento atingir o limiar configurável de horas (padrão: 1.800 horas de lâmpada).

**Critério de aceite:** Alerta enviado por e-mail e exibido no dashboard antes de o equipamento atingir o limite crítico.

---

### RF-10 — Controle de acesso por perfil

**Prioridade:** Alta  
**Origem:** Coordenação + Diretoria

O sistema deve suportar ao menos três perfis: **Professor** (reserva e uso), **Coordenador** (visualização e override), e **Administrador de TI** (configuração completa, incluindo criação de salas e equipamentos).

**Critério de aceite:** Cada perfil acessa apenas as funcionalidades descritas; tentativa de acesso não autorizado retorna erro 403 com log de auditoria.

---

## 4. Requisitos Não-Funcionais

### RNF-01 — Disponibilidade

O sistema deve estar disponível em 99,5% do tempo durante o horário letivo (segunda a sábado, 07h–22h). O hardware de cada sala deve operar em **modo offline** com cache local de autorizações por até 4 horas, garantindo funcionamento mesmo sem conectividade com o servidor central.

---

### RNF-02 — Desempenho

- Autenticação via totem: resposta em até **3 segundos**.
- Ativação do projetor: equipamento operacional em até **30 segundos**.
- Carregamento do dashboard: abaixo de **2 segundos** para visualização inicial.

---

### RNF-03 — Segurança

- Toda comunicação entre totem e servidor central deve usar **TLS 1.3**.
- Credenciais nunca devem trafegar em texto plano.
- Tokens de sessão devem ter validade máxima de 8 horas.
- Tentativas de acesso inválido devem ser logadas com timestamp, sala e identidade tentada.

---

### RNF-04 — Conformidade com LGPD

- Logs de acesso contendo dados pessoais devem ser **anonimizados após 180 dias**.
- O sistema deve disponibilizar funcionalidade de exportação dos dados de um usuário mediante solicitação (direito de acesso, Art. 18 LGPD).
- Dados não devem ser repassados a terceiros sem consentimento explícito.

---

### RNF-05 — Escalabilidade

A arquitetura deve suportar expansão para outros centros da UNIFOR (CCJS, CCS, etc.) sem necessidade de reescrita. Novos campi ou salas devem ser configuráveis via painel administrativo.

---

### RNF-06 — Manutenibilidade

- O código deve seguir padrões documentados (README, ADRs).
- Cada controlador IoT deve suportar atualização de firmware remota (OTA) sem visita presencial.
- Logs do sistema devem ser centralizados e pesquisáveis.

---

### RNF-07 — Usabilidade

- O aplicativo mobile deve ser utilizável por professores sem treinamento, com no máximo **3 toques** para reservar uma sala.
- Interface deve estar disponível em **português brasileiro**.
- O totem deve exibir feedback visual claro (luz verde = acesso liberado, vermelha = negado).

---

## 5. Regras de Negócio

| ID | Regra |
|----|-------|
| RN-01 | Um professor pode ter no máximo **2 salas com sessão ativa** simultaneamente. |
| RN-02 | Sala reservada e não utilizada em **15 minutos** após o horário de início tem a reserva cancelada automaticamente e o slot é liberado. |
| RN-03 | Projetor com mais de **1.800 horas de uso de lâmpada** entra em modo restrito até manutenção registrada. |
| RN-04 | Administradores podem **revogar acesso remotamente** a qualquer momento, encerrando a sessão ativa da sala. |
| RN-05 | Professores afastados ou com vínculo inativo no sistema da UNIFOR **não podem realizar reservas**. |
| RN-06 | Reservas podem ser feitas com no máximo **7 dias de antecedência**. |
| RN-07 | Em caso de sobreposição de reservas, prevalece a **primeira registrada**. Sistemas de grade horária do SIGAA têm prioridade sobre reservas avulsas. |
| RN-08 | O cancelamento de reserva deve ser feito com pelo menos **30 minutos de antecedência**; caso contrário, o evento é registrado como "não comparecimento". |

---

## 6. Casos de Uso

### Diagrama de Casos de Uso

```
+-----------------------------------------------------------+
|              Sistema de Projetores CCT/UNIFOR             |
|                                                           |
|   [UC-01] Reservar sala com projetor                      |
|   [UC-02] Acessar sala via totem (NFC/QR)                 |
|   [UC-03] Ativar projetor automaticamente                 |
|   [UC-04] Encerrar sessão (manual ou automático)          |
|   [UC-05] Solicitar acesso por fallback (OTP)             |
|   [UC-06] Visualizar dashboard administrativo             |
|   [UC-07] Gerenciar salas e equipamentos                  |
|   [UC-08] Consultar histórico de uso                      |
|   [UC-09] Receber alerta de manutenção                    |
|   [UC-10] Revogar acesso remotamente                      |
|                                                           |
+-----------------------------------------------------------+

Atores:
  👤 Professor       → UC-01, UC-02, UC-03, UC-04, UC-05
  🛡️  Coordenador    → UC-06, UC-08, UC-10
  ⚙️  Admin de TI    → UC-06, UC-07, UC-08, UC-09, UC-10
  🤖 Sistema (IoT)  → UC-03, UC-04, UC-09
```

---

### UC-01 — Reservar sala com projetor

| Campo | Descrição |
|---|---|
| **Ator principal** | Professor |
| **Pré-condição** | Professor autenticado no app/portal; sala disponível no horário desejado |
| **Fluxo principal** | 1. Professor acessa app e seleciona "Reservar sala" |
| | 2. Sistema exibe calendário de disponibilidade por sala |
| | 3. Professor seleciona sala, data, hora de início e hora de término |
| | 4. Sistema valida: disponibilidade, antecedência mínima (10 min), limite de reservas ativas (máx. 2) |
| | 5. Sistema confirma reserva e envia notificação ao professor |
| **Fluxo alternativo** | 4a. Sala indisponível → sistema sugere salas alternativas no mesmo horário |
| | 4b. Professor já tem 2 reservas ativas → sistema informa restrição |
| **Pós-condição** | Reserva registrada; slot bloqueado na agenda da sala |

---

### UC-02 — Acessar sala via totem

| Campo | Descrição |
|---|---|
| **Ator principal** | Professor |
| **Pré-condição** | Reserva ativa para a sala no período corrente (±15 min do horário) |
| **Fluxo principal** | 1. Professor aproxima crachá NFC do totem na porta |
| | 2. Totem lê identificação e envia ao sistema central |
| | 3. Sistema valida reserva ativa para aquele professor naquela sala |
| | 4. Sistema retorna autorização; totem exibe luz verde |
| | 5. Sistema aciona controlador IoT da sala (RF-04) |
| **Fluxo alternativo** | 1a. Professor usa QR code do aplicativo em vez do crachá |
| | 3a. Sem reserva → sistema verifica se a sala está disponível; se sim, cria sessão avulsa |
| | 3b. Acesso negado → totem exibe luz vermelha e motivo no display |
| **Pós-condição** | Sessão iniciada; projetor acionado; log de acesso criado |

---

### UC-03 — Ativar projetor automaticamente

| Campo | Descrição |
|---|---|
| **Ator principal** | Sistema IoT |
| **Pré-condição** | Acesso à sala autorizado (UC-02) |
| **Fluxo principal** | 1. Sistema central envia comando MQTT ao controlador da sala |
| | 2. Controlador IoT aciona saída de controle do projetor (via HDMI-CEC ou relé IR) |
| | 3. Projetor liga e exibe tela de standby em até 30 segundos |
| | 4. Controlador confirma ativação ao sistema central |
| **Fluxo alternativo** | 4a. Sem confirmação em 60 segundos → sistema registra falha e notifica admin de TI |
| **Pós-condição** | Projetor operacional; status "em uso" registrado no dashboard |

---

### UC-04 — Encerrar sessão

| Campo | Descrição |
|---|---|
| **Atores** | Professor (manual) / Sistema IoT (automático) |
| **Pré-condição** | Sessão ativa na sala |
| **Fluxo principal (manual)** | 1. Professor toca em "Encerrar sessão" no app ou no totem de saída |
| | 2. Sistema desliga projetor via MQTT |
| | 3. Sistema registra horário de encerramento e duração total |
| **Fluxo principal (automático)** | 1. Sensor de presença detecta sala vazia por 5 minutos consecutivos |
| | 2. Sistema encerra sessão automaticamente (igual ao fluxo manual, a partir do passo 2) |
| **Pós-condição** | Sessão encerrada; sala disponível para novas reservas; consumo de energia registrado |

---

### UC-05 — Solicitar acesso por fallback (OTP)

| Campo | Descrição |
|---|---|
| **Ator principal** | Professor |
| **Pré-condição** | Totem indisponível; professor possui reserva ativa |
| **Fluxo principal** | 1. Professor acessa app e solicita "Código de emergência" |
| | 2. Sistema gera OTP de 6 dígitos, válido por 15 minutos |
| | 3. OTP enviado ao e-mail institucional e SMS do professor |
| | 4. Professor digita OTP no painel físico da sala |
| | 5. Sistema valida e libera acesso normalmente |
| **Pós-condição** | Sessão iniciada; evento de fallback registrado para auditoria |

---

### UC-06 — Visualizar dashboard administrativo

| Campo | Descrição |
|---|---|
| **Atores** | Coordenador, Admin de TI |
| **Pré-condição** | Usuário autenticado com perfil Coordenador ou Admin |
| **Fluxo principal** | 1. Usuário acessa painel administrativo |
| | 2. Sistema exibe: salas em uso em tempo real, alertas ativos, ocupação do dia |
| | 3. Usuário pode filtrar por sala, professor ou período |
| | 4. Sistema renderiza dados atualizados (máx. 30 s de defasagem) |
| **Pós-condição** | Nenhuma alteração de estado; apenas leitura |

---

### UC-07 — Gerenciar salas e equipamentos

| Campo | Descrição |
|---|---|
| **Ator principal** | Admin de TI |
| **Pré-condição** | Usuário autenticado com perfil Admin |
| **Fluxo principal** | 1. Admin acessa módulo de configuração |
| | 2. Cadastra nova sala informando: número, bloco, capacidade, ID do controlador IoT |
| | 3. Associa projetor à sala informando: modelo, número de série, horas iniciais de lâmpada |
| | 4. Sistema valida e salva configuração; controlador IoT recebe configuração via OTA |
| **Pós-condição** | Sala e equipamento disponíveis para reserva e uso |

---

### UC-09 — Receber alerta de manutenção

| Campo | Descrição |
|---|---|
| **Atores** | Sistema IoT, Admin de TI |
| **Pré-condição** | Projetor acumulou horas próximas ou acima do limiar configurado |
| **Fluxo principal** | 1. Sistema detecta que projetor atingiu 90% do limiar de horas (1.620 h) |
| | 2. Alerta preventivo enviado por e-mail ao Admin de TI e exibido no dashboard |
| | 3. Ao atingir 100% (1.800 h), projetor entra em modo restrito (funciona mas exibe aviso) |
| | 4. Admin registra manutenção realizada; sistema zera contador e restaura modo normal |
| **Pós-condição** | Ciclo de manutenção registrado; vida útil do equipamento rastreada |

---

### UC-10 — Revogar acesso remotamente

| Campo | Descrição |
|---|---|
| **Atores** | Coordenador, Admin de TI |
| **Pré-condição** | Sessão ativa em uma sala |
| **Fluxo principal** | 1. Admin ou Coordenador localiza sessão ativa no dashboard |
| | 2. Seleciona "Revogar acesso" com motivo obrigatório |
| | 3. Sistema envia comando de desligamento ao controlador IoT da sala |
| | 4. Projetor desligado; professor recebe notificação no app com motivo |
| | 5. Evento registrado em log de auditoria |
| **Pós-condição** | Sessão encerrada; sala liberada; registro de auditoria criado |

---

## 7. Roteiro de Entrevistas

> Este roteiro deve ser aplicado antes da validação final dos requisitos. Recomenda-se conduzir entrevistas individuais de 30–45 minutos com ao menos 2 representantes de cada perfil.

---

### 7.1 Entrevista com Professores

**Objetivo:** Entender o fluxo real de uso, pontos de dor e expectativas de usabilidade.

**Perfil do entrevistado:** Professor do CCT com pelo menos 1 semestre usando o sistema atual.

---

**Bloco 1 — Contexto de uso atual**

1. Com que frequência você utiliza projetor nas suas aulas? Em quantas salas diferentes você costuma dar aula por semana?
2. Me descreva como é o seu processo hoje, do momento que você decide que vai usar o projetor até o momento que você devolve.
3. Qual é a parte desse processo que mais te incomoda ou atrasa? Por quê?
4. Já aconteceu alguma situação em que você não conseguiu usar o projetor quando precisava? O que aconteceu?
5. Você já perdeu a chave ou teve dificuldade com ela? Como foi resolvido?

---

**Bloco 2 — Expectativas sobre o novo sistema**

6. Se você pudesse fazer um pedido para melhorar esse processo, qual seria?
7. O que seria inaceitável para você em um novo sistema — algo que, se acontecesse, seria pior do que o sistema atual?
8. Você prefere reservar a sala com antecedência ou prefere chegar e usar no improviso? Por quê?
9. Com que frequência você usa o crachá institucional no dia a dia? Você o carrega sempre?
10. Você tem smartphone? Usaria um aplicativo para reservar salas? O que ele precisaria ter para ser útil de verdade?

---

**Bloco 3 — Requisitos de resiliência**

11. O que você faria se chegasse na sala e o sistema não estivesse funcionando? Qual seria a sua expectativa?
12. Se o projetor ligasse automaticamente quando você entrasse, sem precisar tocar em nada, isso seria bom ou causaria algum problema?
13. Você teria alguma preocupação com privacidade ou registro das suas aulas no sistema?

---

**Bloco 4 — Encerramento**

14. Tem alguma coisa que eu não perguntei e que você acha importante para esse sistema funcionar bem?
15. Você conhece algum professor que usa o projetor de forma muito diferente da sua? Vale a pena eu conversar com ele?

---

### 7.2 Entrevista com o Coordenador do CCT

**Objetivo:** Entender necessidades de gestão, visibilidade operacional e restrições institucionais.

**Perfil do entrevistado:** Coordenador ou vice-coordenador do CCT.

---

**Bloco 1 — Gestão operacional atual**

1. Como você acompanha hoje o uso dos projetores? Há algum registro ou controle formal?
2. Com que frequência ocorrem problemas relacionados aos projetores (perdas, danos, manutenção atrasada)? Você tem algum dado sobre isso?
3. Quantos projetores o CCT possui atualmente? Quantas salas são equipadas?
4. Existe algum contrato de manutenção ou garantia vigente para os equipamentos?

---

**Bloco 2 — Expectativas sobre o novo sistema**

5. Quais informações você precisaria ver em um painel de gestão para se sentir confortável com o controle do uso dos projetores?
6. Há situações em que você precisaria bloquear o acesso de um professor específico? Como você imagina que isso funcionaria?
7. Como você gostaria de ser notificado quando um equipamento precisar de manutenção?
8. Existe alguma política interna da UNIFOR que o sistema precisa respeitar (ex: normas de segurança patrimonial, acesso de visitantes)?

---

**Bloco 3 — Restrições e riscos**

9. Qual é o seu maior medo em relação a um sistema automatizado? O que poderia dar errado?
10. Existe resistência de alguma área da universidade (TI central, patrimonial, RH) que precisamos considerar?
11. Há professores substitutos, visitantes ou turmas especiais que precisam de tratamento diferente?

---

### 7.3 Entrevista com o Funcionário de Atendimento Atual

**Objetivo:** Mapear exceções do processo atual que não são visíveis de fora; mitigar resistência à mudança.

**Perfil do entrevistado:** Funcionário responsável pelo controle de chaves e projetores.

---

1. Me conta como é o seu dia a dia com os projetores. Quais são os horários de maior movimento?
2. Quais são as situações mais difíceis ou incomuns que você já precisou resolver?
3. Quando um professor perde a chave, qual é o processo? Já aconteceu de ficarem sem projetor por causa disso?
4. Existe alguma regra informal que você aplica mas que não está escrita em nenhum lugar?
5. Há professores que têm necessidades especiais de acesso (ex: acesso fora do horário, equipamentos extras)?
6. Se esse processo for automatizado, você imagina alguma situação em que ainda seria necessária a sua intervenção?
7. O que você acha que o sistema novo precisa ter para não gerar confusão ou problemas?

---

### 7.4 Entrevista com o Time de TI da UNIFOR

**Objetivo:** Entender restrições de infraestrutura, integração com sistemas existentes e requisitos de segurança.

**Perfil do entrevistado:** Analista ou coordenador de TI da universidade.

---

1. Qual é a infraestrutura de rede disponível nos blocos do CCT? Há Wi-Fi confiável em todas as salas?
2. A UNIFOR possui um serviço de diretório (LDAP/Active Directory) para autenticação? É acessível via API?
3. Existe um sistema de gestão de espaços ou reservas de salas já em uso (ex: SIGAA, sistemas internos)? Como funciona a integração?
4. Quais são as políticas de segurança de TI que um sistema IoT precisa respeitar na rede da universidade?
5. Existe infraestrutura de servidores on-premise disponível? Ou seria necessário usar cloud?
6. Há restrições para instalação de hardware nas salas (cabeamento, alimentação elétrica, fixação no teto)?
7. Quem seria responsável pelo suporte de primeiro nível em caso de falha do sistema?
8. Existe algum sistema legado com o qual obrigatoriamente precisamos integrar?

---

## 8. Restrições e Premissas

### Restrições

- **R1:** O sistema deve ser hospedado em infraestrutura da própria UNIFOR (on-premise), por exigência de conformidade de dados institucionais.
- **R2:** A integração com o diretório de usuários deve respeitar as APIs já disponibilizadas pelo setor de TI central; não é permitido acesso direto ao banco de dados de autenticação.
- **R3:** O orçamento de instalação de hardware deve ser validado pela Diretoria antes do início da fase de implantação.
- **R4:** O sistema não pode interferir com o sistema de controle de acesso às salas já existente (fechaduras, câmeras).

### Premissas

- **P1:** Todos os professores do CCT possuem crachá institucional com chip NFC.
- **P2:** Há tomadas elétricas acessíveis no teto ou em posição adequada para instalação dos projetores fixos.
- **P3:** A rede Wi-Fi institucional cobre todas as salas do CCT com sinal suficiente para comunicação IoT.
- **P4:** O sistema de grade horária (SIGAA ou equivalente) pode exportar dados de alocação de salas via API ou arquivo estruturado.

---

## 9. Glossário

| Termo | Definição |
|---|---|
| **Sessão** | Período entre a ativação do projetor pelo professor e o encerramento (manual ou automático) |
| **Totem** | Dispositivo físico instalado na entrada de cada sala, com leitor NFC, display e indicador visual |
| **Controlador IoT** | Microcomputador (ex: Raspberry Pi Zero 2W) instalado em cada sala, responsável por acionar o projetor e ler sensores |
| **OTP** | One-Time Password — código numérico de uso único, válido por tempo limitado, usado como mecanismo de fallback |
| **SSO** | Single Sign-On — mecanismo que permite ao usuário autenticar uma vez e acessar múltiplos sistemas |
| **LDAP** | Lightweight Directory Access Protocol — protocolo usado para consultar diretórios de usuários corporativos/institucionais |
| **MQTT** | Message Queuing Telemetry Transport — protocolo de mensagens leve, amplamente usado em sistemas IoT |
| **OTA** | Over-the-Air — atualização de firmware de dispositivos remotamente, sem necessidade de acesso físico |
| **NFC** | Near Field Communication — tecnologia de comunicação sem fio de curto alcance, presente em crachás e smartphones |
| **HDMI-CEC** | Canal de controle de dispositivos HDMI que permite ligar/desligar equipamentos de forma integrada |
| **SIGAA** | Sistema Integrado de Gestão de Atividades Acadêmicas — sistema de gestão acadêmica usado em diversas IES brasileiras |
| **LGPD** | Lei Geral de Proteção de Dados — Lei nº 13.709/2018, que regulamenta o tratamento de dados pessoais no Brasil |

---

*Documento elaborado com base em análise de processo e boas práticas de engenharia de requisitos. Deve ser revisado e validado com os stakeholders antes de iniciar o desenvolvimento.*
