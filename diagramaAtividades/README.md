# Diagramas de Atividade - Sistema de Reserva de Ingressos de Cinema

Este diretório contém os diagramas de atividade em PlantUML para cada caso de uso (UC) do sistema de reserva de ingressos de cinema.

## 📋 Lista de Diagramas de Atividade

### Casos de Uso de Cliente

| Arquivo | ID | Descrição | Tipo |
|---------|----|-----------| -----|
| UC00.puml | UC00 | Realizar Login | Autenticação |
| UC01.puml | UC01 | Consultar Filmes em Cartaz | Consulta |
| UC02.puml | UC02 | Selecionar Sessão | Seleção |
| UC03.puml | UC03 | Escolher Assento | Seleção |
| UC04.puml | UC04 | Reservar Ingresso | Transação Principal |
| UC05.puml | UC05 | Cancelar Reserva | Transação |
| UC06.puml | UC06 | Imprimir Ingresso | Ação |

### Casos de Uso Internos/Extensões

| Arquivo | ID | Descrição | Tipo |
|---------|----|-----------| -----|
| UC07.puml | UC07 | Processar Pagamento | <<include>> UC04 |
| UC08.puml | UC08 | Verificar Disponibilidade | <<include>> UC03, UC04 |
| UC09.puml | UC09 | Aplicar Meia-entrada | <<extend>> UC04 |
| UC10.puml | UC10 | Ingresso Gratuito de Aniversário | <<extend>> UC04 |
| UC11.puml | UC11 | Melhores Assentos por Antecedência | <<extend>> UC04 |

### Casos de Uso de Administrador

| Arquivo | ID | Descrição | Tipo |
|---------|----|-----------| -----|
| UC12.puml | UC12 | Gerenciar Filmes | Gerenciamento |
| UC13.puml | UC13 | Gerenciar Sessões | Gerenciamento |
| UC14.puml | UC14 | Gerenciar Salas | Gerenciamento |
| UC15.puml | UC15 | Gerar Relatórios de Vendas | Relatório |
| UC16.puml | UC16 | Enviar Relatórios para ANCINE | Integração |
| UC17.puml | UC17 | Consultar Lista Oficial ANCINE | Integração |

## 🎯 Fluxos Representados

Cada diagrama de atividade inclui:

- **Fluxo Principal**: Caminho ideal do caso de uso
- **Fluxos Alternativos (FA)**: Caminhos alternativos e tratamento de erros
- **Decisões**: Pontos de ramificação baseados em condições
- **Pré-condições e Pós-condições**: Anotações explicativas

## 📌 Convenções

- **Retângulos com bordas arredondadas**: Atividades/ações do sistema
- **Losangos**: Pontos de decisão (if/then/else)
- **Círculos**: Início e fim do fluxo
- **Notas**: Informações adicionais e contexto

## 🔗 Relacionamentos entre UCs

### UC04 (Reservar Ingresso) - Caso de Uso Principal

UC04 integra múltiplos casos de uso:

```
UC04 <<include>> UC07 (Processamento de Pagamento)
UC04 <<include>> UC08 (Verificação de Disponibilidade)
UC04 <<extend>> UC09 (Aplicar Meia-entrada)
UC04 <<extend>> UC10 (Ingresso de Aniversário)
UC04 <<extend>> UC11 (Melhores Assentos)
```

### UC03 (Escolher Assento) - Fluxo Auxiliar

UC03 <<include>> UC08 (Verificação de Disponibilidade)

## 📊 Estrutura de Fluxos de Cliente

```
UC00 (Login)
  ├─ UC01 (Consultar Filmes)
  │  └─ UC02 (Selecionar Sessão)
  │     └─ UC03 (Escolher Assento) ──┐
  │        └─ UC08 (Verificar Disp)  │
  │                                   ├─> UC04 (Reservar Ingresso)
  │                                   │    ├─ UC08 (Verificar Disp)
  │                                   │    ├─ UC07 (Pagamento)
  │                                   │    ├─ UC09 (Meia-entrada) [opcional]
  │                                   │    ├─ UC10 (Aniversário) [opcional]
  │                                   │    └─ UC11 (Antecedência) [opcional]
  │
  └─ UC05 (Minhas Reservas)
     └─ UC06 (Imprimir Ingresso)
```

## 📊 Estrutura de Fluxos de Administrador

```
UC00 (Login)
  ├─ UC12 (Gerenciar Filmes)
  ├─ UC13 (Gerenciar Sessões)
  ├─ UC14 (Gerenciar Salas)
  ├─ UC15 (Gerar Relatórios)
  │  └─ UC16 (Enviar para ANCINE)
  └─ UC17 (Consultar Lista ANCINE)
```

## 🎬 Como Visualizar

Para visualizar os diagramas em PlantUML:

1. **Online**: Use [PlantUML Online Editor](http://www.plantuml.com/plantuml/uml/)
2. **VS Code**: Instale a extensão "PlantUML"
3. **Exportar**: Converta para PNG/SVG/PDF usando ferramentas PlantUML

## 📝 Notas Importantes

- Todos os diagramas estão sincronizados com a documentação em [USECASE.md](../USECASE.md)
- Os fluxos alternativos estão representados para facilitar a compreensão do comportamento do sistema
- As decisões são claramente marcadas com condições
- Pré-condições e pós-condições estão documentadas em cada diagrama

## 🔄 Atualizações

Sempre que houver mudanças nos casos de uso, os diagramas de atividade devem ser atualizados para manter a consistência com a documentação.
