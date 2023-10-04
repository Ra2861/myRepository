# Introdução

Os chatbots conversacionais desenvolvidos com inteligência artificial só terão suas primeiras conversas depois de treinado, implantado e pronto, isso se torna um grande potencial desperdiçado. Desta forma este texto busca propor uma implantação de um chatbot autoalimentado em nosso projeto, assim ele seria capaz de obter uma nova base de dados a partir de cada diálogo em que participa.

Essas conversas são abundantes, específicas para tarefas, dinâmicas e de baixo custo. Isso reflete a maneira como os humanos aprendem a conversar: não apenas observando outros envolvidos em conversas "de nível especialista", mas também ajustando e corrigindo ativamente nosso discurso com base no feedback incorporado às nossas próprias interações (Bassiri, 2011; Werts et al., 1995). Construir um agente de diálogo com essa capacidade permitiria sua melhoria contínua e adaptação ao longo de sua vida útil, sem a necessidade de custos adicionais de anotação para cada melhoria.

Esta abordagem oferece uma solução promissora para o desafio da falta de atualização de modelos de chatbots em um mundo em constante mudança, destacando a importância do concept drift e da adaptabilidade contínua. O futuro da conversação artificial reside na capacidade de aprender e melhorar a partir das próprias interações, assim como fazem os seres humanos, e é isso que nossa abordagem visa alcançar.

# Solução Proposta

## Diagrama de Blocos

![Diagrama de Blocos](link_para_a_imagem.png)

## Descrição dos Blocos

### Fase Inicial de Treinamento (Treinamento Inicial)

Nesta fase, o agente de diálogo é treinado em duas tarefas principais: DIALOGUE (previsão da próxima fala) e SATISFACTION (avaliação da satisfação do parceiro de conversa). O treinamento é realizado utilizando dados supervisionados disponíveis, que são chamados de exemplos Human-Human (HH) e foram gerados em conversas entre dois seres humanos.

### Fase de Implantação (Implantação)

Durante esta fase, o agente de diálogo participa de conversas de várias rodadas com os usuários e extrai novos exemplos de duas categorias. A cada turno, o agente observa o contexto (x), que é a história da conversa até aquele ponto, e usa isso para prever sua próxima fala (yˆ) e a satisfação estimada de seu parceiro (sˆ). Se o índice de satisfação estiver acima de um limite especificado (t), o agente extrai um novo exemplo de DIALOGUE chamado Human-Bot (HB) usando o contexto anterior (x) e a resposta do humano (y), continuando a conversa.

### Solicitação de Feedback (Feedback Request)

Se o usuário parecer insatisfeito com a resposta anterior do agente (ˆs < t), o agente solicita feedback por meio de uma pergunta (q). A resposta de feedback (f) é usada para criar um novo exemplo para a tarefa FEEDBACK (previsão do feedback). O agente confirma o recebimento do feedback e a conversa continua.

### Taxa de Coleta de Exemplos (Ajuste da Taxa)

A taxa na qual novos exemplos de DIALOGUE e FEEDBACK são coletados pode ser ajustada, variando o valor do limite de satisfação (t). Isso permite controlar o equilíbrio entre aprimoramento contínuo e conservação de recursos.

### Retreinamento Periódico (Retreinamento)

Periodicamente, o agente é submetido a uma fase de retreinamento, na qual todos os dados disponíveis (incluindo HH, HB e exemplos de FEEDBACK) são usados para melhorar o desempenho na tarefa principal de DIALOGUE.

### Uso de Feedback Natural (Feedback Natural)

É importante destacar que as respostas do usuário são sempre em formato de diálogo natural. Em nenhum momento, os novos exemplos de FEEDBACK são inspecionados, pós-processados ou limpos. A abordagem confia na relação aprendível entre os contextos da conversa e o feedback correspondente, aproveitando
