# Fluxo de verificação de idiomas

## Entendimento

O site abre no idioma nativo do navegador do usuário e por esse motivo há situações onde o mesmo acaba por enviar mensagens num determinado idioma sem ler as instruções, resultando numa situações onde o usuário se frusta com mensagens que não entende nem sabe como proceder a fim de alcançar uma interação produtiva.  
Nesse caso se faz necessário detectar o idioma pela própria mensagem.  
Havia um método do Google capaz de realizar traduções (semelhante à seu tradutor), mas sua integração estava fora de meu nível de conhecimento.  
A idéia era usar tal API a fim de detectar o idioma automaticamente, assim como existe tal opção na ferramenta de tradução, usar isso como checagem dupla antes de responder ao contato e, mesmo nos casos onde a mensagem não seja entendida pelo bot, ser capaz de explicar o cenário em seu idioma. Assim melhorando a experiência e guiando para uma interação positiva.  
Com a ajuda do time de plataforma foi elaborada uma chamada que tornou isso possível em nível superficial a fim de demonstrar e explicar ao cliente. Havendo interesse o método precisará ser aperfeiçoado e testado em mais situações de excessão.

### Requisitos da API

1. Conta Google Cloud com configurações (usando a da Ilha no momento);

### Requisitos do projeto

1. Resposta no mesmo idioma do contato, salvando para futuras inteções;
2. Certeza na detecção do idioma;
3. Fácil manutenção;
4. Adição do método não interfira no entendimento atual da estrutura de fluxos do projeto;

### Limitações da API

1. É paga;
2. Codificação ISO usada para parametrização do idioma;

### Limitações do projeto

1. Navegação restrita entre fluxos "pai" e "filho";

## Fluxo e fluidez

O conjunto de ações é chamado no fluxo **Classifier** e já na primeira ação do fluxo **verify language** é chamado o webhook que realiza a pesquisa. O split em seguida verifica o índice `data.detections[0][0].confidence` a fim de aceitar apenas os casos onde há 100% de certeza no idioma detectado.  
Havendo certeza o split *bothub_lang* filtra o idioma e salva a categoria de acordo com o padrão ISO usado no Bothub. Se nenhum idioma esperado por encontrado, o split encerra no *Other* sem realizar alterações.  
A série opções que seguem são updates com o respectivo idioma do contato no RapidPro, finalizado com o update no campo *bothub_lang* que é usado nas consultas à inteligencia.

A fim de evitar loopings, foi necessário alterar a posição da chamada à inteligência do escopo para um novo flux chamado *Classifier call*, que é chamado já com os novos parâmetros de idioma do contato e salva o resultado em *Flow results* caso a confidência encontrada seja superior em relação à da chamada anterior (quando o idioma seria o incorreto).  
Uma vez finalizado, o fluxo retorna à ação anterior (fluxo pai) dando continuidade em sua normal estrutura.  

Destaca-se o seguinte esclarescimento, essa estrutura é chamada apenas nos casos onde a confidência da primeira consulta à inteligência do escopo seja inferior a 45%. Aliado ao custo individual de cada mensagem consultada, a quantidade de requisições à endpoints de terceiros e possibilidade de falhas (time-out) foi o maior ponto de preocupação nessa sugestão.

# Fluxo bias

## Entendimento

Há a necessidade de tratar mensagens fora do escopo atual de maneira eficiente, sem prejudicar a experiência do usuário e trazendo de volta àquilo que o bot se propõe. Estudando um pouco a API de buscas do google, desenvolvi um fluxo que permite isso usando um webhook que devolve o primeiro resultado da busca (semelhante à fazer a busca pelo site do google).

Alcaçado o entendimento, desenvolvi um método para ter resultado esperado em nível superficial a fim de demonstrar e explicar ao cliente. Havendo interesse o método precisará ser aperfeiçoado e testado em mais situações de excessão.

### Requisitos da API

1. Mensagem não pode possuir espaços;
2. Query string com todos os parâmetros diferene de nulo;

### Requisitos do projeto

1. Resposta no mesmo idioma do contato;
2. Distinção entre respostas à coisas objetivas vs. abstratas;
3. Informar que se trata de informação externa;
4. Exibir feedback com quick-replies quando conteúdo satisfatório não for encontrado;

### Limitações da API

1. ID de idiomas;
2. Caracteres especiais;

### Limitações do projeto

1. Separação entre texto e URL;

## Fluxo e fluidez

Dentro do fluxo **Out of scope issues**, que é chamado após esgotadas todas as possibilidades de interpretação dentro do escopo, há um *Result* para substituir os espaços existentes na mensagem por **+** e logo em seguida é chado o webhook à API do Google.  
Nesse webhook os parâmetros são:  
1. query: Mensagem à ser pesquisada (resolvido de @results.message);
2. key: Chave do app que criei para essa chamada;
3. limit: Limite de respostas (configurado como 1);
4. indent: Para que o JSON retorne identado (configurado como True);
5. languages: Para que o resultado retorne de acordo com o idioma do contato (@fields.bothub_lang por usar padrão ISO 639);

O resultado retorna uma lista e verificando se há conteúdo em **itemListElement** o split que segue avança para a primeira etapa da resposta ou finaliza informando que não há resposta e existe as quick-replies.  
Havendo resposta, a primeira mensagem que o contato receberá é a informação que se trata de texto de origem externa e sua respectiva URL de acesso. Em seguida há um filtro para as situações onde a resposta se trata de "objeto" e outra para "serviço".  
Após ambas também é enviado o set de quick-replies a fim de trazer o contato ao conteúdo esperado pelo projeto.
