# Introdução

Os chatbots conversacionais desenvolvidos com inteligência artificial só terão suas primeiras conversas depois de treinado, implantado e pronto, isso se torna um grande potencial desperdiçado. Desta forma este texto busca propor uma implantação de um chatbot autoalimentado em nosso projeto, assim ele seria capaz de obter uma nova base de dados a partir de cada diálogo em que participa.

Essas conversas são abundantes, específicas para tarefas, dinâmicas e de baixo custo. Isso reflete a maneira como os humanos aprendem a conversar: não apenas observando outros envolvidos em conversas "de nível especialista", mas também ajustando e corrigindo ativamente nosso discurso com base no feedback incorporado às nossas próprias interações (Bassiri, 2011; Werts et al., 1995). Construir um agente de diálogo com essa capacidade permitiria sua melhoria contínua e adaptação ao longo de sua vida útil, sem a necessidade de custos adicionais de anotação para cada melhoria.

Esta abordagem oferece uma solução promissora para o desafio da falta de atualização de modelos de chatbots em um mundo em constante mudança, destacando a importância do concept drift e da adaptabilidade contínua. O futuro da conversação artificial reside na capacidade de aprender e melhorar a partir das próprias interações, assim como fazem os seres humanos, e é isso que nossa abordagem visa alcançar.

# Solução Proposta

## Diagrama de Blocos

![Diagrama de Blocos](link_para_a_imagem.png)

Descrição dos Blocos

**Fase Inicial de Treinamento (Treinamento Inicial)**

Nesta fase, o agente de diálogo é treinado em duas tarefas principais: DIALOGUE (previsão da próxima fala) e SATISFACTION (avaliação da satisfação do parceiro de conversa). O treinamento é realizado utilizando dados supervisionados disponíveis, que são chamados de exemplos Human-Human (HH) e foram gerados em conversas entre dois seres humanos.

**Fase de Implantação (Implantação)**

Durante esta fase, o agente de diálogo participa de conversas de várias rodadas com os usuários e extrai novos exemplos de duas categorias. A cada turno, o agente observa o contexto (x), que é a história da conversa até aquele ponto, e usa isso para prever sua próxima fala (yˆ) e a satisfação estimada de seu parceiro (sˆ). Se o índice de satisfação estiver acima de um limite especificado (t), o agente extrai um novo exemplo de DIALOGUE chamado Human-Bot (HB) usando o contexto anterior (x) e a resposta do humano (y), continuando a conversa.

**Solicitação de Feedback (Feedback Request)**

Se o usuário parecer insatisfeito com a resposta anterior do agente (ˆs < t), o agente solicita feedback por meio de uma pergunta (q). A resposta de feedback (f) é usada para criar um novo exemplo para a tarefa FEEDBACK (previsão do feedback). O agente confirma o recebimento do feedback e a conversa continua.

**Taxa de Coleta de Exemplos (Ajuste da Taxa)**

A taxa na qual novos exemplos de DIALOGUE e FEEDBACK são coletados pode ser ajustada, variando o valor do limite de satisfação (t). Isso permite controlar o equilíbrio entre aprimoramento contínuo e conservação de recursos.

**Retreinamento Periódico (Retreinamento)**

Periodicamente, o agente é submetido a uma fase de retreinamento, na qual todos os dados disponíveis (incluindo HH, HB e exemplos de FEEDBACK) são usados para melhorar o desempenho na tarefa principal de DIALOGUE.

**Uso de Feedback Natural (Feedback Natural)**

É importante destacar que as respostas do usuário são sempre em formato de diálogo natural. Em nenhum momento, os novos exemplos de FEEDBACK são inspecionados, pós-processados ou limpos. A abordagem confia na relação aprendível entre os contextos da conversa e o feedback correspondente, aproveitando

Essa abordagem permite que o sistema conversacional aprenda e se adapte de forma contínua à medida que interage com os usuários, incorporando feedback real e ajustando-se a novos padrões e conceitos. O sistema se torna mais eficaz e relevante ao longo do tempo, refletindo as mudanças nas expectativas e necessidades dos usuários.

A proposta visa criar um ciclo de aprendizado contínuo que se assemelha ao processo de aprendizado humano, em que a interação constante com o ambiente leva a melhorias constantes no desempenho e na eficácia do sistema conversacional.

### Conclusão

A proposta de fomentar o aprendizado contínuo em um sistema conversacional, baseado no paradigma apresentado, oferece uma abordagem promissora para manter a relevância e a eficácia desse sistema em um ambiente dinâmico. O ciclo de vida do "chatbot de autoalimentação" apresentado no diagrama de blocos permite que o agente de diálogo aprenda e se adapte constantemente às necessidades dos usuários, melhorando seu desempenho ao longo do tempo.

Considerando essa proposta, é importante ressaltar algumas considerações:

**Aprimoramento Contínuo:** A abordagem proposta capacita o sistema a melhorar sua capacidade de diálogo e satisfação do usuário continuamente. Ao extrair exemplos reais de conversas e feedback dos usuários, o sistema pode se ajustar às mudanças nos padrões de interação e nas expectativas dos usuários.

**Feedback Natural:** A abordagem se baseia em feedback natural, não exigindo intervenções humanas para inspecionar ou pós-processar os dados. Isso torna a abordagem mais eficiente e alinhada com conversas reais.

**Ajuste da Taxa de Coleta:** A capacidade de ajustar a taxa de coleta de exemplos permite equilibrar a necessidade de aprendizado contínuo com os recursos disponíveis, tornando a abordagem flexível e adaptável.

É importante reconhecer que a implementação dessa abordagem requer um esforço considerável:

**Coleta de Dados:** É necessário coletar e armazenar grandes volumes de dados de conversas, incluindo exemplos de interações bem-sucedidas e feedback do usuário. Nesta etapa tecnologias de bancos de dados como MongoDB e sistemas de armazenamento em nuvem como AWS S3 podem ser utilizadas.

**Treinamento de Modelos:** O treinamento de modelos de aprendizado de máquina para prever as próximas respostas e a satisfação do usuário exige recursos computacionais significativos. Plataformas como TensorFlow e PyTorch oferecem as ferramentas necessárias para treinar modelos de linguagem e modelos de satisfação do usuário.

**Ajuste de Parâmetros:** A definição de parâmetros, como o limite de satisfação, requer análise e ajuste cuidadosos para equilibrar o aprendizado contínuo com a qualidade das interações.

**Retreinamento Periódico:** A fase de retreinamento periódico exige uma infraestrutura robusta para atualizar os modelos com dados acumulados.A automação do processo de retreinamento é essencial. É possível usar ferramentas de automação de fluxo de trabalho como Apache Airflow para agendar tarefas de retreinamento.

**Monitoramento Contínuo:** É fundamental estabelecer sistemas de monitoramento contínuo para avaliar o desempenho do sistema e identificar problemas ou deriva de conceito.Ferramentas como ELK Stack (Elasticsearch, Logstash e Kibana) podem ser usadas para monitoramento de logs.

### Bibliografia 

HANCOCK, Braden; BORDES, Antoine; MAZARE, Pierre-Emmanuel; WESTON, Jason. Learning from dialogue after deployment: Feed yourself, chatbot!. Pré-impressão arXiv, 2019. Disponível em: <https://arxiv.org/abs/1901.05415>. Acesso em: [03/10/2023].