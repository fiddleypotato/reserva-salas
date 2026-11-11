## 1. Visão do Sistema

**Problema e Objetivo:** 
A necessidade de gerir de forma eficiente e flexível a partilha de espaços de trabalho. O objetivo central deste projeto é criar uma plataforma que permita a reserva fácil de espaços, como escritórios privados e salas de reunião flexíveis, garantindo simultaneamente que a gestão do edifício mantém o controlo sobre a ocupação e a resolução de conflitos.

**Utilizadores Principais:**
*   **Clientes:** Utilizadores que efetuam reservas de espaços, os quais estarão divididos em dois níveis de subscrição (tiers) distintos, determinando as suas permissões e acessos.
*   **Administradores:** Utilizadores com uma conta de gestão com permissões elevadas, incluindo a capacidade e autoridade para cancelar as reservas efetuadas pelos clientes (por motivos de gestão, manutenção ou conflito).

**Fora do Âmbito:**
Nesta fase, ficam fora do âmbito do sistema a integração direta com hardware de controlo de acessos físico (como fechaduras inteligentes ou catracas eletrónicas), a gestão avançada de faturação e contabilidade corporativa (além da atribuição do tier de subscrição), e o agendamento de serviços de limpeza ou manutenção dos espaços.

## 2. Análise de Stakeholders e dos seus Interesses

| Stakeholder | Descrição | Principais Interesses e Expectativas | 
| ----- | ----- | ----- | 
| **Clientes / Subscritores** | Utilizadores que alugam os espaços (freelancers, empresas). | Processo de reserva intuitivo, disponibilidade garantida, clareza sobre os benefícios do seu tier de subscrição, e notificações atempadas caso um administrador cancele a sua reserva. | 
| **Administradores do Espaço** | Equipa de gestão que opera o edifício e o sistema. | Controlo total sobre o calendário, capacidade de resolver conflitos rapidamente (cancelamento de reservas), e facilidade na gestão dos níveis de subscrição. | 
| **Proprietários / Investidores** | Entidades que financiam ou detêm os espaços físicos. | Maximização da taxa de ocupação, relatórios detalhados de faturação por tier, e garantia de segurança e manutenção do espaço. | 
| **Equipa de TI / Suporte** | Responsáveis pelo desenvolvimento e manutenção da plataforma. | Estabilidade do sistema, arquitetura escalável para gerir picos de tráfego, e facilidade em auditar os cancelamentos feitos pelos administradores. | 

## 3. Registo de Riscos

| ID | Risco | Probabilidade | Impacto | Mitigação | 
| ----- | ----- | ----- | ----- | ----- | 
| **R01** | **Conflito de reservas (Overbooking):** Múltiplos utilizadores tentam reservar o mesmo espaço em simultâneo. | Média | Alto | Implementar bloqueios transacionais (*pessimistic locking*) na base de dados e testes de stress durante o desenvolvimento. | 
| **R02** | **Insatisfação por cancelamento administrativo:** Clientes perdem a reserva de forma abrupta por ação do administrador. | Média | Médio | Criar políticas claras nos Termos e Condições e desenvolver um sistema de notificações automáticas que inclua a justificação do cancelamento e sugestão de espaços alternativos. | 
| **R03** | **Alucinação do assistente de IA (Risco IA):** O chatbot de suporte alimentado por IA fornece informações erradas sobre regras de subscrição ou confirma reservas que não foram registadas. | Baixa | Alto | Implementar *guardrails* rígidos no modelo de IA para que este atue apenas em modo de leitura (consulta de disponibilidade) e direcione a intenção de reserva para o fluxo standard (sem intervenção de IA para a transação). | 
| **R04** | **Viés na recomendação de espaços (Risco IA):** O algoritmo de IA que sugere salas aos utilizadores ignora as restrições do seu tier de subscrição, sugerindo espaços premium a utilizadores do tier base. | Média | Médio | Impor restrições baseadas em regras de negócio (*hard constraints*) pós-processamento, filtrando as sugestões da IA cruzando-as com a tabela de permissões de tiers antes de as apresentar ao cliente. | 

## 4. Modelo de Processo

**Modelo Escolhido: Modelo Iterativo**
A escolha de um modelo iterativo justifica-se pela capacidade de desenvolver e refinar o sistema através de ciclos repetidos. Em vez de construir todo o sistema de uma vez, o projeto será dividido em iterações. Por exemplo, a primeira iteração pode focar-se apenas na infraestrutura básica de reservas e nos dois níveis de subscrição. As iterações seguintes podem adicionar os poderes de cancelamento do administrador e, por fim, as funcionalidades de inteligência artificial. Isto permite testar o modelo central de negócio (os tiers) muito mais cedo.

**Comparação com a Alternativa (Modelo Cascata / Waterfall):**
Num modelo em Cascata (Waterfall), o progresso flui numa única direção e todas as especificações — como as regras precisas das subscrições e os limites da IA — teriam de estar fechadas antes de a programação começar. Se os utilizadores não aderissem ao formato inicial dos tiers, o modelo em cascata tornaria a adaptação cara e demorada. O modelo iterativo mitiga este risco, pois permite que o feedback dos clientes ao final de cada iteração seja incorporado e refine a direção da iteração seguinte.

## 5. Product Backlog Inicial

As histórias de utilizador (User Stories) abaixo estão priorizadas para focar primeiro nas funcionalidades essenciais de reserva e gestão antes de avançar para otimizações com IA.

| Prioridade | Épico | User Story | 
| ----- | ----- | ----- | 
| **1 (Alta)** | Gestão de Subscrições | **Como** cliente, **quero** selecionar um dos dois níveis de subscrição disponíveis no registo, **para que** possa ter acesso às condições e preços que melhor se adequam ao meu uso. | 
| **2 (Alta)** | Reservas | **Como** cliente, **quero** visualizar o calendário de disponibilidade de escritórios e salas, **para que** consiga escolher um horário livre. | 
| **3 (Alta)** | Reservas | **Como** cliente, **quero** reservar um espaço de acordo com os limites do meu nível de subscrição, **para que** garanta o meu local de trabalho. | 
| **4 (Alta)** | Administração | **Como** administrador, **quero** poder cancelar qualquer reserva no sistema, **para que** consiga libertar salas por motivos de manutenção, incumprimento ou conflito. | 
| **5 (Alta)** | Administração | **Como** administrador, **quero** poder visualizar as listas de todos os clientes e os seus respetivos níveis de subscrição, **para que** possa gerir os acessos ao edifício. | 
| **6 (Média)** | Notificações | **Como** cliente, **quero** receber um email automático sempre que um administrador cancelar a minha reserva, **para que** seja informado atempadamente e possa reagendar. | 
| **7 (Média)** | Funcionalidades de IA | **Como** cliente, **quero** que o sistema de IA me recomende espaços frequentemente usados por mim no meu horário habitual, **para que** o processo de reserva seja mais rápido. | 
| **8 (Baixa)** | Funcionalidades de IA | **Como** administrador, **quero** aceder a um *dashboard* preditivo gerado por IA, **para que** consiga antecipar os dias de maior sobrelotação do espaço. |

testeDC