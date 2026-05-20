# Regras de Negócio da Solução (RN) — Sistema de Empréstimo de Projetores CCT/Unifor

---

## RN1. Professor deve estar cadastrado na base acadêmica para realizar empréstimo

* **Identificador:** RN1 — Professor deve estar cadastrado na base acadêmica para realizar empréstimo

* **Descrição:** O sistema somente permite o registro de um empréstimo quando o professor solicitante é localizado na base acadêmica da Unifor. Caso o nome informado não retorne nenhum resultado na consulta ao Sistema Acadêmico, o empréstimo não pode ser registrado e o atendimento é encerrado sem movimentação no banco de dados.

---

## RN2. Projetor com status "emprestado" não pode ser emprestado novamente

* **Identificador:** RN2 — Projetor com status "emprestado" não pode ser emprestado novamente

* **Descrição:** Um projetor só pode ser selecionado para empréstimo quando seu status no sistema for "disponível". O sistema deve exibir na tela de novo empréstimo apenas os projetores com status "disponível". Projetores com status "emprestado" não devem aparecer como opção de seleção, impedindo duplicidade de registros para o mesmo equipamento.

---

## RN3. Empréstimos em aberto do professor devem ser alertados antes de novo empréstimo

* **Identificador:** RN3 — Empréstimos em aberto do professor devem ser alertados antes de novo empréstimo

* **Descrição:** Ao localizar o professor durante o fluxo de novo empréstimo, o sistema deve verificar se ele possui projetores emprestados e ainda não devolvidos. Em caso positivo, o sistema deve exibir um alerta com os detalhes dos empréstimos em aberto (identificador do projetor e data do empréstimo) antes que o funcionário prossiga. A decisão de autorizar ou recusar o novo empréstimo cabe ao funcionário, conforme critério institucional do CCT.

---

## RN4. Formulário de empréstimo deve ser preenchido automaticamente pelo sistema

* **Identificador:** RN4 — Formulário de empréstimo deve ser preenchido automaticamente pelo sistema

* **Descrição:** Após a identificação do professor na base acadêmica, o sistema deve preencher automaticamente os campos do formulário virtual de empréstimo: nome completo, matrícula, disciplina(s) lecionadas no semestre vigente e data/hora da solicitação. O funcionário não deve preencher manualmente esses campos. O preenchimento automático deve ocorrer em menos de 1 segundo após a localização do professor.

---

## RN5. Somente funcionário autenticado pode registrar empréstimos e devoluções

* **Identificador:** RN5 — Somente funcionário autenticado pode registrar empréstimos e devoluções

* **Descrição:** O acesso às funcionalidades de registro de empréstimo, registro de devolução e consulta de relatórios é restrito a funcionários autenticados com credenciais institucionais da Unifor. Sessões encerradas ou credenciais inválidas devem impedir qualquer operação no sistema. A autenticação deve ser integrada ao sistema de identidade institucional (ex.: LDAP ou SSO da Unifor).

---

## RN6. Devolução do projetor encerra o empréstimo e libera o equipamento

* **Identificador:** RN6 — Devolução do projetor encerra o empréstimo e libera o equipamento

* **Descrição:** Ao confirmar a devolução de um projetor, o sistema deve registrar a data e hora da devolução, encerrar o registro de empréstimo correspondente e atualizar o status do projetor para "disponível". Essas três operações devem ocorrer de forma atômica: ou todas são concluídas com sucesso, ou nenhuma alteração é persistida no banco de dados, preservando a consistência entre o estado físico e o registrado no sistema.

---

## RN7. Todo empréstimo deve registrar dados para geração de relatórios gerenciais

* **Identificador:** RN7 — Todo empréstimo deve registrar dados para geração de relatórios gerenciais

* **Descrição:** Cada registro de empréstimo deve armazenar obrigatoriamente: nome completo e matrícula do professor, disciplina lecionada no semestre vigente, identificador do projetor, data e hora do empréstimo e data e hora da devolução (quando ocorrer). Esses dados devem estar disponíveis para consulta nos relatórios gerenciais do CCT, permitindo análises como: professores que mais utilizam projetores, disciplinas com maior demanda, horários de pico de utilização e tempo médio de empréstimo por equipamento.

---

## RN8. Nome com múltiplos resultados exige seleção explícita pelo funcionário

* **Identificador:** RN8 — Nome com múltiplos resultados exige seleção explícita pelo funcionário

* **Descrição:** Quando a busca por nome do professor retornar dois ou mais registros na base acadêmica, o sistema deve exibir a lista completa de resultados com nome completo e matrícula de cada professor encontrado. O funcionário deve selecionar explicitamente o professor correto antes de prosseguir com o empréstimo. O sistema não deve assumir automaticamente qual professor foi identificado em caso de ambiguidade.

---

## RN9. Estoque abaixo de 20% deve gerar alerta ao administrador

* **Identificador:** RN9 — Estoque abaixo de 20% deve gerar alerta ao administrador

* **Descrição:** O sistema deve monitorar continuamente a proporção de projetores disponíveis em relação ao total cadastrado. Quando a quantidade de projetores com status "disponível" for igual ou inferior a 20% do total de projetores cadastrados no CCT, o sistema deve gerar um alerta automático ao administrador do sistema, possibilitando ação preventiva antes que o estoque se esgote completamente.

---

## 2. Checklist de Validação das Regras de Negócio

Use este checklist antes de finalizar cada regra.

### 2.1 Estrutura mínima

* [ ] Identificador único e padronizado (ex.: RN1, RN1.1, RN2).
* [ ] Nome da regra no formato sujeito + verbo + objeto.
* [ ] Descrição clara, direta e sem ambiguidades.

### 2.2 Qualidade da regra

* [ ] A regra descreve apenas uma decisão/comportamento principal.
* [ ] Condições de aplicação (gatilho/contexto) estão explícitas.
* [ ] Resultado esperado da regra está explícito.
* [ ] A regra é verificável e testável.

### 2.3 Consistência e rastreabilidade

* [ ] Não há conflito com outras regras já existentes.
* [ ] A regra referencia origem (negócio, norma, lei ou decisão do cliente), quando aplicável.
* [ ] A regra está alinhada com CDU, RNF e demais artefatos relacionados.

### 2.4 Prontidão

* [ ] Conteúdo revisado por pares.
* [ ] Regra pronta para uso em análise, desenvolvimento e testes.
