# Atualização de nomes de fluxo

Agora os fluxos contêm um prefixo que determina seu propósito. A motivação dessa atualização foi facilitar administração de traduções entre ambientes pois as labels não geram filtros na tela de exportação e a busca por fluxos precisava ser manual.

## Fluxos e organização

A principal motivação desse método foi permitir a fácil manutenção de traduções, onde é possível destacar que o cliente somente precisará editar fluxos marcados como **Response messages** e **Sorting input**.

1. Label **Core engine**, prefixo **C**
  Consiste nos fluxos núcleo, onde o contato é preparado e sua mensagem percorre a estratégia de interpretação.
2. Label **Messages engine**, prefixo **M**
  Fluxos de processamento, onde a mensagem tem sua resposta endereçada.
3. Label **Response messages**, prefixo **R**
  Conteúdo de respostas. Todos *Send Messages* com blocos de texto que o contato receberá estão aqui. Em alguns casos esses podem possuir *Wait for response* a fim de filtrar superficialmente a mensagem do contato.
4. Label **Sorting input**
  Organiza filtros que precisam de manutenção nas traduções em suas regras.

> Como a tela de exportação de fluxos no RP não leva em consideração essa marcação de labels, o prefixo de fluxos foi uma atualização para permitir a fácil seleção de fluxos por função.

### Lista de fluxos **Core engine**
* **Classifier** -> Métodos de classificação da mensagem;
* **Classifier call** -> Chamada webhook para a inteligência do escopo;
* **Interaction counter** -> Contabiliza as interações por canal numa constante global;
* **Issues** -> Classificação da inteção detectada ou ausência dela;
* **Label - Low confidence by language** -> Marca a label *Low confidence* na mensagem respeitando o idioma do contato;
* **Label - New question by language** -> Marca a label *New question* na mensagem respeitando o idioma do contato;
* **Label - Reported rumors by language** -> Marca a label *Reported rumors* na mensagem respeitando o idioma do contato;
* **New message** -> Verificação dupla para tratar novas mensagens, de acordo com canais, que não entrem por triggers e assegurar a preparação adequada do contato;
* **Telegram contact setup** -> Prepara o contato de acordo com as particularidades do canal Telegram;
* **Update data** -> Atualiza informações usando recursos da API John Hopkins;
* **Verify language** -> Verificação dupla para os casos onde o chat receba mensagens em um idioma diferente daquele que está no contato e ações a fim de atualizar com o idioma correto.
* **VK contact setup** -> Prepara o contato de acordo com as particularidades do canal VK;
* **WEB contact setup** -> Prepara o contato de acordo com as particularidades do canal WEB;

### Lista de fluxos **Messages engine**

* **Direct addressed responses** -> Responsável por endereçar casos onde a mensagem possui todos os fragmentos necessários para chegar a determidado feed-back, seja à resposta ou ao tratamento.
* Todos os demais fluxos tem a mesma característica de endereçamento à respectiva intenção.

| | | |
---|---|---
**Comparison** | **Concepts** | **Duration**
**Immunity** | **Mask** | **Mask type**
**Myth weather** | **Myths** | **Pandemic**
**Pregnancy** | **Prevention** | **Recommendation**
**Risk** | **Symptoms** | **Transmission**
**Treatment** |

### Lista de fluxos **Response messages**

| | | | | |
|---|---|---|---|---|
Air transmission | Antibiotics | Asymptomatic | Bad words | Breast feeding
 | Caring | Cases | Continue medicines | Countries dashboard | COVID19 immunity
 | Dangerous | Disease course | Disinfect | Distancing help | Farewell
 | Finishing | Flu symptoms | Greetings | Grocery | Hand wash
 | HIV risk | Home meeting | Hotlines | Ibuprofen | influenza
 | isolation | Isolation how long | Issues alert | Lessons | Maintenance
 | Mask advice | Mask homemade | Mask medical | Mask non-medical | Mask type alert
 | Mask use | Mortality | Mother to child | Mutation | Myths - Antibiotics
 | Myths - bank notes | Myths - cold weather | Myths - garlic | Myths - hand dryers | Myths - herd immunity
 | Myths - hot bath | Myths - hot weather | Myths - Medicines | Myths - mosquitoes | Myths - nose saline
 | Myths - Older | Myths - Pets | Myths - Treatment | Myths - uv lamp | Myths list
 | Older people | Origin | Out of scope issues | Outbreak peak | Outdoors
 | Pandemic ends | Patients | physical distancing | Pregnancy care | Pregnancy testing
 | Pregnant | Prevent spread | Protection | Public workers | Quarantine
 | Recovery | Reinfection | Report rumors | Risk - children | Risk - high risk group
 | Risk alert | Rumors | Rumors - with code in the chat | SARS | School
 | Severity | Should wear mask | Smoking | social distancing | Spread
 | Surfaces | Symptoms alert | Temperature | Testing | Thank messages
 | Topics by option | Travel airport | Treatment alert | Vaccine | Violence
 | Virus status | What is corona | What is COVID19 | Worries

### Lista de fluxos Sorting input

* Auxiliary verbs and actions
* Chronic conditions

# Fluxos alterados

## Issues

* Remoção da medida paliativa para frases com *raped rape raping* acusando falso positivo em **Outbreak**.
* Adição dos filtros de entidades para **Violence** e **School**.
* Remoção dos filtros referentes à mensagens de resposta à baixa confidência.
* Adição de link ao fluxo **Issues alert** em resposta à baixa confidência.

## Classifier

* Atualização do método de chamada à respostas com fragmentos diretamente da mensagem dentro do fluxo **Direct addressed responses** (todas as ações estão condensadas nesse fluxo).
* Substitução do webhook de chamada à inteligência do escopo pow um um fluxo dedicado à essa ação, **Classifier call**.
* Adição do método de dupla verificação do idioma da mensagem.
* Atualização do filtro *bias* para direcionamento à **Out of scope issues**.
* Remoção do filtro para mensagens com *no not nops never* (Monitorar comportamento da inteligência *Binary*).

## Mask

* Adição de link à **Auxiliary verbs and actions** para verificação de frases com *how* e *should*.

## Risk

* Remoção dos *Send messages*.
* Adição de link ao fluxo **Risk alert** com o conteúdo dos *Send messages* removidos.

## Recommendation

* Correção do filtro para *noentity* (para fluxos sem resposta padrão quando entidades não forem detectadas.)

## Transmission

* Remoção do filtro para a entidade *temperature*.
* Remoção do filtro para mensagens com a palavra *banknotes*.

## Prevention

* Correção do filtro para *noentity* (para fluxos sem resposta padrão quando entidades não forem detectadas.)

## Duration

* Remoção do filtro para mensagens com as palavras *recover* ou *infected*.
* Correção do filtro para *noentity* (para fluxos sem resposta padrão quando entidades não forem detectadas.)

## Concepts

* Correção do filtro para *noentity* (para fluxos sem resposta padrão quando entidades não forem detectadas.)

## Comparison

* Remoção de dupla presença de entidades para endereçar respostas.
* Correção do filtro para *noentity* (para fluxos sem resposta padrão quando entidades não forem detectadas.)

## Treatment

* Remoção dos *Send messages*.
* Adição de link ao fluxo **Treatment alert** com o conteúdo dos *Send messages* removidos.

## Immunity

* Correção do filtro para *noentity* (para fluxos sem resposta padrão quando entidades não forem detectadas.)

## Symptoms

* Remoção do filtro para mensagens com as palavras *Hypertension Obesity Diabetes Cardiovascular disease Asthma COPD*.
* Link de fluxo exclusivo para esse filtro, **Chronic conditions**.

## Topics by option

* Filtros referentes ao processamento de letras correspondentes às opções agora estão com link ao fluxo **Direct addressed responses**.

## Prevent spread

* Remoção do filtro, pois agora a resposta não depende mais de marcação de entidade.

# Alteração de label

## Passou a ser Messages engine

* Mask
* Mask type

## Passou a ser Response messages

* Topics by option
* Rumors
* Finishing
* Greetings
* Bad words
* Report rumors
* Out of scope issues
* Farewell
* Hotlines
* Maintenance

# Fluxos criados

## Classifier call

Com a chamada ao webhook com a inteligência do escopo. Atualização para evitar duplicidade de código.

## Risk alert

Contém apenas o *Send message* com o bloco de resposta.

## Treatment alert

Contém apenas o *Send message* com o bloco de resposta.

## Symptoms alert

Contém apenas o *Send message* com o bloco de resposta.

## Mask type alert

Contém apenas o *Send message* com o bloco de resposta.

## Issues alert

Contém apenas o *Send message* com o bloco de resposta.

## Auxiliary verbs and actions

Filtro para **how** e **should**, criado para administração de traduções.

## Chronic conditions

Filtro para doenças cronicas, criado para administração de traduções.

# Testes rodando

## Rumors with code in the chat

Alteração da verificação por token no chat ao invés de envio por email.

## Gênero e região

Questões em passos diferentes de fluxos para extrair gênero e região do usuário.

## Violence

Nova intenção.

## School

Nova intenção.

## Detecção de idiomas (API Google)

Descrição [aqui](https://github.com/WilliamTales/HealthBuddy/blob/master/evolucao.md#fluxo-de-verifica%C3%A7%C3%A3o-de-idiomas).

## Questões fora de escopo (API Google)

Descrição [aqui](https://github.com/WilliamTales/HealthBuddy/blob/master/evolucao.md#fluxo-de-verifica%C3%A7%C3%A3o-de-idiomas).
