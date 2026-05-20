# Especificação de Requisitos Funcionais

## Caso de Uso (CDU) - Sistema de Empréstimo de Projetores — CCT/Unifor

## Histórico de Versões

<!-- markdownlint-disable MD060 -->
| Data       | Versão | Descrição                                      | Autor             |
| ---------- | ------- | ---------------------------------------------- | ----------------- |
| 20/05/2026 | 1.0     | Criação inicial do CDU-01                      | Equipe do Projeto |
|            |         |                                                |                   |
|            |         |                                                |                   |
<!-- markdownlint-enable MD060 -->

## 1. Nome do Caso de Uso

Registrar Empréstimo de Projetor

---

## 2. Objetivo

Permitir que o funcionário do CCT registre o empréstimo de um projetor a um professor, realizando a identificação automática do professor a partir do seu nome e o preenchimento automático do formulário virtual de empréstimo.

---

## 3. Tipo de Caso de Uso

Concreto.

---

## 4. Atores

### 4.1 Primário

**Funcionário do CCT** — responsável por operar o sistema e registrar o empréstimo no momento em que o professor solicita o equipamento presencialmente.

### 4.2 Secundário

**Sistema Acadêmico da Unifor** — fornece automaticamente os dados cadastrais do professor (matrícula, disciplinas lecionadas no semestre vigente) a partir do nome informado.

---

## 5. Precondições

- O funcionário está autenticado no sistema.
- Existe ao menos um projetor disponível (não emprestado) no estoque do CCT.
- O professor solicitante está cadastrado na base acadêmica da Unifor.

---

## 6. Fluxo Principal

### P1. Professor solicita um projetor ao funcionário

O professor se dirige ao setor de empréstimos do CCT e informa verbalmente seu nome ao funcionário.

### P2. Funcionário acessa a tela de novo empréstimo

O funcionário seleciona a opção "Novo Empréstimo" na tela inicial do sistema.

### P3. Funcionário informa o nome do professor

O funcionário digita o nome do professor no campo de busca do formulário de empréstimo.

### P4. Sistema localiza o professor na base acadêmica

O sistema consulta o Sistema Acadêmico da Unifor e retorna os dados do professor correspondente ao nome informado: nome completo, matrícula e disciplinas lecionadas no semestre vigente.

### P5. Sistema preenche automaticamente o formulário virtual

O sistema exibe o formulário de empréstimo com os campos já preenchidos: nome completo, matrícula, disciplina(s) e data/hora da solicitação (gerada automaticamente pelo sistema).

### P6. Funcionário confirma os dados e seleciona o projetor

O funcionário verifica os dados exibidos com o professor, seleciona um projetor disponível na lista apresentada pelo sistema e confirma o empréstimo.

### P7. Sistema registra o empréstimo

O sistema salva o registro do empréstimo no banco de dados, associando o projetor selecionado ao professor, e marca o equipamento como "emprestado".

### P8. Sistema exibe confirmação do empréstimo

O sistema exibe uma mensagem de confirmação com os dados do empréstimo registrado (professor, projetor, data e hora).

### P9. Funcionário entrega o projetor ao professor

O funcionário busca o projetor correspondente e o entrega ao professor, concluindo o atendimento.

---

## 7. Fluxos Alternativos

### A1. Nome do professor retorna mais de um resultado

#### A1.1. No passo P4, o sistema encontra mais de um professor com o nome informado.

#### A1.2. O sistema exibe uma lista com os resultados encontrados, contendo nome completo e matrícula de cada professor.

#### A1.3. O funcionário seleciona o professor correto na lista, confirmando com o próprio professor.

#### A1.4. O caso de uso retorna ao passo P5.

---

### A2. Professor possui empréstimo em aberto

#### A2.1. No passo P4, após localizar o professor, o sistema identifica que ele possui um ou mais projetores emprestados e ainda não devolvidos.

#### A2.2. O sistema exibe um alerta informando os detalhes do(s) empréstimo(s) em aberto (projetor, data do empréstimo).

#### A2.3. O funcionário decide, conforme critério institucional, se prossegue ou cancela o novo empréstimo.

#### A2.4. Se o funcionário decidir prosseguir, o caso de uso retorna ao passo P5. Se cancelar, o caso de uso é encerrado sem registro.

---

### A3. Estoque com apenas um projetor disponível

#### A3.1. No passo P6, o sistema exibe apenas um projetor disponível na lista.

#### A3.2. O funcionário confirma o empréstimo sem necessidade de seleção adicional.

#### A3.3. O caso de uso segue para o passo P7.

---

## 8. Fluxos de Exceção

### E1. Professor não encontrado na base acadêmica

#### E1.1. No passo P4, o sistema não localiza nenhum professor com o nome informado.

#### E1.2. O sistema exibe a mensagem: *"Nenhum professor encontrado com esse nome. Tente usar apenas o sobrenome ou verifique a grafia."*

#### E1.3. O funcionário solicita ao professor que confirme seu nome completo ou matrícula.

#### E1.4. O funcionário realiza uma nova busca. Se o professor for encontrado, o caso de uso retorna ao passo P4. Se não for encontrado após nova tentativa, o empréstimo não pode ser registrado e o funcionário encerra o atendimento sem registro.

---

### E2. Nenhum projetor disponível no estoque

#### E2.1. No passo P6, o sistema não apresenta nenhum projetor disponível na lista.

#### E2.2. O sistema exibe a mensagem: *"Não há projetores disponíveis no momento."*

#### E2.3. O funcionário informa ao professor sobre a indisponibilidade e o caso de uso é encerrado sem registro.

---

### E3. Falha na integração com o Sistema Acadêmico

#### E3.1. No passo P4, o sistema não consegue se comunicar com o Sistema Acadêmico da Unifor.

#### E3.2. O sistema exibe a mensagem: *"Não foi possível consultar a base acadêmica. Tente novamente em instantes."*

#### E3.3. O funcionário aguarda e tenta novamente. Se a falha persistir, o atendimento é suspenso até a normalização da integração.

---

### E4. Falha ao salvar o registro de empréstimo

#### E4.1. No passo P7, ocorre erro no banco de dados durante a tentativa de salvar o empréstimo.

#### E4.2. O sistema exibe a mensagem: *"Erro ao registrar o empréstimo. O projetor não foi marcado como emprestado. Tente novamente."*

#### E4.3. O sistema garante que o estado do projetor no banco de dados não é alterado (operação atômica).

#### E4.4. O funcionário tenta registrar o empréstimo novamente. Se o erro persistir, o caso de uso é encerrado sem registro.

---

## 9. Pós-condições

- O empréstimo do projetor está registrado no banco de dados com todos os dados associados (professor, matrícula, disciplina, projetor, data e hora).
- O projetor emprestado está marcado como "indisponível" no sistema.
- Os dados do empréstimo estão disponíveis para consulta nos relatórios gerenciais.

---

## 10. Requisitos Não Funcionais

- O sistema deve localizar e exibir os dados do professor em no máximo **2 segundos** após a busca (RNF 1.1.1).
- O formulário deve ser preenchido automaticamente em menos de **1 segundo** após a identificação do professor (RNF 1.1.1).
- O fluxo completo de registro de empréstimo deve ser concluído em no máximo **4 interações** na interface, após a digitação do nome (RNF 1.6.3).
- A operação de salvamento do empréstimo deve ser atômica — ou é salva integralmente ou não altera o banco de dados (RNF 1.9.3).

---

## 11. Ponto de Extensão

### PE1. Registrar Devolução de Projetor

Caso o funcionário identifique, durante o atendimento (passo P6), que o professor ainda possui um projetor em aberto, o sistema pode direcionar para o caso de uso **CDU-02 — Registrar Devolução de Projetor**, permitindo encerrar o empréstimo anterior antes de registrar o novo.

---

## 12. Frequência de Utilização

**Alta.** Este é o caso de uso central do sistema, acionado sempre que um professor solicitar um projetor. A frequência esperada é de múltiplos registros diários, com pico nos horários de início de aula (manhã: 07h–08h, tarde: 13h–14h, noite: 19h–20h).

---

## 13. Interface Visual

### IV1. Tela de Novo Empréstimo

A tela deve apresentar:
- Campo de busca por nome do professor (com botão "Buscar" e suporte à tecla Enter).
- Área de exibição dos dados encontrados: nome completo, matrícula e disciplina(s).
- Botão de confirmação de dados ("Confirmar Professor").
- Lista de projetores disponíveis para seleção.
- Botão "Registrar Empréstimo".
- Área de confirmação final com resumo do empréstimo registrado.

Navegabilidade: tela acessível a partir do menu principal ("Novo Empréstimo"). Após a confirmação, o sistema retorna automaticamente à tela inicial.

---

## 14. Observações

- O professor **não interage diretamente** com o sistema; toda operação é mediada pelo funcionário.
- Futuramente, pode ser avaliada a possibilidade de busca por matrícula do professor como alternativa ao nome, para casos de homônimos frequentes.
- O campo "Disciplina" deve refletir apenas as disciplinas do semestre vigente, conforme retornado pelo Sistema Acadêmico.

---

## 15. Referências

- Especificação de Requisitos Não Funcionais — Sistema de Empréstimo de Projetores CCT/Unifor (v1.0).
- CDU-02 — Registrar Devolução de Projetor.
- CDU-03 — Consultar Relatórios Gerenciais.

---

## 16. Checklist de Validação do Artefato (CDU)

### 16.1 Estrutura mínima

* [ ] Nome do caso de uso iniciado com verbo no infinitivo.
* [ ] Objetivo claro, direto e com foco em um objetivo principal.
* [ ] Tipo do caso de uso informado (concreto/abstrato), quando aplicável.
* [ ] Atores primário e secundários identificados corretamente.
* [ ] Precondições registradas (ou seção marcada como "Não se aplica").
* [ ] Fluxo principal completo e coerente com o objetivo.
* [ ] Fluxos alternativos e de exceção definidos quando necessários.
* [ ] Pós-condições registradas (ou seção marcada como "Não se aplica").
* [ ] Requisitos não funcionais específicos do CDU registrados, quando existirem.
* [ ] Pontos de extensão identificados corretamente, quando existirem.
* [ ] Frequência de utilização estimada.

### 16.2 Qualidade da especificação

* [ ] Passos escritos com linguagem simples e objetiva.
* [ ] Ações descritas com verbos no presente do indicativo (3ª pessoa).
* [ ] Alternância entre ação do ator e ação da solução está clara.
* [ ] Não há ambiguidade (termos vagos sem detalhe técnico).
* [ ] Regras de negócio e mensagens estão referenciadas quando necessário.

### 16.3 Consistência e rastreabilidade

* [ ] Pontos de entrada e saída dos fluxos alternativos estão explícitos.
* [ ] Fluxos de exceção estão vinculados aos passos corretos da solução.
* [ ] Referências internas entre passos (retorna/segue para) estão corretas.
* [ ] Interface visual (IV1 etc.) está coerente com o fluxo descrito.
* [ ] Referências para visão da demanda, glossário e RNF estão atualizadas.

### 16.4 Revisão final

* [ ] Não há contradições entre seções do artefato.
* [ ] Links internos e externos foram validados.
* [ ] Documento revisado por pares.
* [ ] Artefato pronto para uso em desenvolvimento e testes.
