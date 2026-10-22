# App de Tarefas Diárias: Ideias, Decisões e Fases

> Status: fase de brainstorm. Nada foi desenvolvido ainda.

## Objetivo

Criar uma aplicação pessoal para organizar tarefas, compromissos, metas e hábitos, com versão para **PC** e **celular**. Uso exclusivo do autor.

---

## 1. Stack e organização

- **App do PC:** React
- **App do celular:** React Native
- **Back-end:** API em .NET
- **Banco de dados:** SQL Server

---

## 2. Infraestrutura


### Autenticação (adiada)

**Decisão atual:** por enquanto, **sem login, com acesso total ao app**. O tema será retomado depois.

**Para o login entrar depois sem retrabalho:**
- Os apps chamam a API por uma **camada única**, de modo que acrescentar o token depois seja mexer em um lugar só
- Desenhar os endpoints já pensando em proteção, para exigir autenticação em tudo de uma vez

---

## 3. Telas e navegação

### Áreas do app

| Área | Papel |
|---|---|
| **Início** | Calendário e a lista "Hoje" |
| **Quadro (Kanban)** | Criação e gestão de tarefas, com drag and drop no PC |
| **Compromissos** | Lista própria, apenas compromissos |
| **Metas e hábitos** | Área própria |
| **Revisão semanal** | Gráfico e mensagem de motivação ou alerta |
| **Busca geral** | Pesquisa em tarefas, compromissos, metas e hábitos |

### PC

- **Menu superior** com as áreas principais: Início, Quadro, Compromissos, Metas e hábitos e Revisão semanal
- **Busca** sempre visível no menu superior
- **Ícone de configurações** no canto: tema escuro, exportar e importar dados
- **Menu lateral** com as opções específicas de cada área:
  - **Início:** filtro do calendário (tarefas, compromissos ou ambos) e escolha entre mês, semana e dia. **Sem botão "+"**
  - **Quadro:** "+ Nova tarefa", "+ Nova coluna" (dentro do gerenciamento de colunas), **Selecionar** (seleção em lote), filtros por etiqueta e prioridade, gerenciamento de colunas (incluindo a marcação da coluna de conclusão) e acesso às tarefas arquivadas
  - **Compromissos:** "+ Novo compromisso", próximos e passados
  - **Metas e hábitos:** "+ Nova meta" (abre a escolha do tipo: com prazo, numérica ou hábito), metas com prazo, metas numéricas, hábitos e concluídas
  - **Revisão semanal:** sem "+"; pode usar o menu lateral para navegar entre semanas anteriores
- **Tela inicial:** **calendário à esquerda** e **lista "Hoje" à direita**


### Lista "Hoje"

- Tarefas com prazo hoje e as **atrasadas** (em destaque, no topo)
- Compromissos do dia, em ordem de horário
- Hábitos do dia, com o **check-in direto na lista**

### Calendário

- **Somente visualização**
- **Tarefas aparecem como contagem**, uma vez por dia (ex.: "3 tarefas"), incluindo as de horário específico. Clicar na contagem abre a **lista das tarefas do dia** (que pode ser ordenada por horário), e dali se chega à página de cada tarefa. Sem ícone de check e sem risco nas concluídas no calendário
- **Compromissos** como blocos com horário (início e fim) na grade, clicáveis até a página do item. A grade de horários é só para compromissos
- **Tarefa às 23:59 é "dia inteiro":** vale tanto para o horário padrão quanto para quem escolheu 23:59 manualmente. O **atraso só conta depois das 23:59** do dia do prazo
- **Filtro:** mostrar só tarefas, só compromissos ou os dois
- **Visões:** mês, semana e dia no PC; no celular, mês compacto com a lista do dia embaixo (ao tocar num dia) ou visão de agenda
- **Metas com prazo** (e as numéricas com prazo) aparecem na data final, e os **marcos** com data também. **Hábitos ficam fora** do calendário

### Lista de compromissos

- Ordem **cronológica**, só com compromissos
- Botão "+" de criar novo no **canto inferior direito** (no celular; no PC, também no menu lateral)
- **Compromissos passados ficam escondidos por padrão**, com filtro ou aba de "passados"
- **Agrupada em blocos:** "Hoje", "Amanhã", "Esta semana" e "Mais tarde", mantendo a ordem cronológica dentro de cada bloco

## 4. Funcionalidades e regras

### Tarefas e Kanban

- **Colunas personalizáveis:** começam com "A fazer / Fazendo / Feito", com opção de renomear e criar outras
- **Prioridade e etiquetas:** cor ou tag (trabalho, pessoal, estudo, projetos), com filtro no quadro
- **Subtarefas / checklist** dentro do cartão
- **Notas da tarefa:** texto e links
- **Data e horário:** horário padrão **23:59**, alterável na criação
- **Registro de tempo:** lista de registros por tarefa (data, duração, origem: Pomodoro ou manual)
- **Movimentação no celular:** pressionar e segurar o cartão abre um menu com as ações **mover para [coluna]**, **concluir**, **editar**, **apagar**, **arquivar** e **selecionar**. No PC, drag and drop (as mesmas ações num menu de contexto ou botão no cartão: a confirmar)
- **Arquivar manualmente** (sugestão, a confirmar): apenas esconde, sem alterar o status, como no arquivamento automático

### Coluna de conclusão

- Uma coluna é marcada como **coluna de conclusão** (padrão: "Feito"). **Só pode haver uma**
- **Entrar nela conta como concluir:** o app grava a data de conclusão (com o horário do dispositivo) e começa a contar os 7 dias para arquivar. A ação "concluir" do menu do celular é só um atalho para mover o cartão para essa coluna
- **Sair dela reabre a tarefa:** a data de conclusão é apagada, e a tarefa deixa de contar como feita na revisão semanal e nas metas
- **Apagar essa coluna** exige escolher outra como coluna de conclusão antes
- **Trocar a coluna de conclusão** não mexe nos cartões antigos: quem já tinha data de conclusão continua com ela
- **Marcar todos os itens do checklist não conclui a tarefa:** o cartão só vai para a coluna de conclusão quando o usuário mover
- Funciona offline, pois o app sabe qual é a coluna pela cópia local
- A recorrência usa a mesma regra

### Seleção em lote (Quadro)

- Ao ativar "Selecionar", cada cartão ganha uma caixa de seleção e aparece uma barra de ações em lote
- Ações: mover para uma coluna, concluir, arquivar, apagar, definir etiqueta e prioridade, alterar o prazo
- **Apagar em lote** pede confirmação com a contagem (ex.: "apagar 8 tarefas?")
- **Ocorrências de série recorrente** ficam bloqueadas para as ações de alteração (prazo, apagar) no lote, por simplicidade
- **Offline:** em lote, só **concluir** continua liberado
- **No celular:** "Selecionar" é uma opção do menu de pressionar e segurar, com o cartão tocado já marcado

### Arquivamento

- Tarefas concluídas são arquivadas **7 dias** após a conclusão
- Arquivar **apenas esconde**; não marca como concluída
- A tarefa arquivada continua na busca e conta para metas, revisão semanal e estatísticas
- Deve existir uma tela para consultar as arquivadas
- Exige guardar a **data de conclusão**
- Quem arquiva é uma rotina da **API**

### Tarefas recorrentes

- **Período mínimo:** diário (sem recorrência por hora)
- **Quem gera:** a **API**, guardando a regra de recorrência e uma marca de "gerado até quando"
- **Gatilho:** sempre que restarem **menos de 3 ocorrências futuras**, a API gera mais **7**
- **No quadro:** o cartão só aparece **no dia anterior e no dia da ocorrência**; as demais existem no banco e aparecem no calendário
- **Ciclo de vida de uma ocorrência:**
  1. Dia anterior: cartão aparece no quadro
  2. Dia da recorrência: continua no quadro
  3. Dia seguinte, se não foi feita: fica como **atrasada**
  4. Depois disso: é **arquivada** (marcada como "não realizada", sem contar como concluída)
- **Arquivamento das não feitas:** no dia seguinte ao atraso, por rotina da API
- **Editar:** o usuário escolhe entre **uma ocorrência** ou **todas**. "Todas" alcança as futuras e as não feitas ainda visíveis (atrasadas incluídas), sem mexer nas arquivadas
- **Encerrar série:** botão "Encerrar tarefa recorrente" na tarefa, que encerra **a partir da atual** (a ocorrência atual permanece). A API grava a data de término, apaga as futuras já geradas e para de gerar novas. O app deve cancelar os lembretes agendados das ocorrências removidas
- **Diferença para hábito:** tarefa recorrente tem cartão, prazo e vai para o quadro (ex.: pagar conta); hábito é só check-in diário com sequência, sem cartão
- **Compromissos não têm recorrência por enquanto:** cada compromisso que se repete é criado à mão

### Compromissos

- Existem só no calendário e na lista de compromissos
- Têm data e hora (início e fim); não têm coluna nem status
- Criados pelo botão na lista de compromissos
- Sem recorrência (por enquanto)

### Metas e hábitos

Três tipos, tratados como uma meta cujo cálculo de progresso muda conforme o tipo:

- **Meta com prazo:** progresso calculado a partir das tarefas do kanban vinculadas (proporção delas na coluna de conclusão). Suporta **marcos** (etapas intermediárias)
- **Meta numérica:** valor atual e alvo (ex.: 3 de 12 livros), atualizado manualmente, com prazo opcional. **Não se conclui sozinha:** ao chegar ao alvo, o app avisa e o usuário confirma, podendo continuar registrando
- **Hábito:** sem fim; check-ins diários, com sequência (streak) e frequência (todo dia, X vezes por semana ou dias específicos da semana)
  - **Regra da sequência:** pular **1 dia** mantém a sequência; pular **2 dias seguidos** zera
  - Em hábitos com frequência, só os dias previstos contam como pulados

**Estatísticas:** gráfico de constância dos hábitos e histórico das metas concluídas.


### Revisão semanal

- Gráfico e mensagem de motivação ou chamada de atenção
- Mensagens **fixas no código**, em um array, cada uma com um valor mínimo que define quando é exibida
- Base do valor: **taxa de conclusão** (concluídas dividido por planejadas), com possível comparação à semana anterior
- Ocorrências arquivadas como "não realizadas" entram na conta como não feitas

### Pomodoro

- Temporizador de foco ligado a uma tarefa
- Pode registrar o tempo na tarefa, mas o usuário também pode **inserir o tempo manualmente**
- Cada dispositivo tem o seu cronômetro e salva o tempo ao final (sem sincronizar cronômetro em andamento)
- Guardar a **hora de início** e calcular o tempo decorrido a partir dela, para não perder precisão com o app em segundo plano

### Sincronização e uso offline

- **Sem conexão com a API**, o app **não cria nada novo**. Só ficam liberadas **três ações**: **concluir tarefa**, **marcar item de checklist** e **fazer check-in de hábito**. Tudo o mais (criar, editar) fica bloqueado. Desmarcar também vale (assumido, para permitir desfazer um toque errado). Tudo é enviado quando a conexão voltar
- **Horário:** guardar o horário do **dispositivo** no momento da ação (a data de conclusão alimenta arquivamento e revisão semanal)
- **Fila de operações:** lista de ações pendentes (ex.: "concluir tarefa X às 14:32"), reenviáveis sem duplicar efeito
- **Conflitos:** operações sobre itens que não existem mais no banco são ignoradas; nos demais casos, vale a atualização **mais recente** (ex.: marcar e depois desmarcar um check-in, vence a última). "Mais recente" depende do relógio de cada aparelho (aceitável para uso pessoal)
- **Cópia local:** o dispositivo guarda uma cópia dos dados no momento da última ação, para consulta offline
- **Lembretes** já agendados no celular continuam disparando offline
- **Itens apagados:** marcar como apagado (em vez de remover de fato) para que dispositivos offline saibam da exclusão; a API limpa de vez depois de um tempo

**Avisos na tela:**
- **Faixa fina no topo** enquanto estiver offline, com o horário da última sincronização (ex.: "Sem conexão. Exibindo dados de hoje, 14:32")
- **Indicador de "aguardando sincronização"** nas ações pendentes, com a contagem da fila na faixa (ex.: "3 ações aguardando"). Some quando sincronizar
- **Falha ao sincronizar** por motivo diferente de falta de conexão (ex.: item apagado): o app avisa, sem falhar em silêncio
- **Botões de criar e editar desativados** offline, com uma explicação curta ao tocar

### Exportar e importar dados

- Formato **JSON**, com exportação e **importação** (restaurar tudo se algo der errado)
- Incluir **versão do formato** no arquivo, para ler exportações antigas
- Usar **IDs únicos (GUID)** nos itens
- A importação **substitui** os dados existentes. Proteções combinadas:
  - confirmação clara antes de executar
  - **backup automático** dos dados atuais logo antes de substituir
  - validação do arquivo (formato e versão) **antes** de apagar qualquer coisa
  - tudo em **uma transação**, para não deixar o banco pela metade se algo falhar
  - depois, os dispositivos descartam o cache local, cancelam os lembretes antigos e agendam os novos

### Conforto no dia a dia

- Tema escuro
- Busca geral
- Atalho global no PC para criar tarefa rápida: depende da decisão de empacotar como app desktop (exige app em segundo plano, ícone na bandeja)

---

## 5. Campos dos itens

### Tarefa

| Campo | Observação |
|---|---|
| **Id** | Identificador único (GUID) |
| **Título** | |
| **Notas** | Texto e links (campo único, sem descrição separada) |
| **Prazo** | Data e hora; 23:59 como padrão (equivale a "dia inteiro") |
| **Prioridade** | |
| **Etiquetas** | Várias por tarefa |
| **Coluna** | Em qual coluna do quadro está |
| **Posição** | Ordem do cartão dentro da coluna (para o drag and drop) |
| **Checklist** | Itens com texto, marcado ou não, e ordem |
| **Registros de tempo** | Data, duração e origem (Pomodoro ou manual) |
| **Meta vinculada** | Opcional, com o marco, se houver |
| **Recorrência** | Referência à série e data da ocorrência, quando recorrente |
| **Data de conclusão** | Vazia enquanto aberta; preenchida ao entrar na coluna de conclusão |
| **Arquivada** | Sim ou não, com o motivo: concluída ou não realizada |
| **Lembretes** | Até 3 por item (em análise) |
| **Criada em / atualizada em** | Horário do dispositivo; decide o "mais recente" nos conflitos |
| **Apagada** | Marca de exclusão |

### Compromisso

- Id, título e notas (texto e links)
- Início e fim (data e hora)
- Local
- Opção de **dia inteiro**
- Lembretes (até 3, em análise)
- Criado em, atualizado em e apagado

### Meta (campos comuns)

- Id, título e notas (texto e links)
- Tipo: com prazo, numérica ou hábito
- Status: ativa, concluída ou arquivada
- Data de conclusão
- Criada em, atualizada em e apagada

### Meta com prazo

- Prazo (data final)
- **Marcos:** cada um com título, data opcional e marcado ou não
- As tarefas vinculadas ficam do lado da tarefa (campo "meta vinculada")

### Meta numérica

- Valor inicial, valor atual, alvo e unidade (livros, km, reais)
- Prazo opcional
- **Histórico de atualizações:** cada registro com data e valor (alimenta o histórico de metas e o gráfico de evolução)

### Hábito

- Frequência: todo dia, X vezes por semana ou dias específicos da semana
- Data de início
- Lembretes (até 3, em análise)
- **Check-ins:** um registro por dia, com a data e se foi feito
- A sequência atual e a maior sequência **não são guardadas**: são calculadas a partir dos check-ins e da regra de tolerância de 1 dia