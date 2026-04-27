# CASOS DE USO --- SISTEMA DE RESERVA DE INGRESSOS

## Diagrama

![Diagrama caso de uso](images/diagramaCasoUso.drawio.png "Diagrama de Casos de Uso")

## 👤 Atores

- **A1 --- Cliente:** Usuário que consulta filmes, seleciona sessões e assentos, realiza reservas, cancela reservas e obtém ingressos.
- **A2 --- Administrador:** Responsável por gerenciar filmes, sessões, salas e relatórios no sistema.
- **A3 --- Sistema ANCINE** (ator externo): Serviço externo da Agência Nacional do Cinema que fornece a lista oficial de filmes e recebe relatórios obrigatórios.

---

# UC00 --- Realizar Login
![Sequencia_UC00]()

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
![Diagrama de classes](https://www.plantuml.com/plantuml/png/ZLPBSnit4xplhzZIYVu-MsLVLIwN6OfAM1KfZYJcFWI6wt7ms1hCyk0a_pt4ajZ2y57nROd6exkd1-3d9C0oUDS8YSFsXPAWXzPY-um9UJT-haB7c59CJPF-fD03Ws-DWvJc8aoRGN1bOdYXxzY-RveHQTTf0ARO_jMqxV_9thNhzUe-VuVeyQvz4QRGvT_eWLmza31yXNh0Wi0J94CUWyIxzXu4ytYFl3rPAY13gaS4Wu3G3j83kA-mnoX81c3841dWsSZWRJkU--QktSJe44BpJv6oS8H1y235H4jTV22BZYnO9SMZtt2DbRqokC60Mp8Kvu5so2cxiqvWuoCcDJ55HuO4-149ba4OBCmC27QHAHFSHRAZSmuxsg4hzQasdcNFvZA0gmHLI-OfPI-_C7eQ_3Xx4NtFyBRDto-O4iHBNiojfq0faufySCI3C9d1gPfro1WC0J-GyvXcXNODvLFYDGmSd2B0p-X08bDK48OTLTZyQR5wMiwu_2upznfAA3_-_UvTAQiULFgc05103_yTPHFwRzFloR9r6CFm0vqGB3AQgBOy2zcmhOyuKhlvSDF6FVsR3xUt_xifHnO6sSmVO0yzbEEEJIBKApPZ9wb0xI17dHVJsfbEJH6l2OwC2hjZ5iy9RabdAWmUEKxa-aDu5Ptp2u0FznY_ekyZ0nFuNFKwFz4CFN0zucY3N0tjXOmUQXM5UH4oHlrSXf0CJjbe1mO3yP729AR3WAmceLy02to_q8I7zjGdbOlobyYm2cJeU56-aiSQUo2lWJv7xYT8k1z6GnxS8Oi6cS7gQcCIMAOkoXwe2bqju0EtshbiInM0zzcweuI3gvUn8hOyH5fjkOcLPKNZWWaCQ0DoKYE_HW6dX9udniIm9VOw4GQ2GNSwvlArntzCE-_HidTMNXuEhQyeQevbl08N_Ij1Vzc3Khnr806cg7JvwV7G9PmUaaSaVMCpo41Ilo8gUeM3xe6QNbCwMGagKvIeBz21lzBraRqYDXRkQirqvNN9XI6ad8ILhIVt74ssdcSMR4nl2gPKLXBsdnFV8g2ykTTY-VjpTUBPFOgUX6nSUisNV9ythNmlS_cUEX9mTVmyi05U7TSKv7gf9ivD4N7oP6ejjT7AEuackhWCEpfhlf-HTUCL8CzMmuL-1i36sRJlcS1d_8OaO9N2gGg2NJvIAJ0nfC49yA-K17xRag63S_dtn_4w-dJXVyECovTAnDaPjxUVJfsxF6aygVcCmGxU_Gi0)


--- 

## Diagramas de Sequências

### UC00 - Realizar Login
![Diagrama de sequência UC00 - Realizar Login](https://www.plantuml.com/plantuml/png/bLHTQjj047xNAGOzjL2RnErJe2N6SS3Wfk0c1vYinitGrQuxkpAbjmbzw45yiSvAsM39TaXU3FBEz_Dz-tCP8afiQbj4ZfRWRdPs1YDO4Lh-Wm9B_uDTKQ29Ng2SdiFCChb89M20wVdpmWIcLSsEGnIil7JBasAZv6nuWox2NMXGs1U5ieo-vCyBG4CXe81jfPx-rWcblu3WhUBL8r2DR84Wa7l08GUxjPSQvLqxzQ4RUf_9-d9D9AUc3eOBj15cGXKvmyW1jhj7onK68DYGhDdcy0N0CMQk4l4wKMV0PvWj5_ERstc6v6JxFe10LLd8i7VwkNiKGgWyc2Svie5UTjPllm3j_ZNz0BofeJV4dbvzNTmilj-kfbVVLeFmarU5nrwYjeBhjEkz8jZbAWuBfjzypmBbV9H2W6-w6CyO7yfJYPmHgjKW2YIV67cBuHhjGzOvVaJDdZFdjvWeA5kV9EjUoU3U-byDOOMmyVi2nztaSQLlqP5gXUbIHtywzq1i21oFnsNszX56YKMxLngt--l1AABh_hH07q5E_RM8n7o8yciICYIPscVnJ-rxOveD2Ux-9QegttA0mJqFPFD1AUT1Wnc6cdMq0vGlxGvrvw2NxFWpacTFHr-o-vbiIiytkItd_EIgub9_qjFw7m00)

### UC01 - Consultar Filmes em Cartaz
![Diagrama de sequência UC01 - Consultar Filmes em Cartaz](https://www.plantuml.com/plantuml/png/RP2nQiCm48PtFSNXEI7r6cX9C6JiKl805_b212UTyKcXz7KoTChO9_2BLObYfuOk1lttVVAlssZ4FYRF9hh81FlcfOObD18qSHx1ph4d1VBGP12_Z44RfOTgMqF3ZaAa2b1XMvik0yww3aCisYpv85MKUOICis0VN8ij6PClNxwLIzQ0zgRs8DtmFLm4gcSdU-zKcNloHpaapICuZc3f_XhxH4-sD9fWpSVX9-EjwCCq1Rd3oBW6EKJA9392YyMuYBdDCZFM7x5KZYiMY_pyxRPU0tswW-iX3jUOM1QWBaYdj03BgEgEpc9vAqsrvZjHOh9EmlX8o6j4E5CFBKNa4-bg4WroXDrDXtSAf-GVIkkqap1SpyG6uBz-rOoVzWr1m8kd4AKo55gpoP_a6Nu1)

### UC02 - Selecionar Sessão
![Diagrama de sequência UC02 - Selecionar Sessão](https://www.plantuml.com/plantuml/png/bPD1ZjGm44NtFiLNhnb1iym2JMkakG3Y03L9pL2IOqSk9qAS1iIQ9IV8nIZ7IUWmpGAfrOhShxx__rsvrqmfZxrtMR9dV7fuznvt-CYUMuc1aZsgpZ-YSzJcc72wdF7WXKFc4qXnERl15AILWKB6e_YU3oTbdkBnY3mT3Ywg9cXtcFZTXohIGF-OU2udu1wjJT5Njn26hkeDl9GWq6Bs5oiwqI66-UV4egv22qwU6iIdpCRQixxriQK3SZatu2_o90cVOvg_9udwfi9WZ44wwbXNftNiO5LhNzUADLs3YPCyYqLOxTabVf63VQXIQaNxlBMEDlRxByTGmErOFGrqhOUE5lSE2xP5f25n0UZrX6ElZCUISFV1-X8pTKNbPbMdly7JArB75qw6nZ4dqlslCkvVNLPqRnJuhqv3dAWtYBSyjjZRwY9nZYaGvczrTIn_NRBYvx25K_Ib95jNMechFKIbjeY99KWhbCfxT7OSEdTlNsFlVmC0)

### UC03 - Escolher Assento
![Diagrama de sequência UC03 - Escolher Assento](https://www.plantuml.com/plantuml/png/ZLCxZjH04Ctx54yJUOATPPTi0hIZ5Ho0pm5gx4AKr1-hgZr2d0P4GCGfV37QuvRNNar0ObkjztdzglnacJGFmRiisJE-FBzvYpkyjpRvRwmucN7CoJbgSrBiJWzuzbAEU0Sod1vSN_ZIIaynunDvsXnK-kRi8nlhXPmhMdVlhfmZwAoY2DGJEeAHdzziDlvERi8KRB4xWh_BkK1fLZOqdLYVulZtmlu-jKDFNRhNsGEPGvzq_AL2--fN8nrXxBcL54an1AG1WSGMNLTn6yg5LRvAIuhPj2Jnqb77u31xGPcyS-INkpUsnofY8xPtm2tr6be_NqEJ2eMfwEOLllPtdAAcLcZLnSjrapbKvElcb6q8QQxNy_XdAfYzCJvC1IrE4bUfjmbMOhkRyCFXiDjIwfJpG5v-aC9o-JR4T9rvmQv7h4ozyiIbUISMOhT4VInHwoQYfTYoNzy7HMVJLONe2mOTr-BMNMq6ukNC-l_zAkUaaHJZpwrDNMCqvUzwt3kEdNigZo7uVm00)

### UC04 - Reservar Ingresso
![Diagrama de sequência UC04 - Reservar Ingresso](https://www.plantuml.com/plantuml/png/VLLHQjj04FtNAGP38QOnjPE6jeQ6cD12Vqffo04cqjWTMEsgiojJSflIZz92Jk6BTIIlLvQivaSVk-_Dl9bvixhn0INKfSa21qDmVpj_3JFuIfvaYmAVx4R8UvTbc0SdC5ajuDOmsK0JG0-hHLPf0CwvGXiElDxPodk5kzxP5znWEJnDoNg7Ttg2_QCe8CjKpEpwa7G9kRDhbX859Eg8LufIfKkW7_p0xMrTEZXRiodf7BncsFrovu1xfVuSjcYSJBLe4-3ZwX9cyC7GPlUq9GC89PblJeCa0bHEG9LHJiNkhyqPuUp-Th6OPW174gBk9Mn9UCqv2f26LKs62om8Ag6IwpA30rHPiG5AmWOrp7Fz7YwWOFGPHEIXM27KU8r09PLO8UIkx2J7N63uWGGBvpEoHTRhs1vwBBTWNpdR2TOgvszJdwvWTY21rOvgLlQHq_OkWtH_F3UXuCIY0Di-U7nyasTpJ4vvARwglOGA1S4wlVPaQFTRWsHaF2LfVGtxqjHeSESCvnoQ-JFE52fOODk2TysW0GPsz14lFO8YUm8J72kpxp7vt3KwhkOl57nI3OOQ3J-Yx5qAmGLiX3RZItelZlLot2j0opfBl_ifx5g7pgT0iFi3T73tytKyaANrxYddVYWbZVeyMRRZ_RVwHv9XHQUbirsxTh-hvlr8moBhiMX2jsFytBYYzdYKzTMHscYRLF4mJtgQbb29oydxNhvct6_QSSTEwtFPSK0ehwCO0EWkXXO-PKmYPMG1uuvAjxo1oif9MuuVuMaYjoQDJ09X_m2euTk41Aw676rE1YLIqQGtTG_lq1416dFG726oMpxkJSGsGqZXOYQz6bL9UYk2eZZle22eVGgZTaxXkdQX3cKxtk3J7q5FN8eP5wMJaduyAgZamD9jJC4RRimuRbgX3VlG6bU38fnz3qNYUY2GvdYQZKyrBVBfi-hoka8P4XSZlpXu6JrrghcfyqO_TMd-0G00)

### UC05 - Cancelar Reserva
![Diagrama de sequência UC05 - Cancelar Reserva](https://www.plantuml.com/plantuml/png/TLHDhXCn3Dxd5DQiOD4d2c9J2VIgIc-7X9x40TmPjxMKdv6JLkYjE0G7G6B1YbFq9fm4PpfXIAVTj8tZpz_tsKuVOq99HsTDucG9lcpUl8S5RD2tP57WcIB92Ot1DWM1Phs4ZMNoYHh02EkbwHNFBVVeKyckOfznZqu1eOgkOvp7EiUUOnBiWXYZL8kFfUGAiAKOKILyOd_0M2vYOyPVcgrwLc0vUm8Pxm4JwzTrkKYMMWv-oWJLWj2EpalL2dGYuHstYkc3FVzMvGWT5SpWybfD35RzPK5jy3iMX-TVvv_XMawvbBgOGPjWyry4UXB7YRjW06v9BFfVlQuIfZ6ie9SmzbA8t3ROc8jbfkTorXkq_8833KpWYkeVkgWlwHDjcNEUofO41V0IYBF4vTqc092D14_hjx209xG711ygxVSKxcubt-vRJ9YE4Jv0MtVRTtdPhs7LgkPEx8U338pZ9c4RN8_-K5pC_T9Nth90C_VqyF3GA4BPpUXzcRq7GUXKbU1BC7VBLVlYTTccfO4_txvVu9hTSvRVp0kntmLnrPQVU9_xKDIyWqNv7u2E8iUXHVLE3zHtPZhdTAYrfhSuF5eWE0MR57l-SNdM9dVaKJ-Eplu5)

### UC06 - Imprimir Ingresso
![Diagrama de sequência UC06 - Imprimir Ingresso](https://www.plantuml.com/plantuml/png/TP0zQWD138NxEONOAYaCSPF2HR0n15oap06KMMKHp8mMQTQ4N2TrJk6BHG7lsCPsCp_YUqzwMbj5hLDEeN9D1E_RfsTOm2wFofaLTcLGCfCGC5PHw3PBs2QcKga3DDWim-W677d4KizmvnOlxRy2Z7wZ6xpHPArkNPYVZhlR2kYBFrY1Pm9w7hWYsvMbKQB8Kl14NpEIEBhS4O_ajSEFj9QNA9KzE-TnS8P5qOCdpzN4e8I9lx77073RAAD6rAD47XuRU0h9vLCqdmtUEWPAHl3wdmDc2qNdUhPHok5tJyaDxsojkoU18f0bKwdItP3RD92wVl9IBssgu-vJeDA7jHzJJdy0)

### UC07 - Processar Pagamento
![Diagrama de sequência UC07 - Processar Pagamento](https://www.plantuml.com/plantuml/png/VP91Rjim44NtEiM7ztQRRWfaKIH8qNLGe0UOK9E00CKHPYYXrocww45yiKvacX2is1ijoFSVnS_f7goGbcb8NOabCNuzVVwAFPvL0fkHufdUQE1SfEie557i7k_mbA8lyGvaUBphHbV444VA1J_PM6TQhwqTTHlxRnwy1_-EBr4nKn95aKAfQ_2PCKakAgHu5Hq8FMDiGlyPnrh6QKuSekGpVilNKo-68Afie-JUARQjCkR5LFacb0lCBn7xHdMK2hul1yp7FxxZQy2rxDZwBahPwFZl-5SW8wiRb3wzK0xYUPUk7EInsG7Vk53Nck-X_1RDdTewsz3r9OPOeYB8i4XpOUSu6UF7Uo6hW7AOR7FgnOlkjlpXSDXzd95NTeYrDOqiirXz7vaGA0TEf1u_TL07y1nOymi3R5vHcVCSIHjLhtTMrJDyutgRLgXCXl7ZBZZttUbRU_k2VHs1cDiGtEZiagsDtPZWPNPFmy9GBg9PG8AHp0Gzryjoxhi7_qn3-Wy0)

### UC08 - Verificar Disponibilidade
![Diagrama de sequência UC08 - Verificar Disponibilidade](https://www.plantuml.com/plantuml/png/ZO_DIiH03CVlynHXJ-sXe3Vn8BieFe2ezzX6o9BEb2Jj-omUFFaKVJ6dxhPkGM4lGyZyFtxPcgDwtqfmTY5ukR-wWGfUIVcTQrHuOEjIv3SMRh2X4BgivvexZ0uxCugULhidCj81GpWDKDtDkbkeKxHU71Nmk3AW5fpQBe4IIZWfLwPXWJ6QVOMLGERZeHW_KWd2WrBUXO2oO47pmpv-3IG1uAzeFJBgyqIWg8nj_g2YNEddcoQz9uqJYQFt1fkp2g0vhadWwy1D8362nzqrL4lmVZpyElujt1FxL9Y4wpoDd_dWI-audl5bXjY4RNxwLhu1)

### UC09 - Aplicar Meia-entrada
![Diagrama de sequência UC09 - Aplicar Meia-entrada](https://www.plantuml.com/plantuml/png/TLFDYjH04BxdAOe1GP6PsHLNS43POL3mu4Nm0MgIob3IwOhLtM5VHpnuykGZvCMiPdwsDvaz93ILx_kgwliwzo6YnjIvAd9q1DztD-zX2TlUSOqANubnIJugDbXLM4THM6nlOUVOghG03B2zhNhJu9fxz16-KI0Ty4djIwJkNA-CllnuXcs0VIlQeSBFHD1OzGT2LxOz8OrahJOGn9biD4ADaGwYZD08xCbJE_wjMQOD0Zcoid_2U4aXfiPce_MxkzVhNgMb44HVFT-kaJfrDfH0BPqHzXZ7VphlXxtQqHjbVTojv9Ro5yQ0ZXlKa-w0uv_njvZTBiAkBC4m_ih82g2GASpTW3NBkMK3Fntz-n0r4T1bzmcx6FiOkKLGIzwo8RYxUM7eYIFsjMNjIkYHsqX5BX1ypePC9cT_TVQiHm-yPuN5CuJLQhMOaomkfPeK94ML8Xj9HZre3jCEjhAYCxfvURwqZm5DoOO3cAIfH7xWMJW0frNVmF8YB7t4ekbSp0H2e4DYpN9N1xqUJeU6q7nHH46K3-pdwEoBKXJrg802hKiFal_ejtbStrJvkRTNwjn_)

### UC10 - Conceder Ingresso Gratuito de Aniversário
![Diagrama de sequência UC10 - Conceder Ingresso Gratuito de Aniversário](https://www.plantuml.com/plantuml/png/dP71Yjim48RlUeeXzmrDUykXR1hQqaiFXJx0H1gRARB6Z6HJzcsMdbhOf_2BTMnMh4C2o-PYiCJ__TzexdD1BKCNNU4I2NuUT-_W3ewIF0LI-9PQfPm5lYgMWOj083Wa7adpzAWipg4leh0vxE0OcLAX3M26myxrHcRFFQPIhyxEVb0c7T4vYzvzhBzxo1BPita1r_PsrOxhzkUGvMl57koM6_QMxwWxgOK4UXLZT3W36WuKEM2Wr-BJl_p6nY4lTITAr4nFdWMIB1pdC1Ru7AcTdaQA3kgyPwC_a1fan2WwO-hy3k1QhLBBkUYrslF4ORS7BwbXxL3NSjX7qmxYA6Q2B_CYuFlqHy0ESKORDBsytYMLVlE9RVkVNfY1SozfcTBub313ZuhFoyZRxNPpoP66b3ofQNQzefMQQgnQxwlMzFVwKcxpghkvHUc3AQtWlwP760f7Vi0WbVkwrcMISg4b_0QZ5DozVOOk_WS0)

### UC11 - Reservar Melhores Assentos por Antecedência
![Diagrama de sequência UC11 - Reservar Melhores Assentos por Antecedência](https://www.plantuml.com/plantuml/png/lP8nhjim38PtdOB8rA88KgUTF1G9WdPhXmOz06FHAG5ACYZPo7sw5SWfVB7AZft63tdhMsI0vF_zFsNjOa9DhHEJE0d1hqDHm1f-KYJjKE4xoU-W564V8_aK8ZH1OUyJLMJxlxvYD0Qhb8khVG47OSgz5M1M5AR9RAwuGP_crg9sf1WnB4iek2XCniPC05X_cLabTAHSSuKAZjn90zW0ZOQpeaEm13LR4hPewK4-sJwgyN4XSFtLymsm1SiOZK59y5Net5yx4WCJOeIXO0d29r9Kg33HEMZMv1pak7MR_Ar9QI1pD30Aid1UhCmsCU6umtpPU6ijfeiIwC8drdiNtbN1TmCu-8o63xFbUmDvVk4HIMYSkyiDIIJuDip1Z_vFU7A3hzWtvF5_HbY4U25HIa47qrhQIm0VrA7aH_RnqoOVd-SCsvpXIExvRtY35DwQNJvQ9_y0)

### UC12 - Gerenciar Filmes
![Diagrama de sequência UC12 - Gerenciar Filmes](https://www.plantuml.com/plantuml/png/ZLF1RXGn3BtdAwmzmQ6jkjnwG5qLoXt41vZ4snebySZ9ZFYTYWDVm1ViZ-4qOUYMBU8oCxAVttlZbroNYbhd55tb6Wa-trnkuGm-a5BsZ0gt71CLvz1NKTZijh0BYJENgXX4Du05Tbit6GjxdZ3NFd9K-KIbe3YtC-wtlNy5w5jLuMve9SfL813ik-GZpF16VmMHcz7H0ey1ku5op4d5Ixmd1JpyE7mNUFKRgHSKkDfBASb2-lfuZlDUD5aROoL3Gp3krS8AMp1oMBlEmF2kVS9oUB2ErG1QyQE9T0D58diJ3cWjW9MmRyGOINdFtZYBLGu_gS1-hhDY0QoyO6FidBVDy23mabRyKnmCtYUMxmyFBq2FzalV-0jRlhsOc_L90jcemkFAECZv-Vbcp9xAPbLPEFivOXXA50kPhvSSFzzn3sX4VKhEIzvp2mja8kr1Tw4St92xwR7BnVl07N1Agc1SZAPdRZ3pvlvFheirASlb4ix6CU2y7eJJ-oLLWqsYu35DyWordXPhYofrhQ7oPat_gqNwvmQuVoPvZ8KiK2mjIhZvItksYzWiZes1mPNk3ArYf6qgLtBjlw_jCQVu2m00)

### UC13 - Gerenciar Sessões
![Diagrama de sequência UC13 - Gerenciar Sessões](https://www.plantuml.com/plantuml/png/XL9BRXD14DttAKfU9PBjA67d1OeL2SGAYOW353tbkAJ-cEeUYoM7u09H5fo04ynDE0dLpQJL3aPPUDoQUPzNh-isPPGyUcSoPqVm-Uxw3QpWFGa5ooXmJobDloWPWpP7WSNs1hQ3vy0f2mvH5e09jZVce3fi-O0XLnB6qrVeyEJ5X_0W2el6gEBgxSpP0Dfo47YO8tWAES90a9wJ_4KgHsqtuBYCqBw2nG5hD4mlb2dPwFOaWDFFwId2nJDKhcZWh7z2FXv9BbyGEUoYUFsEBfF2OT314rpis7bQGjBHbh2FCZqAnoMCWHL0brtGClq6L9ftR5N0nh1pNETgHA2gP8owmBljDQpWxWpigeAWIlOXcoG5FTQxKTHfL_IDlx30ufmk_Fx-emQ0QCU3DhCuJ--gdYqnP4h_WDEeDyZIz5CDdg1s633p4PCXbqXvlgLNcJf-kG3uLAzZDdoA2lB0yzeaRJCCFCJYt7ctlUhgxaaSxEXmfZLmN-H-UdpTfM_pVeuNokCGwweAr5tY0TVhzUB_vA-ZjbVNPNP2nTPMFXv8vaNLa-U2wdMwn5hiiMrlg-kCQMcPooP3REeAKsDJVhVw6Btx0m00)

### UC14 - Gerenciar Salas
![Diagrama de sequência UC14 - Gerenciar Salas](https://www.plantuml.com/plantuml/png/RL5BZXGn3Dtd55QlO36DPiHg5cXQ85YoG1p09EuPIwcuPATAN6V4WXDmWheOJhg6wW8MdIx5x-ENzs0LjKv33fLh9lZw_lOjtC0dKYgHKU4BPhGGC5PHE9pku9G6BcnLCOaU00rETs5q4OuyOgcTiRyWCvGGd7pphe_l0MExL7XSZGOgLI0HMFTh80Uxz3raRZjU1Z2AmbGuOQAT7bcK_4GAkFnSVWYyYfYmBQblA77rFwL1PjBNEngNi-ZWKyoL70mkBEPdj-jhRE0PCwSNJ0XEWOzD33vq4fTvUSxSPW3N0UWxVsC5ZsZuw4UBIQho5ymaS-GMDOegzHo1ij6gVwL-iRvrwouGPG0Zi-KN6M3b6HjeKxyKu058-SmHTKEVfphf2xp3jhnFo_C6R9jxy0xzRxn3loljyKTfHP92VtDEU3mU3plUrklcC7E9KrujUkxF8-dQftyDN5sgo-nxCSppLd1lxX-pAENChMcGFwBDfgJGVWz-J4F-3G00)

### UC15 - Gerar Relatórios de Vendas
![Diagrama de sequência UC15 - Gerar Relatórios de Vendas](https://www.plantuml.com/plantuml/png/VPInRXD148RxUugHgqJ2jX892XSeLi28nWK8-c5tx8oqjtEQsRDGtYQYG1HKUOHxCNRlOfqljcXShEUV_zl_N_lE4cfgwr0bJe7Ws_tDMvZ39r9K-481K_UiB0QUu3j5ZrPLw98epDQtiFOrHxQaw4LdW0Rhswf1JUoumPZo1YCzuEJiAvcXJ8us76gQ3c70gieMy_V79Ij0LwGAtPCI5Y8T0QkNgIp842kWd_o35H8tOd1bMTZzakMk62qD0oxRo1uzBHlIxgzukNxjPXJ8iKGSjhp-ckDEj4Q5l3Ff562uuWgSH6j3GeK7jXxKIU4-z4a2WGuQgmP5rWvP35896L6X4OKUV9pgypaREhtN2Sx8x32uDfGBIS807FTAPc9lG0iH4DJ4EATO6iL-fyD4Ur7ERM18y06z651qqi-8LG2Nan-x6J6zd8_BZfGSQTPbluJTxrnKHS48DkiRcCEMuaDRGr53Wuf3uFzndfqgHjJ5OZ4xrrYx9yqY38dqf4sAVhZnflH-ybC0R9FKtLCgNMBJFHkaxe_M7FjgNrvQDXhAckGV9V6E7UfnhE2Hbh0dPADYtqFfPNUFezr5DMgDZnHHi_Fu12-KuDaQYJczm8-eGyp7nxkaU621zVR-y_OZNEL_Y7VNVJnt-QEjmpy0)

### UC16 - Enviar Relatórios para ANCINE
![Diagrama de sequência UC16 - Enviar Relatórios para ANCINE](https://www.plantuml.com/plantuml/png/bLInRjj03DtlAmXFDWeFYO4E7eeOGG9aoU2W7q1BjCf2EXeysiZl11sATkqdwCTAoP5G2LQJB09EV8zyVEJTJJ9KErPbCBQIuEljrMTOmbqyCIfigKIh_oXBWWCgmkRnzk7nBWJCJ1HccslOv1L7JgQOYyu04sok-_0AdZn4LKSynrV1SnbdVC1ey4GfeGn-RIcHdd3qpwM8ifmp3M8FqQXGpB0BXdDIM7xfSgs1urwqGWMhVwbh5iW9akFg7n9UKKuO5rf3GUeixQq0sIaNtG6F9bNzubeak1sN4c3Mg56eNvHw2qSe9xfnQoZTBuSUAEOKZT9KJgKJfu6iAMGcSSzju-H3bKk5n_wjMoXg3vrhNsOH1kXE-ceDfXXJnSO3LG5BQzT9UYMv1736vSeTgFxfSsb6f9JHZYjtew6D2HFc-34sDggOP58wSuYQ8BfB6MgZraNEU_R2wHFp6hjBPIAutrp14kwn_8OGqJ7LCNBMjlJk7aYrRTyBqBFpfYNGC-zOONQvNdFgRjyxNL0gsWkjOEuw6f_QdB1l2aphY6Ayzx8AIcx6y6u05XHpp7k7V5d_ULwSyftO_hyQBURSr2VWwCuarrd_ZXbZ-l1kD0Qd3-t5jjCuNxmv3LBjtinCLAcR_UGbLUMYuRsQXnhSht3Z7t-S_m80)

### UC17 - Consultar Lista Oficial ANCINE
![Diagrama de sequência UC17 - Consultar Lista Oficial ANCINE](https://www.plantuml.com/plantuml/png/ZLCnRXin4EpvYe6gfB0CoqK05O4FGWGO29J2o0Csv4fcm2Cl94y8_9e4ALpv5VUnhCYxs5ACA8qAxSpEx0pl9cMAkM-ToZOxnkVLrJjSO1LywfsCyD4A0f-sLbjoQ3Qhsyq7fKZd435h5cXCQxrW8faGPw24Pl5dV8qx6N5B8x7EhrKdchAm8vznwpFl8caA5NCqN5lNyj4_T-H8AH6-UFyNTGaz-IwsHzTltgfJfB35nnAHl_KsMI70bKD3fS0mjWVj9BUwFAOGMSxoP4XqMgHUSqf1GJQzxgRYJtSBuLKxYoNuk_rY9_Yp1Q3SMzDO8h5ZRSDXSHwUSkz2WaGa39kwu6M5imyK9rg9R0aIhzg-v0m_4fBCqcNlhI5pK2hW5rgHTwNSUBnyU1n-1NGXbWKA5JwT8IriRMo553gE5QpO9SQwkS85rkJk2Rx4ADAw8ageDO_p4St-iICWSTpRuJ70J2zlFf_F9WlIrbZedXui_N-15NxUsuRzVT_MziRSXgSzksh2c_gcreTsnk2-3Z-XoL3zZk3bXFhXdE--S4LPu6Bhj8h9qXcPsIdFi1oiAJvlJc24BKVM746vf_B8BktEXqYnd7KZFtthVWC0)


## Diagramas de Estados 

### Classe Usuário 

![Classe Usuário](https://www.plantuml.com/plantuml/png/ROyz3i8m34Ptdo8Z8FK23AWm8ovGTJ1DL5bAOr7RkGzduMAGCa1_nYzvxqckHN5K6o8qXzPLrSdTaC-c8Ibrw1bWlYVaVnzTnaWUnoltb7PxruXrGOJRBVCI-M7XwvKVLCeeZb9-FKj509RAh5Ayo3sRmY-rIVaWzuaWNZS_joxyybA7v6s8Rm00)

### Classe Cliente

![Classe Cliente](https://www.plantuml.com/plantuml/png/PP2n2i9044Jx_OehLSWFM4Z4IX0BMx4OSaSuI7R3vfBlTxaOafXkPlVTt39bdw1fQ1jwdpwkA2xk2RaEAN2A5RmGRNr6rlxiHZJHGnzIHpuSYVxfL-Y3Zk0CwsJqphN9Nkp1ii_uJTfvQNo76gt5YKEdCbhcZWTNeO67qTMar-cqpjiVFzZKHFmHMppprWijVyoJCh2trhu0)

### Classe Administrador

![Classe Administrador](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJy4lCTomjISqhoKnEJCdduaBbWvKWywqKSlBJC_EuqDMufnQbvYLd9kQ1rQH3UKLkcJcvgSNwmQd5nVaWEZ4diPYB2wuOgXde5Yw7rBmKeBi0)


### Classe Filme

![Classe Filme](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJS4vCIImkAKfCoUVYGh31Kgwvpa35YLKALWfbcNd9fJc9HS6fnSM9-HgQ66eTIqeJSpAhW5oWwaGefyW5o2y6geQRH5Wf5qoQi2FGbRhb5oN251Cs61W0N0WTS267rBmKOFW00000)

### Classe Sala

![Classe Sala](https://img.plantuml.biz/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJSCaiBiZFoonBpU52CC4oG1LSw68--IM9AOaALWh5gRcEnSMfUINvnLnGGLJJW6foCfDIYnABuEgu75BpKe1s0m00)

### Classe Assento

![Classe Assento](https://img.plantuml.biz/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJyCaiAqhb0fDWFb0SYHUKMfnQL9QOagzWfP2PbvcScLW45U92I84K-YUN5a2K5IIc9-QcvfNaAoJdvwLb5kK2XT4KGyotKaXEp4jEpO6evkA26O6i8YqpDpYrk3WndyiXDIy5w6u0)

### Classe Sessão

![Classe Sessão](https://img.plantuml.biz/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJSCaiBiZFoonBpU7YGh31Ka6fnQb5POafYGfM2eb9HPb5OQbvAObS266G8f_y4eYAujHSn-BYrBoI_68kg218tZNN4QWf5oGELbHSd9ZlcPUPd9c8OHHUfSYI8CKWzyCKkUObfnOLWLHnEG2T2FGU0000)

### Classe Reserva

![Classe Reserva](https://img.plantuml.biz/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJS2xAJ4n9vEA2q12X_BoqpA9S4DTA8HdAAGfABKujAalKq4HHcfYNd9e3LGbX8odaGZ89f1fe9nT21qp48JKl1UWY0000)


### Classe Pagamento

![Classe Pagamento](https://img.plantuml.biz/plantuml/png/PS_12eCm30RWUvuYHntq1JmCzmRYTUmGQX11MsbJdtyHMt7prlpvGfCs2WL9omplnmUDUMB7Rc0d351UHnoyvHZ93HuRN7CLEXffKUIh6gva7tcfskZmXI5PdiJVRCysnNyRSWXmjgyRgdNw0OeM9DD6lqmFWdE54hGwgzT-0G00)

### Classe Ingresso

![Classe Ingresso](https://img.plantuml.biz/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJS4yjIap9v-A2q60XrzpCaamWsqeX5SGgyinBBqejBixNqEI2IO6KUUOMW8M1wZA1pCnS59LmMP3Iq9BCdCmga3t81ZWdvYMdve14uIomECXfLWh94B7SrBoIV2wu0KWAIk66EgJcfG3z1000)

### Classe Relatório

![Classe Relatorio](https://img.plantuml.biz/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJS2xAJ4p9v-A2q60XzzGY4DDA8Ht8A0fApKaioI_ApDVGv8BAW1IvvfLbGbIbWbX5Zdd9cNcfG3Km2P0Ye2LS3gbvAK0B0G00)

### Classe Integração ANCINE

![Classe Integração ANCINE](https://img.plantuml.biz/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJy4lCzya4YgRamuLSYmjIYnA3KdCII_ABClEvqFWGDNbbcK0z2bOAgI1M4LnMGvCBOIHOAOnjfP2PLvnQb5a45d3BpCbDBGQgXfa4KWfq0HUoLX3V8JKl1UXW0000)
