# CASOS DE USO --- SISTEMA DE RESERVA DE INGRESSOS

## Diagrama

![Diagrama caso de uso](images/diagramaCasoUso.drawio.png "Diagrama de Casos de Uso")

## 👤 Atores

- **A1 --- Cliente:** Usuário que consulta filmes, seleciona sessões e assentos, realiza reservas, cancela reservas e obtém ingressos.
- **A2 --- Administrador:** Responsável por gerenciar filmes, sessões, salas e relatórios no sistema.
- **A3 --- Sistema ANCINE** (ator externo): Serviço externo da Agência Nacional do Cinema que fornece a lista oficial de filmes e recebe relatórios obrigatórios.

---

# UC00 --- Realizar Login

**ID:** UC00\
**Ator(es):** A1 --- Cliente, A2 --- Administrador\
**Pré-condição:** O usuário possui cadastro ativo no sistema.\
**Pós-condição:** O usuário é autenticado e direcionado à área correspondente ao seu perfil (cliente ou administrador).

## Fluxo Principal

1. O usuário acessa a tela de login do sistema.
2. O sistema solicita as credenciais de acesso (e-mail e senha).
3. O usuário informa o e-mail e a senha.
4. O sistema valida as credenciais informadas.
5. O sistema identifica o perfil do usuário (cliente ou administrador).
6. O sistema autentica o usuário e redireciona para a tela inicial correspondente ao seu perfil.

## Fluxos Alternativos

**FA1 --- Credenciais inválidas:**
1. No passo 4, o sistema identifica que o e-mail ou a senha estão incorretos.
2. O sistema exibe a mensagem "E-mail ou senha inválidos".
3. O sistema incrementa o contador de tentativas falhas.
4. O fluxo retorna ao passo 2 do fluxo principal.

**FA2 --- Conta bloqueada por excesso de tentativas:**
1. No passo 4, o sistema identifica que o número máximo de tentativas falhas foi atingido (ex.: 5 tentativas).
2. O sistema bloqueia temporariamente a conta do usuário.
3. O sistema exibe a mensagem "Conta bloqueada temporariamente — tente novamente em 30 minutos ou entre em contato com o suporte".
4. O caso de uso é encerrado.

**FA3 --- Usuário não cadastrado:**
1. No passo 4, o sistema não encontra o e-mail informado na base de dados.
2. O sistema exibe a mensagem "Usuário não encontrado — verifique o e-mail ou cadastre-se".
3. O sistema oferece a opção de realizar cadastro.
4. O caso de uso é encerrado.

**FA4 --- Recuperação de senha:**
1. Em qualquer passo, o usuário seleciona a opção "Esqueci minha senha".
2. O sistema solicita o e-mail cadastrado.
3. O usuário informa o e-mail.
4. O sistema envia um link de redefinição de senha para o e-mail informado.
5. O sistema exibe a mensagem "Link de recuperação enviado para o e-mail informado".
6. O caso de uso é encerrado.

---

# UC01 --- Consultar Filmes em Cartaz

**ID:** UC01\
**Ator(es):** A1 --- Cliente\
**Pré-condição:** O cliente está autenticado no sistema (UC00 realizado) e o sistema está conectado à base de dados.\
**Pós-condição:** A lista de filmes em cartaz é exibida ao cliente.

## Fluxo Principal

1. O cliente acessa a opção "Filmes em cartaz".
2. O sistema recupera a lista de filmes disponíveis nas diferentes salas e locais da rede.
3. O sistema exibe os filmes com título, sinopse, classificação indicativa e sessões disponíveis.
4. O cliente visualiza as informações dos filmes.
5. O cliente seleciona um filme para ver detalhes e sessões.

## Fluxos Alternativos

**FA1 --- Nenhum filme disponível:**
1. No passo 2, o sistema não encontra filmes cadastrados em cartaz.
2. O sistema exibe a mensagem "Nenhum filme em cartaz no momento".
3. O caso de uso é encerrado.

---

# UC02 --- Selecionar Sessão

**ID:** UC02\
**Ator(es):** A1 --- Cliente\
**Pré-condição:** O cliente está autenticado no sistema (UC00 realizado) e selecionou um filme em cartaz (UC01 realizado).\
**Pós-condição:** A sessão é escolhida e o mapa de assentos é apresentado.

## Fluxo Principal

1. O sistema apresenta as sessões disponíveis para o filme selecionado, incluindo horários, salas e locais (unidades da rede).
2. O cliente seleciona uma sessão.
3. O sistema verifica se há assentos disponíveis na sessão escolhida.
4. O sistema apresenta o mapa de assentos da sala correspondente.

## Fluxos Alternativos

**FA1 --- Sessão esgotada:**
1. No passo 3, o sistema verifica que todos os assentos da sessão estão reservados.
2. O sistema exibe a mensagem "Sessão esgotada".
3. O sistema sugere outras sessões disponíveis para o mesmo filme.
4. O cliente seleciona outra sessão e o fluxo retorna ao passo 3 do fluxo principal.

**FA2 --- Cliente retorna à lista de filmes:**
1. Em qualquer passo, o cliente opta por voltar à lista de filmes.
2. O sistema retorna à tela de filmes em cartaz.
3. O caso de uso UC01 é reiniciado.

---

# UC03 --- Escolher Assento

**ID:** UC03\
**Ator(es):** A1 --- Cliente\
**Pré-condição:** O cliente está autenticado no sistema (UC00 realizado) e selecionou uma sessão (UC02 realizado).\
**Pós-condição:** O(s) assento(s) é(são) selecionado(s) e reservado(s) temporariamente.

## Fluxo Principal

1. O sistema exibe o mapa de assentos da sala, indicando assentos disponíveis, ocupados e reservados temporariamente.
2. O cliente seleciona um ou mais assentos desejados.
3. O sistema verifica a disponibilidade dos assentos em tempo real (<<include>> UC08).
4. O sistema confirma a seleção e reserva os assentos temporariamente.
5. O sistema exibe o resumo da seleção ao cliente.

## Fluxos Alternativos

**FA1 --- Assento indisponível:**
1. No passo 3, o sistema identifica que o assento selecionado já foi reservado por outro cliente.
2. O sistema exibe a mensagem "Assento indisponível, por favor selecione outro".
3. O sistema atualiza o mapa de assentos.
4. O fluxo retorna ao passo 2 do fluxo principal.

**FA2 --- Cliente cancela seleção:**
1. Em qualquer passo, o cliente opta por cancelar a seleção de assentos.
2. O sistema libera os assentos reservados temporariamente.
3. O sistema retorna à tela de seleção de sessão (UC02).

---

# UC04 --- Reservar Ingresso

**ID:** UC04\
**Ator(es):** A1 --- Cliente\
**Pré-condição:** O cliente está autenticado no sistema (UC00 realizado) e selecionou assento(s) (UC03 realizado).\
**Pós-condição:** A reserva é confirmada, o ingresso é emitido e a disponibilidade é atualizada.

## Fluxo Principal

1. O sistema exibe o resumo da reserva (filme, sessão, assento(s), valor total).
2. O cliente confirma a reserva.
3. O sistema verifica a disponibilidade dos assentos (<<include>> UC08).
4. O sistema processa o pagamento (<<include>> UC07).
5. O sistema emite o ingresso digital.
6. O sistema registra a venda (quantidade de ingressos e renda obtida).
7. O sistema atualiza a disponibilidade dos assentos.
8. O sistema confirma a reserva ao cliente, exibindo o ingresso digital.

## Fluxos Alternativos

**FA1 --- Cliente solicita meia-entrada (<<extend>> UC09):**
1. Antes do passo 4, o cliente informa que deseja meia-entrada.
2. O caso de uso UC09 (Aplicar Meia-entrada) é executado.
3. O sistema atualiza o valor total com o desconto aplicado.
4. O fluxo retorna ao passo 4 do fluxo principal.

**FA2 --- Cliente utiliza ingresso gratuito de aniversário (<<extend>> UC10):**
1. Antes do passo 4, o cliente solicita o benefício de ingresso gratuito de aniversário.
2. O caso de uso UC10 (Conceder Ingresso Gratuito de Aniversário) é executado.
3. Se elegível, o valor total é zerado e o passo 4 (pagamento) é dispensado.
4. O fluxo retorna ao passo 5 do fluxo principal.

**FA3 --- Cliente possui benefício de melhores assentos por antecedência (<<extend>> UC11):**
1. Antes do passo 1, o sistema verifica se o cliente é elegível para melhores assentos.
2. O caso de uso UC11 (Reservar Melhores Assentos por Antecedência) é executado.
3. Se elegível, o mapa de assentos é reapresentado com os melhores assentos liberados.
4. O fluxo retorna ao passo 1 do fluxo principal com assentos atualizados.

**FA4 --- Pagamento recusado:**
1. No passo 4, o sistema de pagamento recusa a transação.
2. O sistema exibe a mensagem "Pagamento recusado".
3. O sistema libera os assentos reservados temporariamente.
4. O sistema oferece ao cliente a opção de tentar novamente ou cancelar.
5. Se o cliente cancela, o caso de uso é encerrado.
6. Se o cliente tenta novamente, o fluxo retorna ao passo 4.

**FA5 --- Assento não mais disponível:**
1. No passo 3, o sistema identifica que o assento foi reservado por outro cliente durante o processo.
2. O sistema exibe a mensagem "Assento não mais disponível".
3. O sistema retorna ao UC03 para nova seleção.

---

# UC05 --- Cancelar Reserva

**ID:** UC05\
**Ator(es):** A1 --- Cliente\
**Pré-condição:** O cliente está autenticado no sistema (UC00 realizado) e possui uma reserva existente no sistema.\
**Pós-condição:** A reserva é cancelada, o reembolso é processado e a disponibilidade dos assentos é restaurada.

## Fluxo Principal

1. O cliente acessa a área "Minhas Reservas".
2. O sistema exibe a lista de reservas ativas do cliente.
3. O cliente seleciona a reserva que deseja cancelar.
4. O sistema verifica a política de cancelamento (prazo, regras).
5. O sistema solicita confirmação do cancelamento.
6. O cliente confirma o cancelamento.
7. O sistema cancela a reserva.
8. O sistema processa o reembolso do valor pago.
9. O sistema atualiza a disponibilidade dos assentos, liberando-os.
10. O sistema confirma o cancelamento ao cliente.

## Fluxos Alternativos

**FA1 --- Cancelamento fora do prazo permitido:**
1. No passo 4, o sistema identifica que o prazo para cancelamento já expirou.
2. O sistema exibe a mensagem "Cancelamento não permitido — prazo expirado".
3. O sistema informa a política de cancelamento vigente.
4. O caso de uso é encerrado sem cancelamento.

**FA2 --- Falha no processamento de reembolso:**
1. No passo 8, o sistema não consegue processar o reembolso.
2. O sistema exibe a mensagem "Falha no reembolso — tente novamente mais tarde".
3. O sistema mantém a reserva como "cancelamento pendente".
4. O sistema registra a falha para acompanhamento pelo administrador.
5. O caso de uso é encerrado.

**FA3 --- Cliente desiste do cancelamento:**
1. No passo 6, o cliente opta por não confirmar o cancelamento.
2. O sistema mantém a reserva ativa.
3. O sistema retorna à lista de reservas.

---

# UC06 --- Imprimir Ingresso

**ID:** UC06\
**Ator(es):** A1 --- Cliente\
**Pré-condição:** O cliente está autenticado no sistema (UC00 realizado) e possui um ingresso digital emitido (UC04 realizado).\
**Pós-condição:** O ingresso é impresso em formato físico.

## Fluxo Principal

1. O cliente acessa a área "Meus Ingressos".
2. O sistema exibe os ingressos digitais do cliente.
3. O cliente seleciona o ingresso que deseja imprimir.
4. O cliente solicita a impressão.
5. O sistema envia o ingresso para a impressora.
6. A impressora imprime o ingresso.
7. O cliente retira o ingresso impresso.

## Fluxos Alternativos

**FA1 --- Impressora indisponível:**
1. No passo 5, o sistema detecta que a impressora está indisponível ou com erro.
2. O sistema exibe a mensagem "Impressora indisponível no momento".
3. O sistema sugere ao cliente utilizar o ingresso digital no celular.
4. O caso de uso é encerrado.

---

# UC07 --- Processar Pagamento

**ID:** UC07\
**Ator(es):** A1 --- Cliente\
**Tipo:** <<include>> de UC04\
**Pré-condição:** A reserva está em andamento e o valor total foi calculado.\
**Pós-condição:** O pagamento é aprovado e registrado, ou recusado.

## Fluxo Principal

1. O sistema exibe o valor total da reserva.
2. O sistema solicita a forma de pagamento (cartão de crédito, débito, PIX, etc.).
3. O cliente seleciona a forma de pagamento.
4. O sistema solicita os dados de pagamento correspondentes.
5. O cliente informa os dados de pagamento.
6. O sistema valida os dados informados.
7. O sistema processa a transação junto à operadora/banco.
8. O sistema recebe a confirmação de aprovação.
9. O sistema registra o pagamento e emite comprovante.

## Fluxos Alternativos

**FA1 --- Pagamento recusado:**
1. No passo 8, a operadora/banco recusa a transação.
2. O sistema exibe a mensagem "Pagamento recusado — verifique os dados ou tente outra forma de pagamento".
3. O sistema oferece ao cliente a opção de informar novos dados.
4. Se o cliente informa novos dados, o fluxo retorna ao passo 4.
5. Se o cliente cancela, o caso de uso é encerrado com status de pagamento recusado.

**FA2 --- Dados de pagamento inválidos:**
1. No passo 6, o sistema identifica que os dados informados são inválidos (número incorreto, data de validade expirada, etc.).
2. O sistema exibe a mensagem "Dados inválidos — por favor corrija as informações".
3. O fluxo retorna ao passo 4.

---

# UC08 --- Verificar Disponibilidade

**ID:** UC08\
**Ator(es):** (interno ao sistema)\
**Tipo:** <<include>> de UC04\
**Pré-condição:** Assento(s) selecionado(s) pelo cliente.\
**Pós-condição:** Assento confirmado como disponível e reservado temporariamente, ou rejeitado.

## Fluxo Principal

1. O sistema consulta a base de dados de assentos em tempo real.
2. O sistema verifica se o(s) assento(s) solicitado(s) está(ão) livre(s).
3. O sistema confirma a disponibilidade.
4. O sistema reserva o(s) assento(s) temporariamente para o cliente.

## Fluxos Alternativos

**FA1 --- Assento já reservado:**
1. No passo 2, o sistema identifica que o assento já foi reservado por outro cliente.
2. O sistema rejeita a solicitação.
3. O sistema retorna o status "indisponível" para o assento.
4. O caso de uso chamador trata a indisponibilidade.

---

# UC09 --- Aplicar Meia-entrada

**ID:** UC09\
**Ator(es):** A1 --- Cliente\
**Tipo:** <<extend>> de UC04\
**Pré-condição:** O cliente solicita meia-entrada durante o processo de reserva.\
**Pós-condição:** O desconto de meia-entrada é aplicado e a categoria é registrada, ou o desconto é negado.

## Fluxo Principal

1. O cliente informa que deseja meia-entrada.
2. O sistema solicita a categoria do benefício (estudante, maior de 65 anos ou professor da rede pública/privada).
3. O cliente seleciona a categoria.
4. O sistema solicita o documento comprobatório (carteirinha de estudante, documento de identidade com data de nascimento, ou comprovante de vínculo como professor).
5. O cliente apresenta/informa o documento.
6. O sistema valida a comprovação.
7. O sistema aplica o desconto de 50% no valor do ingresso.
8. O sistema registra a categoria do ingresso de meia-entrada vendido (para fins de relatório).

## Fluxos Alternativos

**FA1 --- Comprovação inválida:**
1. No passo 6, o sistema não consegue validar o documento comprobatório (documento vencido, dados inconsistentes, etc.).
2. O sistema exibe a mensagem "Comprovação inválida — desconto de meia-entrada não pode ser aplicado".
3. O sistema solicita ao cliente se deseja apresentar outro documento ou prosseguir com ingresso integral.
4. Se o cliente apresenta outro documento, o fluxo retorna ao passo 4.
5. Se o cliente opta por ingresso integral, o fluxo segue para FA2.

**FA2 --- Cliente opta por ingresso integral:**
1. O cliente informa que deseja prosseguir com o ingresso integral (preço cheio).
2. O sistema mantém o valor original do ingresso.
3. O caso de uso é encerrado, retornando ao fluxo do UC04.

---

# UC10 --- Conceder Ingresso Gratuito de Aniversário

**ID:** UC10\
**Ator(es):** A1 --- Cliente\
**Tipo:** <<extend>> de UC04\
**Pré-condição:** O cliente é cadastrado no programa de fidelidade e está no mês de seu aniversário.\
**Pós-condição:** O ingresso é emitido sem custo para o cliente, ou o benefício é negado.

## Fluxo Principal

1. O cliente solicita o benefício de ingresso gratuito de aniversário.
2. O sistema verifica se o cliente está cadastrado no programa de fidelidade.
3. O sistema verifica se o mês atual corresponde ao mês de aniversário do cliente.
4. O sistema verifica se o cliente ainda não utilizou o benefício no mês corrente.
5. O sistema aplica o benefício, zerando o valor do ingresso.
6. O sistema registra a utilização do benefício para o cliente.

## Fluxos Alternativos

**FA1 --- Cliente não cadastrado no programa de fidelidade:**
1. No passo 2, o sistema identifica que o cliente não é membro do programa de fidelidade.
2. O sistema exibe a mensagem "Benefício disponível apenas para membros do programa de fidelidade".
3. O sistema oferece a opção de prosseguir com compra normal.
4. O caso de uso é encerrado, retornando ao fluxo do UC04.

**FA2 --- Não é mês de aniversário:**
1. No passo 3, o sistema identifica que o mês atual não corresponde ao mês de aniversário do cliente.
2. O sistema exibe a mensagem "Benefício disponível apenas no mês de seu aniversário".
3. O sistema oferece a opção de prosseguir com compra normal.
4. O caso de uso é encerrado, retornando ao fluxo do UC04.

**FA3 --- Benefício já utilizado no mês:**
1. No passo 4, o sistema identifica que o cliente já utilizou o ingresso gratuito neste mês.
2. O sistema exibe a mensagem "Benefício de aniversário já utilizado neste mês".
3. O sistema oferece a opção de prosseguir com compra normal.
4. O caso de uso é encerrado, retornando ao fluxo do UC04.

---

# UC11 --- Reservar Melhores Assentos por Antecedência

**ID:** UC11\
**Ator(es):** A1 --- Cliente\
**Tipo:** <<extend>> de UC04\
**Pré-condição:** O cliente é cadastrado no programa de fidelidade e está realizando a compra com pelo menos dois dias de antecedência em relação à sessão.\
**Pós-condição:** Os melhores assentos são disponibilizados para seleção do cliente, ou o acesso é negado.

## Fluxo Principal

1. O sistema verifica se o cliente está cadastrado no programa de fidelidade.
2. O sistema verifica se a data da compra é pelo menos dois dias antes da data da sessão.
3. O sistema libera os melhores assentos (categorias premium/preferenciais) para seleção.
4. O sistema destaca os melhores assentos no mapa de assentos.
5. O cliente seleciona entre os assentos disponíveis (incluindo os premium).

## Fluxos Alternativos

**FA1 --- Cliente não é membro do programa de fidelidade:**
1. No passo 1, o sistema identifica que o cliente não é membro do programa de fidelidade.
2. O sistema exibe apenas os assentos convencionais.
3. O caso de uso é encerrado, retornando ao fluxo normal do UC03/UC04.

**FA2 --- Compra sem antecedência mínima de dois dias:**
1. No passo 2, o sistema identifica que a compra está sendo feita com menos de dois dias de antecedência.
2. O sistema exibe apenas os assentos convencionais.
3. O caso de uso é encerrado, retornando ao fluxo normal do UC03/UC04.

---

# UC12 --- Gerenciar Filmes

**ID:** UC12\
**Ator(es):** A2 --- Administrador\
**Pré-condição:** O administrador está autenticado no sistema (UC00 realizado).\
**Pós-condição:** O filme é cadastrado, atualizado ou removido do sistema.

## Fluxo Principal

1. O administrador acessa a área de gerenciamento de filmes.
2. O sistema exibe a lista de filmes cadastrados.
3. O administrador seleciona a ação desejada (cadastrar, editar ou remover).
4. O administrador informa ou altera os dados do filme (título, sinopse, duração, classificação indicativa, gênero).
5. O sistema valida os dados informados.
6. O sistema salva as alterações na base de dados.
7. O sistema confirma a operação ao administrador.

## Fluxos Alternativos

**FA1 --- Dados inválidos:**
1. No passo 5, o sistema identifica dados obrigatórios não preenchidos ou formato inválido.
2. O sistema exibe as mensagens de erro correspondentes a cada campo inválido.
3. O sistema solicita a correção dos dados.
4. O fluxo retorna ao passo 4.

**FA2 --- Filme com sessões ativas:**
1. No passo 3, o administrador tenta remover um filme que possui sessões futuras ativas.
2. O sistema exibe a mensagem "Filme não pode ser removido — possui sessões ativas".
3. O sistema lista as sessões vinculadas ao filme.
4. O caso de uso é encerrado sem remoção.

**FA3 --- Filme já cadastrado:**
1. No passo 5, o sistema identifica que já existe um filme com o mesmo título cadastrado.
2. O sistema exibe a mensagem "Filme já cadastrado no sistema".
3. O sistema sugere editar o registro existente.
4. O fluxo retorna ao passo 3.

---

# UC13 --- Gerenciar Sessões

**ID:** UC13\
**Ator(es):** A2 --- Administrador\
**Pré-condição:** O administrador está autenticado no sistema (UC00 realizado) e há filmes e salas cadastrados no sistema.\
**Pós-condição:** A sessão é cadastrada, atualizada ou removida.

## Fluxo Principal

1. O administrador acessa a área de gerenciamento de sessões.
2. O sistema exibe a lista de sessões cadastradas.
3. O administrador seleciona a ação desejada (cadastrar, editar ou remover).
4. O administrador informa ou altera os dados da sessão (filme, sala, horário, local/unidade).
5. O sistema valida os dados e verifica conflitos de horário e sala.
6. O sistema salva a sessão na base de dados.
7. O sistema confirma a operação ao administrador.

## Fluxos Alternativos

**FA1 --- Conflito de horário/sala:**
1. No passo 5, o sistema identifica que já existe uma sessão agendada na mesma sala e horário.
2. O sistema exibe a mensagem "Conflito de horário — sala já ocupada neste horário".
3. O sistema exibe as sessões que geram conflito.
4. O sistema sugere horários ou salas alternativas disponíveis.
5. O fluxo retorna ao passo 4.

**FA2 --- Sessão com ingressos vendidos:**
1. No passo 3, o administrador tenta remover uma sessão que já possui ingressos vendidos.
2. O sistema exibe a mensagem "Sessão não pode ser removida — há ingressos vendidos".
3. O sistema exibe a quantidade de ingressos vendidos para a sessão.
4. O caso de uso é encerrado sem remoção.

---

# UC14 --- Gerenciar Salas

**ID:** UC14\
**Ator(es):** A2 --- Administrador\
**Pré-condição:** O administrador está autenticado no sistema (UC00 realizado).\
**Pós-condição:** A sala é cadastrada, atualizada ou removida.

## Fluxo Principal

1. O administrador acessa a área de gerenciamento de salas.
2. O sistema exibe a lista de salas cadastradas, organizadas por local/unidade.
3. O administrador seleciona a ação desejada (cadastrar, editar ou remover).
4. O administrador informa ou altera os dados da sala (nome/número, capacidade, mapa de assentos, local/unidade da rede).
5. O sistema valida os dados informados.
6. O sistema salva as alterações na base de dados.
7. O sistema confirma a operação ao administrador.

## Fluxos Alternativos

**FA1 --- Sala com sessões ativas:**
1. No passo 3, o administrador tenta remover uma sala que possui sessões futuras agendadas.
2. O sistema exibe a mensagem "Sala não pode ser removida — possui sessões agendadas".
3. O sistema lista as sessões vinculadas à sala.
4. O caso de uso é encerrado sem remoção.

**FA2 --- Dados inválidos:**
1. No passo 5, o sistema identifica dados obrigatórios não preenchidos (ex.: capacidade igual a zero).
2. O sistema exibe as mensagens de erro.
3. O sistema solicita a correção dos dados.
4. O fluxo retorna ao passo 4.

---

# UC15 --- Gerar Relatórios de Vendas

**ID:** UC15\
**Ator(es):** A2 --- Administrador\
**Pré-condição:** O administrador está autenticado no sistema (UC00 realizado) e existem dados de vendas registrados no sistema.\
**Pós-condição:** O relatório é gerado e exibido ao administrador.

## Fluxo Principal

1. O administrador acessa a área de relatórios.
2. O sistema exibe as opções de tipo de relatório (por sessão, por filme, por sala, por cinema/unidade ou por período).
3. O administrador seleciona o tipo de relatório desejado.
4. O administrador informa os filtros aplicáveis (datas, filme, sala, unidade).
5. O sistema consulta o histórico de vendas e reservas.
6. O sistema consolida os dados: quantidade de ingressos vendidos, renda obtida, quantidade de ingressos de meia-entrada por categoria (estudantes, maiores de 65 anos, professores).
7. O sistema exibe o relatório consolidado ao administrador.
8. O sistema oferece opção de exportar o relatório.

## Fluxos Alternativos

**FA1 --- Nenhum dado disponível para o filtro selecionado:**
1. No passo 5, o sistema não encontra registros de vendas para os filtros informados.
2. O sistema exibe a mensagem "Nenhum dado encontrado para os filtros selecionados".
3. O sistema sugere ao administrador alterar os filtros.
4. O fluxo retorna ao passo 3.

**FA2 --- Relatório automático após término de sessão:**
1. O sistema detecta que uma sessão foi encerrada.
2. O sistema gera automaticamente o relatório da sessão encerrada.
3. O sistema armazena o relatório no histórico.
4. O relatório fica disponível para consulta e envio à ANCINE (UC16).

---

# UC16 --- Enviar Relatórios para ANCINE

**ID:** UC16\
**Ator(es):** A2 --- Administrador, A3 --- Sistema ANCINE\
**Pré-condição:** O administrador está autenticado no sistema (UC00 realizado), relatórios obrigatórios estão gerados e as sessões correspondentes estão encerradas.\
**Pós-condição:** Os relatórios são enviados e confirmados pelo Sistema ANCINE.

## Fluxo Principal

1. O sistema gera o relatório obrigatório automaticamente após o término de cada sessão (ingressos vendidos, renda, categorias de meia-entrada, títulos exibidos).
2. O administrador acessa a área de relatórios para a ANCINE.
3. O sistema exibe os relatórios pendentes de envio.
4. O administrador revisa o relatório.
5. O administrador confirma o envio.
6. O sistema transmite o relatório ao Sistema ANCINE.
7. O Sistema ANCINE recebe e processa o relatório.
8. O Sistema ANCINE retorna a confirmação de recebimento.
9. O sistema registra a confirmação e marca o relatório como "enviado".

## Fluxos Alternativos

**FA1 --- Falha na comunicação com ANCINE:**
1. No passo 6, o sistema não consegue estabelecer conexão com o Sistema ANCINE.
2. O sistema exibe a mensagem "Falha na comunicação com ANCINE".
3. O sistema agenda o reenvio automático do relatório.
4. O sistema registra a tentativa com falha no log.
5. O administrador é notificado sobre a falha e o agendamento de reenvio.

**FA2 --- Relatório rejeitado pela ANCINE:**
1. No passo 7, o Sistema ANCINE identifica inconsistências no relatório.
2. O Sistema ANCINE retorna mensagem de rejeição com os erros encontrados.
3. O sistema exibe os erros ao administrador.
4. O administrador corrige os dados necessários.
5. O fluxo retorna ao passo 5 para reenvio.

---

# UC17 --- Consultar Lista Oficial ANCINE

**ID:** UC17\
**Ator(es):** A2 --- Administrador, A3 --- Sistema ANCINE\
**Pré-condição:** O administrador está autenticado no sistema (UC00 realizado) e a integração com o serviço da ANCINE está ativa e configurada.\
**Pós-condição:** A lista de filmes oficiais é obtida e os títulos selecionados são disponibilizados nas salas.

## Fluxo Principal

1. O administrador acessa a opção "Consultar Lista Oficial ANCINE".
2. O sistema envia requisição de consulta ao Sistema ANCINE.
3. O Sistema ANCINE retorna a lista oficial de filmes disponíveis para exibição.
4. O sistema exibe a lista de filmes ao administrador.
5. O administrador seleciona os títulos que deseja disponibilizar na rede.
6. O administrador associa os títulos selecionados às salas e unidades desejadas.
7. O sistema registra quais títulos foram disponibilizados em cada sala, com base na lista oficial da ANCINE.
8. O sistema confirma a operação ao administrador.

## Fluxos Alternativos

**FA1 --- Falha na comunicação com ANCINE:**
1. No passo 2, o sistema não consegue estabelecer conexão com o serviço da ANCINE.
2. O sistema exibe a mensagem "Falha na comunicação com o serviço da ANCINE — tente novamente mais tarde".
3. O caso de uso é encerrado.

**FA2 --- Filme já cadastrado no sistema:**
1. No passo 5, o administrador seleciona um título que já está cadastrado no sistema.
2. O sistema exibe a mensagem "Filme já cadastrado no sistema".
3. O sistema destaca os filmes já existentes na lista.
4. O administrador pode optar por atualizar os dados do filme existente ou ignorar.
5. O fluxo retorna ao passo 5 para continuar a seleção.

**FA3 --- Lista vazia retornada pela ANCINE:**
1. No passo 3, o Sistema ANCINE retorna uma lista vazia.
2. O sistema exibe a mensagem "Nenhum filme disponível na lista oficial da ANCINE no momento".
3. O caso de uso é encerrado.

---

## Diagrama de Classes
![Diagrama de classes](images/diagramaClasse.drawio.png "Title")


--- 

## Diagramas de Sequências

### UC00 - Realizar Login
![Diagrama de sequência UC00 - Realizar Login](images/sequenciaUC00.png "Diagrama de Sequência - UC00 Realizar Login")

### UC01 - Consultar Filmes em Cartaz
![Diagrama de sequência UC01 - Consultar Filmes em Cartaz](images/sequenciaUC01.png "Diagrama de Sequência - UC01 Consultar Filmes em Cartaz")

### UC02 - Selecionar Sessão
![Diagrama de sequência UC02 - Selecionar Sessão](images/sequenciaUC02.png "Diagrama de Sequência - UC02 Selecionar Sessão")

### UC03 - Escolher Assento
![Diagrama de sequência UC03 - Escolher Assento](images/sequenciaUC03.png "Diagrama de Sequência - UC03 Escolher Assento")

### UC04 - Reservar Ingresso
![Diagrama de sequência UC04 - Reservar Ingresso](images/sequenciaUC04.png "Diagrama de Sequência - UC04 Reservar Ingresso")

### UC05 - Cancelar Reserva
![Diagrama de sequência UC05 - Cancelar Reserva](images/sequenciaUC05.png "Diagrama de Sequência - UC05 Cancelar Reserva")

### UC06 - Imprimir Ingresso
![Diagrama de sequência UC06 - Imprimir Ingresso](images/sequenciaUC06.png "Diagrama de Sequência - UC06 Imprimir Ingresso")

### UC07 - Processar Pagamento
![Diagrama de sequência UC07 - Processar Pagamento](images/sequenciaUC07.png "Diagrama de Sequência - UC07 Processar Pagamento")

### UC08 - Verificar Disponibilidade
![Diagrama de sequência UC08 - Verificar Disponibilidade](images/sequenciaUC08.png "Diagrama de Sequência - UC08 Verificar Disponibilidade")

### UC09 - Aplicar Meia-entrada
![Diagrama de sequência UC09 - Aplicar Meia-entrada](images/sequenciaUC09.png "Diagrama de Sequência - UC09 Aplicar Meia-entrada")

### UC10 - Conceder Ingresso Gratuito de Aniversário
![Diagrama de sequência UC10 - Conceder Ingresso Gratuito de Aniversário](images/sequenciaUC10.png "Diagrama de Sequência - UC10 Conceder Ingresso Gratuito de Aniversário")

### UC11 - Reservar Melhores Assentos por Antecedência
![Diagrama de sequência UC11 - Reservar Melhores Assentos por Antecedência](images/sequenciaUC11.png "Diagrama de Sequência - UC11 Reservar Melhores Assentos por Antecedência")

### UC12 - Gerenciar Filmes
![Diagrama de sequência UC12 - Gerenciar Filmes](images/sequenciaUC12.png "Diagrama de Sequência - UC12 Gerenciar Filmes")

### UC13 - Gerenciar Sessões
![Diagrama de sequência UC13 - Gerenciar Sessões](images/sequenciaUC13.png "Diagrama de Sequência - UC13 Gerenciar Sessões")

### UC14 - Gerenciar Salas
![Diagrama de sequência UC14 - Gerenciar Salas](images/sequenciaUC14.png "Diagrama de Sequência - UC14 Gerenciar Salas")

### UC15 - Gerar Relatórios de Vendas
![Diagrama de sequência UC15 - Gerar Relatórios de Vendas](images/sequenciaUC15.png "Diagrama de Sequência - UC15 Gerar Relatórios de Vendas")

### UC16 - Enviar Relatórios para ANCINE
![Diagrama de sequência UC16 - Enviar Relatórios para ANCINE](images/sequenciaUC16.png "Diagrama de Sequência - UC16 Enviar Relatórios para ANCINE")

### UC17 - Consultar Lista Oficial ANCINE
![Diagrama de sequência UC17 - Consultar Lista Oficial ANCINE](images/sequenciaUC17.png "Diagrama de Sequência - UC17 Consultar Lista Oficial ANCINE")


