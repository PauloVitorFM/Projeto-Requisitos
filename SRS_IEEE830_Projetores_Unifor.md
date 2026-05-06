# Documento SRS – Sistema de Gestão de Empréstimo de Projetores
## Padrão IEEE 830

---

# 1. Introdução

## 1.1 Propósito

Este documento descreve os requisitos de software do Sistema de Gestão de Empréstimo de Projetores do CCT (Centro de Ciências Tecnológicas) da Universidade de Fortaleza – Unifor.

O objetivo do sistema é controlar o processo de empréstimo e devolução de projetores utilizados por professores durante suas aulas, garantindo maior rastreabilidade, organização e eficiência operacional.

Este documento segue como referência o padrão IEEE 830 para especificação de requisitos de software.

---

## 1.2 Escopo

O sistema permitirá:

- Cadastro de professores, funcionários e projetores.
- Consulta de disponibilidade de equipamentos.
- Registro de empréstimos e devoluções.
- Controle de status dos projetores.
- Registro de ocorrências e danos.
- Geração de relatórios administrativos.

A solução será utilizada principalmente por funcionários responsáveis pela sala de equipamentos e pela coordenação do CCT.

---

## 1.3 Definições, Acrônimos e Abreviações

| Termo | Definição |
|---|---|
| CCT | Centro de Ciências Tecnológicas |
| UML | Unified Modeling Language |
| SRS | Software Requirements Specification |
| IEEE 830 | Padrão para documentação de requisitos |
| Stakeholder | Parte interessada no sistema |
| Empréstimo | Processo de retirada temporária do projetor |
| Ocorrência | Registro de problema, atraso ou dano |

---

## 1.4 Referências

- IEEE Std 830-1998 – Recommended Practice for Software Requirements Specifications.
- Documento de visão do projeto.
- Entrevistas realizadas com stakeholders.
- Diagramas UML do sistema.

---

## 1.5 Visão Geral do Documento

Este documento está organizado da seguinte forma:

- Introdução.
- Descrição geral do sistema.
- Requisitos detalhados.
- Revisão de consistência entre UML e requisitos.

---

# 2. Descrição Geral

## 2.1 Perspectiva do Produto

O Sistema de Gestão de Empréstimo de Projetores será uma aplicação responsável por controlar o fluxo de retirada e devolução de equipamentos audiovisuais utilizados pelos professores do CCT.

O sistema substituirá controles manuais e descentralizados atualmente utilizados.

---

## 2.2 Funções do Produto

O sistema deverá:

- Permitir autenticação de usuários.
- Permitir cadastro de professores e projetores.
- Consultar disponibilidade de equipamentos.
- Registrar empréstimos.
- Registrar devoluções.
- Atualizar status dos projetores.
- Registrar ocorrências.
- Gerar relatórios administrativos.
- Consultar histórico de utilização.

---

## 2.3 Características dos Usuários

| Usuário | Características |
|---|---|
| Professor | Solicita utilização de projetores |
| Funcionário responsável | Realiza registros de empréstimo e devolução |
| Coordenação | Consulta relatórios e históricos |
| Administrador | Gerencia usuários e permissões |

---

## 2.4 Restrições Gerais

- O sistema deve operar na infraestrutura da universidade.
- O acesso deve ser restrito a usuários autorizados.
- O sistema deve funcionar em navegadores modernos.
- O sistema deve armazenar registros históricos.
- O sistema deve manter integridade dos dados.

---

## 2.5 Premissas e Dependências

- Existirá um funcionário responsável pela sala de equipamentos.
- Professores possuirão credenciais institucionais válidas.
- Haverá disponibilidade de rede interna para acesso ao sistema.
- Os projetores possuirão identificação única.

---

# 3. Requisitos Detalhados

# 3.1 Requisitos Funcionais

## RF01 – Cadastro de Professores

### Descrição
O sistema deve permitir cadastrar professores autorizados a utilizar projetores.

### Entradas
- Nome
- Matrícula
- Email
- Departamento

### Processamento
O sistema valida os dados e armazena as informações.

### Saídas
Professor cadastrado com sucesso.

---

## RF02 – Cadastro de Projetores

### Descrição
O sistema deve permitir cadastrar projetores disponíveis para empréstimo.

### Entradas
- Código patrimonial
- Modelo
- Status
- Observações

### Saídas
Projetor cadastrado.

---

## RF03 – Consulta de Disponibilidade

### Descrição
O sistema deve exibir projetores disponíveis, emprestados ou em manutenção.

### Entradas
- Consulta do funcionário

### Saídas
Lista de projetores filtrada por status.

---

## RF04 – Registro de Empréstimo

### Descrição
O sistema deve permitir registrar o empréstimo de um projetor.

### Entradas
- Professor
- Projetor
- Data/hora
- Sala

### Regras
- O projetor deve estar disponível.
- O professor deve estar cadastrado.

### Saídas
Registro de empréstimo realizado.

---

## RF05 – Registro de Devolução

### Descrição
O sistema deve registrar a devolução do projetor.

### Entradas
- Identificação do empréstimo
- Estado do equipamento
- Data/hora da devolução

### Saídas
Empréstimo finalizado e status atualizado.

---

## RF06 – Registro de Ocorrências

### Descrição
O sistema deve permitir registrar problemas relacionados ao projetor.

### Entradas
- Tipo da ocorrência
- Descrição
- Data/hora

### Saídas
Ocorrência registrada.

---

## RF07 – Geração de Relatórios

### Descrição
O sistema deve permitir gerar relatórios administrativos.

### Tipos de relatório
- Histórico de empréstimos
- Projetores mais utilizados
- Atrasos
- Equipamentos em manutenção

---

# 3.2 Requisitos Não Funcionais

## RNF01 – Usabilidade
O sistema deve possuir interface simples e intuitiva.

---

## RNF02 – Desempenho
As operações principais devem responder em até 3 segundos.

---

## RNF03 – Segurança
O sistema deve permitir acesso apenas a usuários autenticados.

---

## RNF04 – Disponibilidade
O sistema deve estar disponível durante os horários de aula.

---

## RNF05 – Responsividade
O sistema deve funcionar adequadamente em diferentes resoluções.

---

## RNF06 – Integridade
O sistema não deve permitir perda inconsistente de registros históricos.

---

## RNF07 – Manutenibilidade
O sistema deve possuir arquitetura que facilite manutenção futura.

---

# 3.3 Regras de Negócio

| Código | Regra |
|---|---|
| RN01 | Apenas professores cadastrados podem solicitar projetores |
| RN02 | Apenas funcionários autorizados podem registrar empréstimos |
| RN03 | Projetores indisponíveis não podem ser emprestados |
| RN04 | O status do projetor deve ser atualizado automaticamente |
| RN05 | Projetores danificados devem ser enviados para manutenção |
| RN06 | O histórico de empréstimos não pode ser excluído |
| RN07 | Toda devolução deve possuir data e horário registrados |

---

# 4. Revisão de Consistência

## 4.1 Alinhamento entre UML e Requisitos

Foi realizada uma revisão de consistência para verificar se os diagramas UML representam corretamente os requisitos definidos.

---

## 4.2 Verificação do Diagrama de Casos de Uso

| Caso de Uso | Requisito Relacionado |
|---|---|
| Solicitar projetor | RF04 |
| Consultar disponibilidade | RF03 |
| Registrar empréstimo | RF04 |
| Registrar devolução | RF05 |
| Registrar ocorrência | RF06 |
| Gerar relatórios | RF07 |

Resultado:
Todos os casos de uso possuem correspondência direta com requisitos funcionais.

---

## 4.3 Verificação do Diagrama de Classes

| Classe | Relacionamento com requisitos |
|---|---|
| Professor | RF01 |
| Projetor | RF02 |
| Emprestimo | RF04 e RF05 |
| Ocorrencia | RF06 |
| Funcionario | RF04 e RF05 |

Resultado:
As entidades principais necessárias para implementação dos requisitos estão representadas no diagrama de classes.

---

## 4.4 Verificação do Diagrama de Sequência

### Fluxo de Empréstimo
Representa:
- Consulta de disponibilidade.
- Validação do professor.
- Registro do empréstimo.
- Atualização do status do projetor.

Relacionamento:
RF03 e RF04.

---

### Fluxo de Devolução
Representa:
- Consulta do empréstimo.
- Registro da devolução.
- Atualização do status.
- Registro de ocorrência.

Relacionamento:
RF05 e RF06.

---

## 4.5 Conclusão da Revisão de Consistência

Após análise, conclui-se que os diagramas UML estão alinhados aos requisitos funcionais e regras de negócio do sistema.

Não foram identificadas inconsistências relevantes entre os requisitos especificados e os modelos UML produzidos.

O sistema apresenta coerência estrutural entre:
- Requisitos funcionais.
- Regras de negócio.
- Casos de uso.
- Classes.
- Fluxos de sequência.

---

# 5. Considerações Finais

O documento SRS define formalmente os requisitos do Sistema de Gestão de Empréstimo de Projetores do CCT/Unifor.

A especificação apresentada fornece base para:
- Desenvolvimento do sistema.
- Implementação técnica.
- Validação funcional.
- Criação de testes.
- Evolução futura da solução.

O sistema proposto busca melhorar o controle patrimonial e a eficiência operacional relacionados ao uso de projetores dentro do ambiente acadêmico.
