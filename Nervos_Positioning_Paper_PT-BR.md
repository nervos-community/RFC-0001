---
Número: "0001"
Categoria: Informacional
Status: Rascunho
Author: O Time Nervos
Organização: Nervos Foundation
Criado: 2019-09-12
Traduzido por: Ivan Zucchi e Rodrigo Santiago
---

# Declaração de Posicionamento da Nervos Network

## 1.Propósito do Documento

A Nervos Network é feita de uma série de protocolos e inovações. É importante manter, de forma clara, documentações e especificações técnicas de designs e implementações - para os quais usamos um processo RFC (Request For Comment) (https://github.com/nervosnetwork/rfcs). Contudo, sentimos que é igualmente importante que ajudemos nossas comunidades a entender o que tentamos realizar, as decisões que tomamos, e como chegamos às soluções de design atuais.

Começamos este documento com uma análise detalhada dos problemas que blockchains públicas (permissionless) enfrentam nos dias de hoje e as soluções existentes que tentam resolvê-los. Esperamos que isso dê o contexto necessário para que nossos leitores entendam nosso pensamento de como melhor enfrentar esses desafios, e nossas decisões de design subjacentes. Então, provemos um passo a passo em alto nível de todas as partes da Nervos Network, com foco em como cada pedaço trabalha em conjunto para suportar a visão geral da rede.

## 2. Contexto

Escalabilidade, sustentabilidade e interoperabilidade estão entre os maiores desafios que blockchains permissionless públicas enfrentam nos dias de hoje. Enquanto muitos projetos alegam ter soluções para estes problemas, é importante entender de onde vieram os problemas e colocar soluções no contexto de possíveis trade-offs.

### 2.1 Escalabilidade

Bitcoin[1] foi a primeira blockchain permissionless pública, projetada para ser usada como dinheiro eletrônico peer-to-peer (de comunicação direta entre os utilizadores). Ethereum[2] tornou possível mais casos de uso e criou uma plataforma de computação descentralizada de propósito geral. No entanto, essas duas plataformas impõem limitações em suas capacidades de transação - Bitcoin limita seu tamanho de bloco e Ethereum limita seu limite de gás por bloco. Esses passos são necessários para garantir descentralização no longo prazo, contudo eles também limitam as capacidades das duas plataformas.

A comunidade blockchain propôs uma série de soluções de escalabilidade nos últimos anos. No geral, podemos dividir essas soluções em duas categorias: on-chain e off-chain.

Soluções on-chain tem como objetivo expandir a taxa de transferência do processo de consenso e criar blockchains com taxa de transferência nativa que rivalize com sistemas centralizados. Soluções off-chain usam apenas blockchain como um recurso seguro e uma plataforma de liquidação, enquanto move praticamente todas as transações para as camadas superiores. 

#### 2.1.1 Soluções de Escalamento On-chain com uma Única Blockchain

O caminho mais direto para aumentar a taxa de transferência de uma blockchain é aumentar seu suprimento de espaço de bloco. Com espaço de bloco adicional, mais transações podem fluir pela rede e serem processadas. Aumentar o suprimento de espaço de bloco em resposta à demanda aumentada de transações também permite que as taxas de transação se mantenham baixas.

Bitcoin Cash (BCH) adota esse modelo para escalar sua rede de pagamento peer-to-peer. O protocolo Bitcoin Cash iniciou com um tamanho máximo de bloco de 8 MB, que foi mais tarde aumentado para 32 MB, e que vai continuar a ser aumentado indefinidamente conforme as demandas por transações aumentam. Para referência, seguindo a implementação do Bitcoin (BTC) de Testemunha Segregada em Agosto de 2017, o protocolo Bitcoin agora permite um tamanho médio de bloco de cerca de 2 MB.

No escopo de um data center, a matemática funciona. Se 7.5 bilhões de pessoas criarem, cada uma, 2 transações on-chain por dia, a rede irá necessitar da produção de blocos de 26 GB a cada 10 minutos, levando a uma taxa de crescimento da blockchain de 3,75 TB por dia ou 1,37 PB por ano[3]. Esses requisitos de armazenamento e largura de banda são razoáveis para qualquer serviço de hospedagem nos dias de hoje.

Contudo, restringir operações de nó a um ambiente de datacenter leva a uma única topologia de rede viável e compromete a segurança (a taxa de bifurcação da blockchain irá aumentar conforme as necessidades por transmissão de dados pela rede aumentem), assim como a descentralização (a quantidade de nós será reduzida conforme o custo de participação no consenso aumenta).

De um ponto de vista econômico, um tamanho de bloco sempre crescente não alivia a pressão da taxa sentida pelos usuários. Uma análise da rede do Bitcoin mostrou que taxas permanecem estáveis até que um bloco esteja 80% preenchido, e entaõ crescem exponencialmente[4].

Embora colocar o fardo dos custos de uma rede em crescimento em seus operadores pareça ser uma decisão razoável, pode ser uma visão pouco ambiciosa por duas razões:

- A supressão de taxas de transação força mineradores a confiar predominantemente na compensação por nova emissão de moedas (recompensas de bloco). A não ser que inflação seja uma parte permanente do protocolo, nova emissão de moedas irá eventualmente parar (quando o limite máximo de moedas é atingido), e mineradores não irão receber nem as recompensas de bloco nem taxas de transação relevantes. O impacto econômico disso irá comprometer severamente o modelo de segurança da rede.
- O custo de executar um nó completo torna-se proibitivamente caro. Isso remove a habilidade de usuários regulares de independentemente verificarem o histórico e transações da blockchain, depositando a confiança em provedores de serviço como corretoras e processadoras de pagamento para que garantam a integridade da blockchain. Essa necessidade de confiança nega a proposição central de valor de blockchains permissionless públicas como sistemas distribuídos peer-to-peer e trustless (independe de confiança em terceiros). 

Plataformas com custo de transação otimizado como a Bitcoin Cash enfrentam uma competição significante de outras blockchains (tanto permissioned quanto permissionless), assim como de sistemas de pagamento tradicionais. Decisões de design que melhorem a segurança ou a resistência a censura incorrerão em custos associados e portanto aumentarão o custo de usar a plataforma. Levando em conta um panorama competitivo, assim como os objetivos declarados da rede, é mais provável que custos baixos serão os objetivos principais da rede, ao custo de quaisquer outras considerações.

Esse objetivo é consistente com nossas observações de uso de redes transacionais. Usuários desses sistemas não se importam com trade-offs de longo prazo pois eles irão utilizar a rede apenas por um curto período. Uma vez que seus bens ou serviços tenham sido recebidos e seu pagamento tenha sido finalizado, esses usuários não se preocuparão mais com a efetividade de operação da rede. A aceitação desses trade-offs é aparente no difundido uso de corretoras de cripto-ativos, bem como blockchains mais centralizadas. Esses sistemas são populares primariamente por sua conveniência e eficiência transacional.

Algumas plataformas de contratos inteligentes adotaram abordagens semelhantes para escalar a taxa de transferência de uma blockchain, permitindo apenas um conjunto limitado de "super computadores" a participarem do processo de validação de consenso e validarem a blockchain independentemente.

Embora comprometimentos em relação à descentralização e segurança da rede permitem transações mais baratas e podem ser mais convenientes para um conjunto de usuários, o modelo de segurança comprometido no longo-prazo, a barreira proibitiva de custo para verificar transações independentemente, e a provável concentração e entrincheiramento de operadores de nós nos leva a acreditar que essa não é a melhor maneira de escalar blockchains públicas.

#### 2.1.2 Soluções de Escalamento On-chain por meio de Múltiplas Cadeias

O escalamento por meio de múltiplas cadeias pode ser realizado via fragmentação, como visto no Ethereum 2.0, ou cadeias de aplicação, como visto no Polkadot. Esses designs particionam de maneira eficaz o estado global e as transações da rede em múltiplas cadeias, permitindo cada cadeia a rapidamente atingir consenso local, e mais tarde a totalidade da rede a atingir consenso global, por meio do consenso do "Beacon Chain" ou do "Relay Chain".

Esses designs permitem que múltiplas cadeias utilizem um modelo de segurança compartilhado, ao mesmo tempo em que permitem alta taxa de transferência e transações rápidas dentro dos fragmentos (Ethereum) ou para-chains (Polkadot). Embora cada um desses sistemas seja uma network de blockchains interconectadas, eles se diferem em relação aos protocolos sendo executados em cada cadeia. No Ethereum 2.0, todo fragmento executa o mesmo protocolo, enquanto no Polkadot, cada para-chain pode executar um protocolo customizado, criado por meio do framework Substrate.

Nessas arquiteturas multi-chain, cada dApp (ou instância de dApp) reside apenas numa única cadeia. Embora nos dias de hoje os desenvolvedores estejam acostumados à habilidade de construir dApps que perfeitamente interagem com qualquer outro dApp na blockchain, padrões de design precisarão se adaptar a novas arquiteturas multi-chain. Se um dApp é dividido em diferentes fragmentos, mecanismos serão necessários para manter o estado sincronizado entre as diferentes instâncias do dApp (residindo em diferentes fragmentos). Adicionalmente, embora os mecanismos de camada 2 possam ser implantados para uma rápida comunicação entre fragmentos, transações entre framentos necessitarão do consenso global e introduzirão latência de confirmação.

Com essas transações assíncronas, o problema infame do "trem-e-hotel" surge. Quando duas transações precisam ser atômicas (reservar uma passagem de trem e um quarto de hotel em dois fragmentos diferentes, por exemplo), novas soluções são necessárias. Ethereum introduz o "yanking" de contrato, no qual um contrato dependente é deletado em um fragmento, criado em um segundo fragmento (que contém outro contrato dependente), e as duas transações são então executadas no segundo fragmento. Contudo, o contrato "puxado" (yanked) estaria então indisponível no fragmento original, introduzindo problemas de usabilidade, e novamente requisitando novos padrões de design.

A fragmentação tem suas próprias vantagens e desafios. Se fragmentos podem ser verdadeiramente independentes e as necessidades de transação entre fragmentos são mínimas, uma blockchain pode escalar linearmente sua taxa de transferência aumentando o número de fragmentos. Esse cenário é mais adequado para as aplicações auto-contidas que não necessitam de estado exterior ou colaboração com outras aplicações.

Uma arquitetura fragmentada pode ser problemática para aplicações que são desenvolvidas juntando outras aplicações pilares (isso é conhecido como o "problema de composibilidade"). Composibilidade é especialmente relevante no espaço de finanças descentralizadas (DeFi), onde produtos mais avançados tendem a ser construídos sobre outros produtos pilares.

Em uma nota mais técnica, fragmentação tipicamente requer uma topologia "1 + N", na qual N cadeias se conectam a uma meta-chain, introduzindo um limite superior no número de fragmentos dos quais uma meta-chain pode suportar sem cair em problemas de escalabilidade por si própria.

Observamos um valor significante em um estado global unificado, permitindo que um ecossistema de aplicações interdependentes surja e que desenvolvedores possam inovar nas pontas, de forma similar a qual os desenvolvedores web se utilizam de bibliotecas para operações de baixo nível e APIs abertas para integração de serviços. Uma experiência muito mais simples é habilitada quando desenvolvedores não precisam considerar sincronicidade (em transações de bens ou mensagens entre fragmentos), bem como uma experiência de usuário superior, resultando em consistência nas preocupações arquiteturais de interações de blockchain.  

Reconhecemos que fragmentação é uma solução de escalabilidade promissora (em particular para aplicações com menos interdependência), contudo, acreditamos que é benéfico manter um design que concentra o estado mais valioso em uma única blockchain, permitindo composibilidade. Com este design, soluções de escalabilidade off-chain são utilizadas para permitir taxas de transferência mais altas.

#### 2.1.3 Soluções de Escalamento Off-chain por Meio de uma Camada 2

Em protocolos de camada 2, a blockchain de camada base atua como uma camada de acordo (ou compromisso), enquanto uma rede de segunda camada roteia provas criptográficas que permitem aos participantes "recebam" a criptomoeda. Todas as atividades do segunda camada são criptograficamente seguras pela blockchain subjacente e a camada base é usada apenas para liquidar valores entrando/saindo da rede de segunda camada, e para resolução de disputas. Esses designs operam sem delegação de custódia (ou risco de perda) de fundos e habilitam transações instantâneas e praticamente sem custo.

Essas tecnologias demonstram como uma rede de armazenamento de valor como o Bitcoin pode ser usada para os pagamentos do dia a dia. O exemplo mais típico de uma solução de camada 2 em prática é um canal de pagamento entre um consumidor e uma cafeteria. Vamos supor que Alice visite a Cafeteria Bitcoin toda manhã. No começo do mês, ela deposita fundos em um canal de pagamento Lightning que ela abriu com a cafeteria. Conforme ela visita todo dia, ela criptograficamente assina o direito da cafeteria a pegar uma quantia dos fundos, em troca por seu café. Essas transações acontecem de forma instantânea e são completamente peer-to-peer, "off-chain", permitindo uma experiência de usuário mais sucinta. O canal Lightning é trustless, Alice ou a cafeteria podem fechar o canal a qualquer momento, movendo os fundos que lhes são devidos naquele momento.

Tecnologias de canais de pagamento como Lightning são apenas um exemplo de uma técnica de escalabilidade off-chain; existem muitas tecnologias em maturação que podem escalar a taxa de transferência de blockchains de forma segura desta forma. Enquanto canais de pagamento incluem acordos off-chain para canalizar saldos entre duas partes, canais de estado incluem acordos off-chain para estados arbitrários entre os participantes do canal. Essa generalização pode ser a base de aplicações escaláveis, trustless e descentralizadas. Um único canal de estado pode até ser utilizado por múltiplas aplicações, permitindo uma eficiência ainda maior. Quando uma parte está pronta para sair do canal, ela pode submeter a prova criptográfica acordada para a blockchain, o que irá então executar o estado de transações acordado.

Uma side-chain (cadeia paralela) é outra construção que permite uma taxa de transferência aumentada, embora através de operadores de blockchain terceiros confiáveis. Com uma estaca de duas pontas para uma blockchain com consenso confiável e trustless, os fundos podem ser movidos para frente e para trás entre a cadeia principal e a side-chain. Isso permite um volume mais alto de transações confiáveis na side-chain, com liquidação posterior na cadeia principal. Transações em side-chains tem taxas mínimas, confirmação rápida e alta taxa de transferência. Embora side-chains ofereçam uma experiência superior em certos aspectos, elas comprometem a segurança. No entanto, há um ótimo negócio em pesquisas sobre side-chains trustless, que podem prover as mesmas melhorias de performance sem comprometer segurança.

Um exemplo de uma tecnologia de side-chain trustless é a Plasma (coberta na seção 5.4), uma arquitetura de side-chain que alavanca uma raiz de confiança em uma blockchain com amplo consenso global. Cadeias Plasma oferecem as mesmas melhorias de performance que side-chains centralizadas, contudo o fazem enquanto também oferecem garantias de segurança. No evento de um operador de cadeia Plasma seja malicioso ou esteja com defeito, usuários são providos com um mecanismo que permite que eles, de forma segura, saquem seus bens da side-chain para a cadeia principal. Isso é feito sem a cooperação do operador da cadeia Plasma, oferecendo aos usuários a conveniência de transações de side-chain, assim como a segurança de uma blockchain de camada 1.

Escalabilidade off-chain permite descentralização, segurança e escalabilidade. Ao mover tudo, exceto pelas transações de liquidação e disputas para off-chain, o consenso de uma blockchain pública é usado de forma eficaz. Diversos protocolos de camada 2 podem ser implementados com base em requisitos de aplicação, proporcionando flexibilidade aos desenvolvedores e usuários. Conforme mais participantes são adicionados à rede, a performance não é impactada e todas as partes podem compartilhar as garantias de segurança oferecidas pelo consenso de camada 1. 

### 2.2 Sustentabilidade

Sustentar uma operação de longo prazo de uma blockchain autônoma e sem proprietários apresenta um grande desafio. Incentivos devem ser balanceados entre os diversos stakeholders e o sistema deve ser projetado de forma que permita operações de nó completo e ampla verificabilidade pública. Os requisitos de hardware devem se manter razoáveis, ao mesmo tempo em que suportam uma rede global e aberta.

Adicionalmente, uma vez que uma blockchain pública está em operação, é bem difícil mudar as regras subjacentes que governam o protocolo. Do começo, o sistema deve ser projetado para ser sustentável. Nesse sentido, conduzimos um inventário completo de desafios na construção de blockchains permissionless e sustentáveis.

#### 2.2.1 Descentralização

Uma das maiores ameaças às blockchains públicas a longo prazo é uma barreira cada vez maior de participação independente e verificação das transações, refletidas no custo de uma operação de nó completo. Nós completos permitem que os participantes da blockchain verifiquem de forma independente o estado/histórico on-chain, e mantém mineradores ou validadores da rede responsáveis por recusarem-se a rotear blocos inválidos. Conforme o custo de nós completos cresce e seus números diminuem, participantes da rede são cada vez mais forçados a confiar em operadores profissionais de serviço para proverem ambos o histórico e o estado atual, erodindo o modelo de confiança fundamental de blockchains abertas e permissionless.

Para um nó completo acompanhar a progressão da blockchain, ele deve possuir uma taxa de transferência de processamento adequada para validar as transações, de largura de banda suficiente para receber transações e capacidade de armazenamento para guardar o estado global por inteiro. Para controlar os custos de um nó completo em operação, o protocolo deve tomar medidas para limitar a taxa de transferência ou a capacidade de crescimento de todos os três desses recursos. A maioria dos protocolos de blockchain limitam sua taxa de transferência de processamento ou de largura de banda, mas muito poucos limitam o crescimento do estado global. Conforme essas cadeias crescem em tamanho e comprimento de operação, os custos de operações de nó completo irão aumentar irreversivelmente. 

#### 2.2.2 Modelos Econômicos

Embora tenha havido muitas pesquisas sobre protocolos de consenso nos últimos anos, acreditamos que a cripto-economia é um campo pouco estudado. Em termos gerais, os modelos cripto-econômicos atuais para os protocolos de camada 1 são primariamente focados em incentivos e punições para garantir o consenso de rede, e tokens nativos são usados principalmente para pagar taxas de transação ou satisfazer requisitos de staking que fornecem resistência a ataques Sybil.

Acreditamos que um modelo econômico bem projetado deve ir além do processo de consenso e também garantir a sustentabilidade do protocolo a longo prazo. Em particular, o modelo econômico deve ser projetado com os seguintes objetivos:

- a rede deve ter uma maneira sustentável de compensar os provedores de serviço (tipicamente mineradores ou validadores), garantindo que a rede se mantenha sustentavelmente segura.
- a rede deve ter uma maneira sustentável de manter uma baixa barreira à participação, garantindo que se mantenha descentralizada ao longo do tempo.
- os recursos da rede pública devem ser alocados de forma eficiente e justa
- o token nativo da blockchain deve ter valor intrínseco

#### 2.2.3 Análise do Modelo Econômico do Bitcoin

O protocolo do Bitcoin limita o tamanho dos blocos e impõe um tempo de bloco fixo. Isso torna a taxa de transferência de largura de banda da rede um recurso escasso pelo qual os usuários devem licitar através de taxas de transação. O Bitcoin Script não permite loops, tornando o comprimento do script uma boa aproximação de sua complexidade computacional. Em geral, uma maior demanda por espaço de bloco se traduz em taxas de transação mais caras aos usuários. Adicionalmente, quanto mais entradas, saídas ou passos de processamento estão envolvidos em uma transação, mais caro também o usuário irá pagar por ela.

O valor intrínseco do Bitcoin vem quase inteiramente de seu prêmio monetário (a disposição da sociedade de tratá-lo como dinheiro) e em particular, a disposição a mantê-lo como uma reserva de valor. Como a receita do minerador é denominada em BTC, essa percepção deve se manter para que o modelo econômico do Bitcoin seja sustentável. Em outras palavras, o modelo de segurança do Bitcoin é circular - depende da crença coletiva de que a rede é segura de forma sustentável e pode, portanto, ser usada como uma reserva monetária de valor.

O limite de tamanho de bloco do Bitcoin efetivamente define a barreira para a participação na rede - quanto menor o limite de tamanho de bloco, mais fácil é para não profissionais executarem nós completos. O estado global do Bitcoin é seu conjunto UTXO, com sua taxa de crescimento também efetivamente limitada pelo limite de tamanho do bloco. Os usuários são incentivados a criar e utilizar UTXOs de forma eficiente; a criação de mais UTXOs se traduz em taxas de transação mais caras. No entanto, nenhum incentivo é fornecido para encorajar a combinação de UTXOs e a redução do tamanho do estado global; uma vez que um UTXO é criado, ele ocupará o estado global gratuitamente até que seja gasto.

O modelo econômico baseado em taxas de transação do Bitcoin é um modelo justo para alocar sua taxa de transferência de largura de banda, o recurso escasso imposto pelo protocolo. É um modelo econômico adequado para um sistema de pagamento peer-to-peer, mas é uma escolha ruim para uma plataforma de verdadeira reserva de valor. Os usuários de Bitcoin que utilizam o blockchain para armazenar valor pagam taxas de transação apenas uma vez, mas podem ocupar o estado para sempre, desfrutando de segurança contínua fornecida por mineradores, que são obrigados a fazer investimentos contínuos em recursos.

O Bitcoin tem um limite máximo de hard-cap e sua nova emissão por meio de recompensas em bloco acabará caindo para zero. Isso pode causar dois problemas:

Primeiramente, se o Bitcoin continuar a ter sucesso como reserva de valor, o valor unitário do BTC continuará a aumentar e o valor total que a rede protege também aumentará (à medida que mais valor monetário passa pela rede). Uma plataforma de armazenamento de valor deve ser capaz de aumentar seu orçamento de segurança à medida que o valor que protege aumenta com o tempo, caso contrário, ela convida os invasores a dobrar os gastors e roubar os ativos da rede.

Quando o custo para quebrar a segurança do protocolo é menor que o lucro que eles podem obter agindo honestamente, os invasores sempre atacarão. Isso é análogo a uma cidade que tem que aumentar seus gastos militares à medida que a riqueza dentro da cidade aumenta. Sem esse investimento, mais cedo ou mais tarde a cidade será atacada e saqueada.

Com a existência de recompensas de bloco, o Bitcoin é capaz de escalar a segurança para o valor agregado que armazena - se o preço do Bitcoin dobrar, a receita que os mineradores recebem das recompensas em bloco também dobrará, portanto, eles podem produzir o dobro da taxa de hash, tornando a rede duas vezes mais cara para ser atacada.

No entanto, isso muda quando as recompensas de bloco previsíveis caem para zero. Mineradores terão que depender inteiramente das taxas de transação; sua receita não será mais escalonada para o valor do ativo do Bitcoin, mas será determinada pela demanda de transação da rede. Se a demanda de transação não for alta o suficiente para preencher o espaço do bloco disponível, as taxas de transação totais serão minúsculas. Como as taxas de transação são estritamente função da demanda de espaço de bloco e independentes do preço de um Bitcoin, isso terá um impacto profundo no modelo de segurança do Bitcoin. Para que o Bitcoin permaneça seguro, teríamos que assumir uma demanda de transação consistente e com excesso de capacidade, que também se ajusta ao preço do Bitcoin. Essas são suposições muito fortes. 

Em segundo lugar, quando as recompensas de bloco previsíveis param, a variação na renda por bloco dos mineradores aumenta e fornece incentivos para que estes façam uma bifurcação, em vez de avançar a blockchain. No caso extremo, quando o pool de memória de um minerador está vazio e ele recebe um bloco carregado de taxas, o incentivo maior é fazer uma bifurcação da cadeia e roubar as taxas, em vez de avançar a corrente e produzir um bloco potencialmente sem receita[5]. Isso é conhecido como desafio de "cobrança de taxas" na comunidade Bitcoin, para o qual uma solução satisfatória ainda não foi encontrada, sem remover o hard-cap do Bitcoin.

#### 2.2.4 Análise do Modelo Econômico de Plataformas de Contratos Inteligentes

O modelo econômico típico de plataformas de contratos inteligentes enfrenta ainda mais desafios. Vamos usar o Ethereum como exemplo. O script do Ethereum permite loops, portanto, o comprimento de um script não reflete a complexidade computacional do script. Esta é a razão pela qual o Ethereum não limita o tamanho máximo do bloco ou a taxa de transferência de largura da banda, mas a taxa de transferência computacional (expressa no limite de gás do bloco).

Para ter suas transações registradas na blockchain do Ethereum, os usuários licitam no custo por computação que estão dispostos a pagar em taxas de transação. O Ethereum usa o conceito de "gás" como medida de custo computacional com preço em ETH, e o controle de taxa do "preço do gás" garante que o custo por etapa de cálculo seja independente dos movimentos de preço do token nativo. O valor intrínseco do token ETH vem de sua posição como o token de pagamento da plataforma de computação descentralizada; é a única moeda que pode ser usada para pagar cálculos no Ethereum.

O estado global do Ethereum é representado pelo teste de estado do EVM, a estrutura de dados que contém os saldos e o estado interno de todas as contas. Quando novas contas ou valores de contrato são criados, o tamanho do estado global se expande. O Ethereum cobra quantidades fixas de gás pela inserção de novos valores em seu armazenamento de estado e oferece um "estipêndio de gás" fixo que compensa os custos de gás de uma transação quando os valores são removidos.

Um modelo de armazenamento "pague uma vez, ocupe para sempre" não corresponde à estrutura de custos contínua de mineradores e nós completos, e o modelo não oferece incentivos para os usuários removerem voluntariamente o estado ou removerem o estado mais cedo. Como resultado, o Ethereum experimentou um rápido crescimento do tamanho de seu estado. Um tamanho de estado maior diminui o processamento da transação e aumenta o custo operacional de nós completos. Sem fortes incentivos para limpar o estado, essa é uma tendência que deve continuar.

Semelhante ao Bitcoin, o preço do gás orientado pela demanda da Ethereum é um modelo justo para alocar seu rendimento computacional, o recurso escasso da plataforma. O modelo também atende ao propósito do Ethereum como um sistema de computação descentralizado. No entanto, seu modelo de taxa de armazenamento de estado não corresponde à sua proposta potencial como um estado descentralizado ou plataforma de armazenamento de ativos. Sem um custo para armazenamento de estado de longo prazo, sempre será do interesse dos usuários ocupar o estado para sempre de graça. Sem a escassez de capacidade de armazenamento do estado, nem um mercado, nem uma dinâmica de oferta e demanda pode ser estabelecida.

Ao contrário do Bitcoin, que especifica o limite de tamanho do bloco em seu protocolo principal, o Ethereum permite que os mineradores ajustem dinamicamente o limite de gás do bloco ao produzir blocos. Mineradores com hardware avançado e largura de banda significativa são capazes de produzir mais blocos, dominando efetivamente este processo de votação. Seu interesse é ajustar o limite de gás do bloco para cima, elevar a barra de participação e forçar os mineradores menores a saírem da competição. Este é outro fator que contribui para o rápido aumento do custo da operação de nó completo.

Plataformas de contratos inteligentes como o Ethereum são plataformas de múltiplos ativos. Eles suportam a emissão e transações de todos os tipos de cripto-ativos, normalmente representados como "tokens". Eles também fornecem segurança não apenas para seus próprios tokens nativos, mas também para o valor de todos os ativos criptográficos da plataforma. "Armazenamento de valor" em um contexto de múltiplos ativos, portanto, se refere à propriedade de preservação de valor que beneficia tanto os tokens nativos da plataforma quanto os cripto-ativos armazenados na plataforma.

Com suas recompensas em bloco, o Bitcoin tem um excelente modelo econômico de "reserva de valor". Os mineradores recebem uma recompensa em bloco fixa denominada em BTC e, portanto, sua renda aumenta junto com o preço do BTC. Portanto, a plataforma tem a capacidade de aumentar a receita dos mineradores para aumentar a segurança (medida pelo custo do ataque) enquanto mantém um modelo econômico sustentável.

Para plataformas com vários ativos, torna-se muito mais desafiador atender a esse requisito, porque o "valor" pode ser expresso com cripto-ativos além do token nativo. Se o valor dos cripto-ativos protegidos pela plataforma aumenta, mas a segurança da rede não, torna-se mais lucrativo atacar o processo de consenso da plataforma para dobrar o gasto com cripto-ativos armazenados na plataforma.

Para que uma plataforma de contrato inteligente de múltiplos ativos funcione como reserva de valor, incentivos adequados devem ser colocados em prática para alinhar o crescimento do valor dos ativos de uma rede com sua segurança subjacente. Ou dito de outra forma, o token nativo da plataforma deve ser uma boa captura de valor em relação ao valor do ativo agregado da plataforma. Se o valor intrínseco do token nativo de uma plataforma for limitado ao pagamento da taxa de transação, seu valor será determinado exclusivamente pela demanda de transação, em vez da demanda de armazenamento de ativos.

As plataformas de contratos inteligentes que não são projetadas para funcionar como reserva de valor precisam contar com o prêmio monetário do token nativo (a disposição das pessoas em manter os tokens além de seu valor intrínseco) para oferecer suporte à segurança contínua. Isso só é viável se uma plataforma domina com recursos exclusivos que não podem ser encontrados em outro lugar, ou supera as outras oferecendo o menor custo possível de transações.

O Ethereum atualmente desfruta de tal domínio e pode, portanto, manter seu prêmio monetário. No entanto, com o aumento de plataformas concorrentes, muitas projetadas para taxas mais altas de transações por segundo e fornecendo funcionalidade semelhante, é uma questão em aberto se a confiança em um prêmio monetário por si só pode sustentar a segurança de uma plataforma de blockchain, especialmente se os tokens nativos não forem explicitamente projetados ou acreditados ser dinheiro. Além disso, mesmo se uma plataforma puder fornecer recursos exclusivos, seu prêmio monetário pode ser abstraído pela interface do usuário por meio de trocas eficientes (muito provavelmente quando a adoção em massa do blockchain finalmente chegar). Os usuários manteriam ativos com os quais estão mais familiarizados, como Bitcoin ou moedas estáveis, e adquiririam tokens de plataforma bem a tempo de pagar as taxas de transação. Em qualquer dos casos, a base da criptoeconomia de uma plataforma entraria em colapso.

As plataformas de múltiplos ativos de camada 1 devem fornecer segurança sustentável para todos os cripto-ativos que protegem. Em outras palavras, eles precisam ter um modelo econômico projetado para uma reserva de valor.

#### 2.2.5 Financiamento do Desenvolvimento do Protocolo Central 

Blockchains permissionless públicas são infraestruturas públicas. O desenvolvimento inicial desses sistemas requer uma grande quantidade de fundos e, uma vez em operação, requerem manutenção e atualizações contínuas. Sem pessoas dedicadas para manter esses sistemas, eles correm o risco de erros catastróficos e operação abaixo do ideal. Os protocolos Bitcoin e Ethereum não fornecem um mecanismo nativo para garantir o financiamento do desenvolvimento contínuo, portanto, contam com o envolvimento contínuo de empresas com interesses alinhados e comunidades de código aberto altruístas.

Dash foi o primeiro projeto a utilizar um tesouro para garantir que o desenvolvimento contínuo fosse financiado dentro do protocolo. Embora apoie de forma sustentável o desenvolvimento do protocolo, este design compromete a sustentabilidade do valor da criptomoeda. Como a maioria dos tesouros de blockchain, esse modelo depende de financiamento baseado na inflação, o que corrói o valor dos títulos de longo prazo. 

A Nervos Network usa um modelo de tesouro que fornece financiamento sustentável para o desenvolvimento central. Os fundos do tesouro vêm de uma inflação planejada de detentores de token de curto prazo, enquanto os efeitos dessa inflação são mitigados para detentores de longo prazo. Mais informações sobre este mecanismo estão descritas em (4.6).

### 2.3 Interoperabilidade

A interoperabilidade entre blockchains é um tópico frequentemente discutido e muitos projetos foram propostos especificamente para lidar com esse desafio. Com transações confiáveis ​​entre blockchains, verdadeiros efeitos de rede podem ser obtidos na economia descentralizada.

O primeiro exemplo de interoperabilidade de blockchain foram as trocas atômicas entre Bitcoin e Litecoin. A troca trustless de Bitcoin por Litecoin e vice-versa é possível não por meio de mecanismos dentro do protocolo, mas por meio de um padrão criptográfico compartilhado (especificamente o uso da função hash SHA2-256).

Da mesma forma, o design do Ethereum 2.0 permite a interconexão de muitas cadeias de fragmentos, todas executando o mesmo protocolo e utilizando as mesmas primitivas criptográficas. Essa uniformidade será valiosa ao personalizar o protocolo para comunicação entre fragmentos, no entanto, o Ethereum 2.0 não será interoperável com outras blockchains que não utilizem os mesmos primitivos criptográficos.

Redes de blockchains como Polkadot ou Cosmos vão um passo além, permitindo que blockchains construídos com a mesma estrutura (Cosmos SDK para Cosmos e Substrate para Polkadot) se comuniquem e interajam entre si. Essas estruturas fornecem aos desenvolvedores alguma flexibilidade na construção de seus próprios protocolos e garantem a disponibilidade de primitivas criptográficas idênticas, permitindo que cada cadeia analise os blocos uns dos outros e faça validação cruzada das transações. No entanto, ambos os protocolos dependem de pontes ou "zonas de pegging" para se conectar a blockchains que não são construídas com seus próprios frameworks, introduzindo uma camada adicional de confiança. Para demonstrar: embora Cosmos e Polkadot habilitem "redes de blockchains", as redes Cosmos e Polkadot não são projetadas para serem interoperáveis ​​entre si.

A criptoeconomia das redes cross-chain também pode precisar de um estudo mais aprofundado. Para Cosmos e Polkadot, os tokens nativos são usados ​​para taxas de staking, governança e transação. Deixando de lado a dinâmica criptoeconômica introduzida pelo staking, que por si só não pode fornecer um valor intrínseco de token nativo (discutido em 4.2.4), a confiança nas transações entre cadeias para capturar o valor do ecossistema pode ser um modelo fraco. Em particular, as transações entre cadeias são uma fraqueza, não um ponto forte das redes de várias cadeias, assim como as transações entre fragmentos são uma fraqueza dos bancos de dados fragmentados. Eles introduzem latência, bem como a perda de atomicidade e composição. Há uma tendência natural para os aplicativos que precisam interagir uns com os outros para eventualmente passarem a residir na mesma blockchain para reduzir a sobrecarga da cross-chain (cadeia cruzada), reduzindo a demanda por transações cross-chain e, portanto, a demanda pelo token nativo.

As redes cross-chain se beneficiam dos efeitos de rede - quanto mais cadeias interconectadas houver em uma rede, mais valiosa ela será e mais atraente para novos participantes em potencial na rede. Idealmente, esse valor seria capturado pelo token nativo e usado para estimular ainda mais o crescimento da rede. No entanto, em uma rede de segurança agrupada como a Polkadot, o custo mais alto de participação na rede torna-se um impedimento para que a rede agregue mais valor. Em uma rede fracamente conectada como a Cosmos, se assumirmos a mesma demanda e taxas de transação cross-chain, o custo mais alto da participação de staking reduz o retorno esperado para validadores, desencorajando participações de staking adicionais.

Com sua abordagem em camadas, a Nervos Network também é uma rede multi-chain. Arquitetonicamente, a Nervos usa o modelo de célula e uma máquina virtual de baixo nível para oferecer suporte à personalização real e primitivas criptográficas criadas pelo usuário, permitindo a interoperabilidade entre blockchains heterogêneos (coberto em 4.4.1). De forma criptoeconômica, a Nervos Network concentra o valor (em vez da passagem de mensagens) em sua cadeia raiz. Esse mecanismo aumenta o orçamento de segurança da rede à medida que o valor agregado protegido pela rede aumenta. Isso é abordado em detalhes em (4.4). 

## 3. Princípios Centrais da Nervos Network

A Nervos é uma rede em camadas construída para dar suporte às necessidades da economia descentralizada. Há vários motivos pelos quais acreditamos que uma abordagem em camadas é a maneira certa de construir uma rede blockchain. Existem muitos trade-offs bem conhecidos na construção de sistemas blockchain, como descentralização/escalabilidade, neutra/compatível, privacidade/abertura, armazenamento de valor/custo de transação e solidez criptográfica/experiência do usuário. Acreditamos que todos esses conflitos surgem devido às tentativas de abordar preocupações completamente opostas com uma única blockchain.

Acreditamos que a melhor maneira de construir um sistema não é construir uma única camada abrangente, mas sim separar as preocupações e tratá-las em camadas diferentes. Ao fazer isso, a blockchain da camada 1 pode se concentrar em ser uma infraestrutura pública segura, neutra, descentralizada e aberta, enquanto as redes menores da camada 2 podem ser especialmente projetadas para se adequar melhor ao contexto de seu uso.

Na Nervos Network, o protocolo da camada 1 (o CKB, Common Knowledge Base - Base de Conhecimento Comum) é a camada de preservação de valor de toda a rede. É filosoficamente inspirado pelo Bitcoin e é uma blockchain aberta, pública e de prova de trabalho (proof of work), projetado para ser o mais seguro e resistente à censura possível, para servir como um guardião descentralizado de valor e cripto-ativos. Os protocolos da camada 2 alavancam a segurança da blockchain da camada 1 para fornecer escalabilidade ilimitada e taxas de transação mínimas, e também permitem trade-offs específicos de aplicativo em relação a modelos de confiança, privacidade e finalidade.

Estes são os princípios básicos que levaram ao design da Nervos Network:

- Uma blockchain sustentável e com múltiplos ativos de camada 1 deve ser projetada de forma criptoeconômica para ser uma reserva de valor.
- A camada 2 oferece as melhores opções de escalabilidade, trazendo recursos transacionais quase ilimitados, custos de transação mínimos e uma experiência de usuário aprimorada. As blockchains de camada 1 devem ser projetados para complementar, e não competir com as soluções de camada 2.
- Prova de trabalho como um método de resistência Sybil é essencial para blockchains de camada 1.
- O blockchain de camada 1 deve fornecer um modelo de programação genérico para protocolos interativos e interoperabilidade de blockchain, e permitir que o protocolo seja maximamente customizável e fácil de atualizar.
- Para alocar melhor os recursos e evitar a tragédia dos comuns, o armazenamento de estado deve ter um modelo de propriedade claro e refinado. Para entregar recompensas consistentes de longo prazo aos mineradores (independentemente da demanda de transação), a ocupação do estado deve ter um custo contínuo. 

## 4. A Nervos CKB (Base de Conhecimento Comum)

### 4.1 Visão Geral

"Conhecimento comum" é definido como algo que é conhecido por todos ou quase todos, geralmente com referência à comunidade na qual o termo é usado. No contexto de blockchains em geral, e da Nervos Network em particular, "conhecimento comum" refere-se ao estado verificado por consenso global e aceito por todos na rede.

As propriedades do conhecimento comum nos permitem tratar coletivamente as criptomoedas armazenadas em blockchains públicas como dinheiro. Por exemplo, os saldos e o histórico de todos os endereços no Bitcoin são de conhecimento comum para os usuários do Bitcoin, porque eles são capazes de replicar de forma independente o ledger compartilhado, verificar o estado global desde o bloco de gênese e saber que qualquer outra pessoa pode fazer o mesmo. Esse conhecimento comum permite que as pessoas façam transações totalmente ponto a ponto, sem confiar em terceiros.

O Nervos Common Knowledge Base (CKB) foi projetado para armazenar todos os tipos de conhecimento comum, não se limitando a dinheiro. Por exemplo, o CKB pode armazenar ativos criptográficos definidos pelo usuário, como tokens fungíveis e não fungíveis, bem como provas criptográficas valiosas que fornecem segurança para protocolos de camada superior, como canais de pagamento (5.2) e cadeias de comprometimento (5.4).

Tanto o Bitcoin quanto o Nervos CKB são sistemas de armazenamento e verificação de conhecimento comum. O Bitcoin armazena seu estado global como o conjunto UTXO e verifica as transições de estado por meio de regras e scripts embutidos em transações. O Nervos CKB generaliza a estrutura de dados e recursos de script do Bitcoin, armazena o estado global como o conjunto de células programáveis ​​ativas e verifica as transições de estado por meio de scripts Turing-completos definidos pelo usuário que são executados em uma máquina virtual.

Embora o Nervos CKB tenha recursos completos de contratos inteligentes, como os do Ethereum e outras plataformas, seu modelo econômico é projetado para preservação do conhecimento comum, em vez de pagamento por computação descentralizada. 

### 4.2 Consenso

O Consenso de Nakamoto (NC) do Bitcoin é bem recebido devido à sua simplicidade e baixa sobrecarga de comunicação. No entanto, o NC sofre de duas desvantagens: 1) sua taxa de transferência de processamento de transações está longe de ser satisfatória e 2) é vulnerável a ataques de mineração egoísta, nos quais os invasores podem obter recompensas de bloco adicionais ao se desviarem do comportamento prescrito do protocolo.

O protocolo de consenso da CKB é uma variante do NC que aumenta seu limite de desempenho e resistência à mineração egoísta, mantendo seus méritos. Ao identificar e eliminar o gargalo na latência de propagação de bloco do NC, nosso protocolo suporta intervalos de bloco muito curtos sem sacrificar a segurança. Um intervalo de bloco reduzido não apenas aumenta o rendimento, mas também reduz a latência de confirmação de transação. Ao incorporar todos os blocos válidos no cálculo de ajuste de dificuldade, a mineração egoísta não é mais lucrativa em nosso protocolo. 

#### 4.2.1 Aumentando o Rendimento

A Nervos CKB aumenta a taxa de transferência do consenso PoW com um algoritmo de consenso derivado do Consenso Nakamoto. O algoritmo usa a taxa de órfãos da blockchain (a porcentagem de blocos válidos que não fazem parte da cadeia canônica) como uma medida de conectividade na rede.

O protocolo visa uma taxa de orfãos fixa. Em resposta a uma baixa taxa de órfãos, a dificuldade do alvo é reduzida (aumentando a taxa de produção de bloco) e quando a taxa de órfãos cruza um limite definido, a dificuldade alvo é aumentada (diminuindo a taxa de produção de bloco).

Isso permite a utilização de todos os recursos de largura de banda da rede. Uma baixa taxa de órfãos indica que a rede está bem conectada e pode lidar com uma maior transmissão de dados; o protocolo então aumenta o rendimento sob essas condições. 

#### 4.2.2 Eliminando o Gargalo de Propagação de Blocos

O gargalo em qualquer rede blockchain é a propagação de bloco. O protocolo de consenso da Nervos CKB elimina esse gargalo, modificando a confirmação da transação em um processo de duas etapas: 1) propor e 2) confirmar.

Uma transação deve primeiro ser proposta na "zona de proposta" de um bloco (ou um de seus blocos tios). A transação será então confirmada se aparecer na "zona de confirmação" de um bloco dentro de uma janela definida após sua proposta. Este projeto elimina o gargalo de propagação do bloco, pois as transações confirmadas de um novo bloco já terão sido recebidas e verificadas por todos os nós quando propostas. 

#### 4.2.3 Mitigando Ataques de Selfish Mining

Um dos ataques mais fundamentais ao Consenso de Nakamoto é a mineração egoísta. Nesse ataque, mineradores mal-intencionados ganham recompensas injustas por blocos ao tornar deliberadamente órfãos blocos minerados por outros.

Os pesquisadores observam que a oportunidade de lucro injusta está enraizada no mecanismo de ajuste de dificuldade do Consenso de Nakamoto, que negligencia blocos órfãos ao estimar o poder de computação da rede. Isso leva a uma menor dificuldade de mineração e a maiores recompensas de bloco por média de tempo.

O protocolo de consenso do Nervos CKB incorpora blocos-tio no cálculo de ajuste de dificuldade, tornando a mineração egoísta não mais lucrativa. Isso é válido independentemente da estratégia ou duração do ataque; um minerador é incapaz de obter recompensas injustas por meio de qualquer combinação de mineração honesta e egoísta.

Nossa análise mostra que, com um processo de confirmação de transação em duas etapas, a mineração egoísta de fato também é eliminada por meio de uma janela de tempo de ataque limitada.

Para uma compreensão detalhada do nosso protocolo de consenso, leia [aqui] (https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0020-ckb-consensus-protocol/0020-ckb-consensus-protocol.md ) 

#### 4.2.4 Prova de Trabalho (Proof of Work) contra Prova de Participação (Proof of Stake)

Os sistemas de Prova de Trabalho (PoW) e Prova de Participação (PoS) são vulneráveis ​​a concentrações de poder; no entanto, as qualidades dos sistemas fornecem realidades operacionais muito diferentes para aqueles que estão no poder.

A mineração por PoW incorre em despesas do mundo real que podem exceder as receitas de mineração sem supervisão de custo diligente. Os que estão no poder devem permanecer inovadores, buscar estratégias de negócios sólidas e continuar a investir em infraestrutura para permanecer dominantes. Equipamentos de mineração, operações de pools de mineração e acesso a energia barata estão todos sujeitos a mudanças decorrentes da inovação tecnológica. É difícil manter a monopolização de todos os três por longos períodos de tempo.

Em contraste, os criadores de blocos em sistemas PoS são recompensados ​​de forma determinística, com base no valor apostado, com requisitos de capital operacional muito baixos. À medida que o sistema cresce, o impacto das vantagens naturais proporcionadas às empresas e indivíduos pela primeira vez aumentará. Em um sistema PoS, é possível que o poder se concentre nas mãos de alguns participantes (stakers). Embora os sistemas PoW tenham um problema semelhante com a concentração de mineração, o custo para permanecer no poder em um sistema PoS é significativamente menor.

Além disso, os validadores de PoS têm um poder único: controle do conjunto de validadores. A aceitação de uma transação que permite a um validador ingressar no grupo de consenso está nas mãos dos validadores existentes. Esforços de conluio para influenciar o conjunto de validadores por meio de censura de transação e manipulação de pedidos seriam difíceis de serem detectados, bem como de serem punidos. Por outro lado, a participação de consenso em sistemas PoW é verdadeiramente aberta e não está sujeita à estrutura de poder atual. As vantagens não são dadas aos primeiros participantes do sistema.

Em relação à economia do token, embora se acredite que o staking pode atrair capital procurando obter rendimento (e, portanto, aumentar a demanda pelo token nativo), este não é o cenário completo. Todos os projetos de PoS acabarão vendo sua taxa de participação (staking) se estabilizar, e o capital que entra e sai do pool de capitais apostados seria praticamente o mesmo. O mecanismo de staking por si só não aumentará a demanda pelo token nativo. Em outras palavras, embora a introdução do staking forneça demanda para o token nativo na fase inicial de um projeto (conforme a taxa de staking aumenta), o staking sozinho não consegue fornecer demanda de longo prazo para o token nativo e, portanto, não pode ser o único valor intrínseco de um token nativo.

Os detentores de tokens de longo prazo (holders) em um sistema PoS têm 3 opções: eles podem 1) gerenciar a infraestrutura e executar um nó de validação por conta própria para receber uma nova emissão, 2) delegar seus tokens a um terceiro e confiar em sua integridade e infraestrutura, ou 3) ter o valor de seus tokens diluído pela emissão em andamento. Nenhuma dessas opções é particularmente atraente para holders de longo prazo orientados por reserva de valor.

Acreditamos que a participação permissionless da PoW é um requisito para a infraestrutura na base da atividade econômica global. O principal objetivo da camada 1 é garantir que a blockchain seja o mais descentralizada, segura e neutra possível. Embora os sistemas PoS tenham um papel a desempenhar na economia descentralizada, em nossa opinião eles não atendem aos requisitos de uma camada 1 verdadeiramente aberta e descentralizada. 

#### 4.2.5 Função de Prova de Trabalho

Os blocos da Nervos CKB podem ser propostos por qualquer nó, desde que 1) o bloco seja válido; e 2) o proponente resolveu um quebra-cabeça computacionalmente difícil chamado de prova de trabalho. O quebra-cabeça da prova de trabalho é definido em termos do bloco que está sendo proposto; isso garante que a solução do quebra-cabeça identifique exclusivamente um bloco.

A prova de trabalho do Bitcoin requer encontrar um nonce* válido de forma que o resultado da aplicação de uma função hash no cabeçalho do bloco satisfaça um certo nível de dificuldade. Para Bitcoin, a função hash é SHA2–256 com iteração duas vezes. Embora o SHA2 seja uma boa escolha para o Bitcoin, o mesmo não é verdade para as criptomoedas que vêm depois dele. Uma grande quantidade de hardware dedicado foi desenvolvida para extrair Bitcoin, grande parte do qual fica ocioso, tendo se tornado obsoleto por melhorias de eficiência.

Uma nova criptomoeda utilizando o mesmo quebra-cabeça de prova de trabalho tornaria esse hardware obsoleto útil novamente. Mesmo hardware atualizado pode ser alugado e reaproveitado para extrair uma nova moeda. A distribuição do poder de mineração para uma moeda baseada em SHA2 seria muito difícil de prever e suscetível a mudanças repentinas e grandes. Este argumento também se aplica a otimizações algorítmicas sob medida para SHA2, que foram desenvolvidas para tornar o processamento de software da função mais barato também.

Para uma nova criptomoeda, faz sentido definir o quebra-cabeça da prova de trabalho em termos de uma função que ainda não foi usada por outras criptomoedas. Para a Nervos CKB, demos um passo além e optamos por definí-la em termos de uma função de prova de trabalho que não poderia ter sido objeto de otimização prematura, porque é nova.

No entanto, a indisponibilidade pretendida de hardware de mineração é apenas o caso inicialmente. No longo prazo, as implantações de hardware de mineração dedicado são benéficas, aumentando significativamente os desafios de ataque à rede. Portanto, além de ser nova, uma função de prova de trabalho ideal para uma nova criptomoeda também é simples, reduzindo significativamente a barreira para o desenvolvimento de hardware.

A segurança é o terceiro objetivo óbvio do design. Embora uma vulnerabilidade conhecida possa ser explorada por todos os mineradores igualmente, e simplesmente resulte em uma dificuldade maior, uma vulnerabilidade não divulgada pode levar a uma otimização de mineração que fornece ao(s) descobridor(es) uma vantagem em excesso de sua parcela de poder de mineração contribuída. A melhor maneira de evitar essa situação é apresentar um forte argumento a favor da invulnerabilidade. 

#### 4.2.6 Eaglesong

Eaglesong é uma nova função hash desenvolvida especificamente para a prova de trabalho do Nervos CKB, mas também é adequada em outros casos de uso em que uma função hash segura é necessária. Os critérios de design foram exatamente os listados acima: novidade, simplicidade e segurança. Queríamos um design que fosse simultaneamente novo o suficiente para constituir um pequeno passo em frente para a ciência, bem como próximo o suficiente dos designs existentes para apresentar um forte argumento de segurança.

Para tanto, optamos por instanciar a construção da esponja (como usada no Keccak / SHA3) com uma permutação construída a partir de operações ARX (adição, rotação e xor); o argumento para sua segurança é baseado na estratégia de trilha ampla (o mesmo argumento subjacente ao AES).

Até onde sabemos, Eaglesong é a primeira função hash (ou função, nesse caso) que combina com sucesso todos os três princípios de design.

Você pode ler mais sobre Eaglesong [aqui] (https://medium.com/nervosnetwork/the-proof-of-work-function-of-nervos-ckb-3cc8364464d9). 


### 4.3 Modelo de Célula

A Nervos CKB utiliza o modelo de célula, uma nova construção que pode fornecer muitos dos benefícios do modelo de conta (utilizado no Ethereum), enquanto preserva a propriedade do ativo e as propriedades de verificação baseada em prova do modelo UTXO (utilizado no Bitcoin).

O modelo de célula é focado no estado. As células contêm dados arbitrários, que podem ser simples, como uma quantidade de token e um proprietário, ou mais complexos, como um código que especifica as condições de verificação para uma transferência de token. A máquina de estado da CKB executa scripts associados a células para garantir a integridade de uma transição de estado.

Além de armazenar dados próprios, as células podem fazer referência a dados em outras células. Isso permite que os ativos de propriedade do usuário e a lógica que os rege sejam separados. Isso contrasta com as plataformas de contratos inteligentes baseadas em contas, nas quais o estado é propriedade interna de um contrato e deve ser acessado por meio de interfaces de contratos inteligentes. Na Nervos CKB, as células são objetos de estado independentes que pertencem e podem ser referenciados e transmitidos diretamente. As células podem expressar verdadeiros "ativos suportáveis", pertencentes a seus proprietários (assim como UTXOs são ativos suportáveis ​​para proprietários de Bitcoin), enquanto fazem referência a uma célula que mantém a lógica que garante a integridade das transições de estado.

As transações do modelo de célula também são provas de transição de estado. As células de entrada de uma transação são removidas do conjunto de células ativas e as células de saída são adicionadas ao conjunto. As células ativas compreendem o estado global da Nervos CKB e são imutáveis: uma vez que as células foram criadas, não podem ser alteradas.

O modelo de célula é projetado para ser adaptável, sustentável e flexível. Ele pode ser descrito como um modelo UTXO generalizado e pode suportar tokens definidos pelo usuário, contratos inteligentes e diversos protocolos de camada 2.

Para uma compreensão mais profunda do modelo de célula, consulte [aqui] (https://medium.com/nervosnetwork/https-medium-com-nervosnetwork-cell-model-7323fca57571). 


### 4.4 Máquina Virtual

Enquanto muitos projetos de blockchain de próxima geração utilizam WebAssembly como a base de uma máquina virtual de blockchain, a Nervos CKB inclui a escolha de design exclusivo de uma máquina virtual (CKB-VM) com base no conjunto de instruções RISC-V.

RISC-V é uma arquitetura de conjunto de instruções RISC de código aberto que foi criada em 2010 para facilitar o desenvolvimento de novo hardware e software e é um conjunto de instruções livre de royalties, amplamente compreendido e amplamente auditado.

Encontramos inúmeras vantagens em usar RISC-V em um contexto de blockchain:

- Estabilidade: o conjunto de instruções básicas RISC-V foi finalizado e congelado, bem como amplamente implementado e testado. O conjunto de instruções RISC-V principal é fixo e nunca exigirá uma atualização.
- Aberto e com suporte: RISC-V é fornecido sob uma licença BSD e suportado por compiladores como GCC e LLVM, com implementações de linguagem Rust e Go em desenvolvimento. A Fundação RISC-V inclui mais de 235 organizações membros promovendo o desenvolvimento e suporte do conjunto de instruções.
- Simplicidade e extensibilidade: o conjunto de instruções RISC-V é simples. Com suporte para inteiros de 64 bits, o conjunto contém apenas 102 instruções. O RISC-V também fornece um mecanismo modular para conjuntos de instruções estendidas, permitindo a possibilidade de computação vetorial ou inteiros de 256 bits para algoritmos criptográficos de alto desempenho.
- Precificação precisa de recursos: o conjunto de instruções RISC-V pode ser executado em uma CPU física, fornecendo uma estimativa precisa dos ciclos da máquina necessários para executar cada instrução e informando a precificação de recursos da máquina virtual.

CKB-VM é uma máquina virtual RISC-V de baixo nível que permite computação flexível e completa de Turing. Por meio do uso do formato ELF amplamente implementado, os scripts CKB-VM podem ser desenvolvidos com qualquer linguagem que possa ser compilada para instruções RISC-V. 

#### 4.4.1 A CKB-VM e o Modelo de Célula

Uma vez implantadas, as blockchains públicas existentes são mais ou menos fixas. Atualizar elementos fundamentais, como primitivas criptográficas, envolve empreendimentos de vários anos ou simplesmente não é possível.

CKB-VM dá um passo para trás e move primitivas previamente construídas em VMs customizadas para células na parte superior da máquina virtual. Embora os scripts CKB sejam de nível mais baixo do que os contratos inteligentes no Ethereum, eles trazem o benefício significativo de flexibilidade, permitindo uma plataforma responsiva e uma base para a economia descentralizada em progresso.

As células podem armazenar código executável e fazer referência a outras células como dependências. Quase todos os algoritmos e estruturas de dados são implementados como scripts CKB armazenados nas células. Ao manter a VM o mais simples possível e descarregar o armazenamento do programa para as células, atualizar os algoritmos principais é tão simples quanto carregar o algoritmo em uma nova célula e atualizar as referências existentes. 

#### 4.4.2 Executando Outras Máquinas Virtuais na CKB-VM

Graças à natureza de baixo nível do CKB-VM e à disponibilidade de ferramentas na comunidade RISC-V, é fácil compilar outras VMs (como a EVM do Ethereum) diretamente na CKB-VM. Isso tem várias vantagens:

- Contratos inteligentes escritos em linguagens especializadas em execução em outras máquinas virtuais podem ser facilmente transferidos para execução no CKB-VM. (Estritamente falando, eles estariam executando em sua própria VM que foi compilada para ser executada dentro do CKB-VM.)
- O CKB pode verificar as transições de estado de resolução de disputas de transações da camada 2, mesmo se as regras das transições de estado forem gravadas para serem executadas em uma máquina virtual diferente de CKB-VM. Este é um dos principais requisitos para dar suporte a side-chains trustless de uso geral de camada 2.

Para obter um passo a passo técnico do CKB-VM, consulte [aqui] (https://medium.com/nervosnetwork/an-introduction-to-ckb-vm-9d95678a7757). 

### 4.5 Modelo Econômico

O token nativo da Nervos CKB é o "Byte de Conhecimento Comum", ou CKByte, para abreviar. CKBytes autorizam um detentor de token a ocupar parte do armazenamento de estado total do blockchain. Por exemplo, mantendo 1000 CKBytes, um usuário pode criar uma célula de 1000 bytes de capacidade ou várias células adicionando até 1000 bytes de capacidade.

Usar CKBytes para armazenar dados no CKB cria uma oportunidade de custo para os proprietários de CKBytes; eles não poderão depositar CKBytes ocupados no NervosDAO para receber uma parte da emissão secundária. Os CKBytes têm o preço de mercado e, portanto, um incentivo econômico é fornecido para que os usuários liberem voluntariamente o armazenamento de estado para atender à alta demanda do estado em expansão. Depois que um usuário libera o armazenamento de estado, ele receberá uma quantidade de CKBytes equivalente ao tamanho do estado (em bytes) que seus dados estavam ocupando.

O modelo econômico do CKB permite a emissão do token nativo para o crescimento do estado vinculado, mantendo uma baixa barreira de participação e garantindo a descentralização. À medida que os CKBytes se tornam um recurso escasso, eles podem ser avaliados e alocados com mais eficiência.

O bloco de gênese da Nervos Network conterá 33,6 bilhões de CKBytes, dos quais 8,4 bilhões serão "queimados" de imediato. A nova emissão de CKBytes inclui duas partes - emissão básica e emissão secundária. A emissão básica é limitada a um fornecimento total finito (33,6 bilhões de CKBytes), com um cronograma de emissão semelhante ao Bitcoin. O prêmio por bloco cai pela metade aproximadamente a cada 4 anos, até chegar a 0 novas emissões. Todas as emissões básicas são concedidas aos mineradores como incentivos para proteger a rede. A emissão secundária tem uma taxa de emissão constante de 1,344 bilhões de CKBytes por ano e foi projetada para impor uma oportunidade de custo para a ocupação do armazenamento do estado. Após o término da emissão base, haverá apenas a emissão secundária.

A Nervos CKB inclui um contrato inteligente especial denominado NervosDAO, que funciona como uma "proteção contra a inflação" sob os efeitos da emissão secundária. Os proprietários de CKByte podem depositar seus tokens no NervosDAO e receber uma parte da emissão secundária que compensa exatamente os efeitos inflacionários da emissão secundária. Para holders de longo prazo, contanto que eles travem seus tokens no NervosDAO, o efeito inflacionário da emissão secundária é apenas nominal. Com os efeitos da emissão secundária mitigados, esses usuários estão efetivamente mantendo tokens restritos como Bitcoin.

Enquanto os CKBytes estejam sendo usados ​​para armazenar o estado, eles não podem ser usados ​​para ganhar prêmios de emissão secundária por meio do NervosDAO. Isso torna a emissão secundária um imposto inflacionário constante, ou "aluguel de estado" sobre a ocupação do armazenamento de estado. Esse modelo econômico impõe taxas de estado de armazenamento proporcionais ao espaço e ao tempo de ocupação. É mais sustentável do que o modelo "pague uma vez, ocupe para sempre" usado por outras plataformas e é mais viável e fácil de usar do que outras soluções de aluguel do estado que exigem pagamentos explícitos.

Os mineradores são compensados ​​com recompensas por bloco e taxas de transação. Para recompensas por bloco, quando um minerador extrai um bloco, ele receberá a recompensa de emissão de base completa do bloco e uma parte da emissão secundária. A parcela é baseada na ocupação do estado, por exemplo: se metade de todos os tokens nativos estão sendo usados ​​para armazenar estado, um minerador receberia metade da recompensa da emissão secundária pelo bloco. Informações adicionais sobre a distribuição de emissão secundária estão incluídas na próxima seção (4.6). No longo prazo, quando a emissão básica for interrompida, os mineradores ainda receberão receita de "aluguel de estado" que é independente das transações, mas vinculada à adoção da Base de Conhecimento Comum da Nervos.

Em uma analogia, os CKBytes podem ser considerados como terrenos, enquanto os cripto-ativos armazenados no CKB podem ser considerados casas. O terreno é necessário para construir uma casa e os CKBytes para armazenar ativos no CKB. À medida que a demanda para armazenar ativos em CKB aumenta, a demanda por CKBytes também aumenta. Conforme o valor dos ativos armazenados aumenta, o valor dos CKBytes também aumenta.

A Nervos CKB foi projetado para transformar a demanda de uma grande variedade de ativos em demanda de um único ativo e usá-lo para compensar os mineradores por protegerem a rede.

Para obter uma explicação mais detalhada sobre o modelo econômico, consulte [aqui] (https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0015-ckb-cryptoeconomics/0015-ckb-cryptoeconomics.md). 


### 4.6 Tesouro

A parte da emissão secundária que não for para 1) mineradores ou 2) holders de longo prazo com tokens bloqueados no NervosDAO, irá para um fundo de tesouro. Para demonstrar: se 60% dos CKBytes emitidos forem usados ​​para armazenar o estado e 30% dos CKBytes forem depositados no NervosDAO, os mineradores receberão 60% da emissão secundária, o NervosDAO receberá 30% da emissão secundária, e 10% da emissão secundária irá para o tesouro.

O fundo do tesouro será usado para financiar a pesquisa e o desenvolvimento do protocolo em andamento, bem como para construir o ecossistema da Nervos Network. O uso dos fundos do tesouro será aberto, transparente e integrado para que todos possam ver. Comparado a um modelo de financiamento do tesouro baseado na inflação, este modelo não dilui os detentores de tokens de longo prazo (que depositaram seus tokens no NervosDAO). O financiamento do desenvolvimento do protocolo é derivado estritamente do custo de oportunidade para os detentores de tokens de curto prazo.

O tesouro não será ativado imediatamente após o lançamento da base de conhecimento comum da Nervos. Com a aprovação da comunidade, ele será ativado com um hard-fork posteriormente, somente após a Fundação Nervos ter esgotado o Fundo do Ecossistema, incluído no bloco Gênesis. Antes da ativação do tesouro, esta parte da emissão secundária será "queimada". 


### 4.7 Governança

Governança é como a sociedade ou grupos dentro dela se organizam para tomar decisões. Todas as partes relevantes com interesse no sistema devem ser envolvidas neste processo. Em relação a uma blockchain, isso deve incluir não apenas usuários, holders, mineradores, pesquisadores e desenvolvedores, mas também prestadores de serviços, como carteiras, bolsas e pools de mineração. Vários grupos de partes interessadas têm interesses diversos e é quase impossível alinhar os incentivos a todos. É por isso que a governança de blockchain é um tópico complicado e controverso. Se considerarmos um blockchain como um grande experimento social, a governança requer um design mais sofisticado do que qualquer outra parte do sistema. Após dez anos de evolução, ainda não identificamos as melhores práticas gerais ou processos sustentáveis ​​para governança de blockchain.

Alguns projetos conduzem a governança por meio de um "ditador benevolente pelo resto da vida" (como Linus Torvalds para o Linux). Reconhecemos que isso torna um projeto altamente eficiente, coeso e também encantador: as pessoas amam os heróis; no entanto, isso é contraditório com a descentralização, o valor central do blockchain.

Alguns projetos confiam a um distinto comitê off-chain um amplo poder de decisão, como o ECAF (EOSIO Core Arbitration Forum) sobre EOS. No entanto, esses comitês não possuem poder essencial para garantir que os participantes cumpram suas decisões, o que pode ter influenciado a decisão de encerrar o ECAF no início deste ano.

Alguns projetos, como o Tezos, vão além e implementam a governança on-chain para garantir que todos os participantes cumpram as decisões votadas. Isso também evita quaisquer impactos de discórdia entre desenvolvedores e mineradores (ou usuários de nó completo). Observe que a governança on-chain é diferente de uma votação on-chain simples, se um recurso ou patch proposto obteve votos suficientes por meio da governança on-chain, o código da cadeia será atualizado automaticamente, os mineradores ou nós completos não têm nenhum meio de controlar esta mudança. Polkadot adota uma abordagem ainda mais sofisticada para a governança on-chain, utilizando um conselho eleito, processo de referendo para votação ponderada por interesse e mecanismos de polarização positiva / negativa para contabilizar a participação eleitoral.

No entanto, apesar de sua simplicidade, a governança on-chain na prática não é tão elegante quanto é apresentada. Em primeiro lugar, os votos refletem apenas o interesse dos detentores de tokens, enquanto simplesmente ignoram todas as outras partes. Em segundo lugar, uma baixa taxa de votação é um problema antigo tanto no mundo do blockchain quanto no mundo real. Como os resultados podem ser do interesse da maioria se apenas uma minoria votar? Por último, mas o mais importante, um hard-fork sempre deve ser considerado como o recurso final para todas as partes interessadas. Dada a excelente disponibilidade de dados fornecida pela ampla replicação de uma blockchain permissionless, fazer um fork para fora da cadeia existente com preservação total dos dados e sem interrupção deve sempre ser uma opção. Um hard fork nunca poderia ser implementado por meio de governança on-chain.

Ainda não há respostas viáveis ​​para as questões de governança, portanto, para a Nervos Network, faremos uma abordagem em evolução. Esperamos que a comunidade se desenvolva organicamente nos primeiros dias e com o tempo, à medida que mais tokens são extraídos, a mineração se torne mais distribuída e mais desenvolvedores estejam envolvidos, as responsabilidades de governança gradualmente se tornarão mais descentralizadas. A longo prazo, a governança com base na comunidade gerenciará o processo de atualização do protocolo e a alocação de recursos do tesouro.

A Nervos CKB foi projetado para ser uma infraestrutura autônoma descentralizada que pode durar centenas de anos, o que significa que há certas coisas que exigem nosso melhor esforço como comunidade para se manterem verdadeiras, não importa como essa rede evolua. As 3 principais são:

- O cronograma de emissão é totalmente fixo, portanto, nunca deve sofrer alterações.
- O estado / dados armazenados nas células não devem ser violados.
- A semântica dos scripts existentes não deve ser alterada.

A governança baseada na comunidade para blockchains é um campo muito novo e há muitos experimentos em andamento que valem a pena. Reconhecemos que este não é um tópico trivial e é necessário tempo para estudar, observar e iterar totalmente para chegar a uma abordagem ideal. Estamos adotando uma abordagem conservadora em relação à governança com base na comunidade no curto prazo, embora permaneçamos totalmente comprometidos com essa direção no longo prazo. 


## 5. Visão Geral de Soluções de Camada 2

### 5.1 O que é a Camada 2?

A camada 1 de uma rede blockchain é definida por restrições. Uma blockchain ideal de camada 1 não compromete a segurança, descentralização e sustentabilidade, no entanto, isso cria desafios relacionados à escalabilidade e aos custos de transação. As soluções da camada 2 são construídas sobre os protocolos da camada 1, permitindo que a computação seja movida para fora da cadeia com mecanismos para se estabelecer com segurança na blockchain de camada 1.

Isso é semelhante à liquidação no sistema bancário atual ou aos registros regulatórios exigidos pela SEC. Ao reduzir a quantidade de dados que requerem consenso global, a rede pode servir a mais participantes e facilitar mais atividades econômicas do que seria possível da outra forma, enquanto ainda mantém as propriedades de descentralização.

Os usuários da camada 2 dependem da segurança fornecida pela blockchain da camada 1 e utilizam essa segurança ao mover ativos entre camadas ou resolver uma disputa. Essa função é semelhante a um sistema de tribunal: o tribunal não precisa monitorar e validar todas as transações, mas apenas serve como um local para registrar as principais evidências e para resolver disputas. Da mesma forma, em um contexto de blockchain, a blockchain da camada 1 permite que os participantes façam transações off-chain e, no caso de um desacordo, fornece a eles a capacidade de trazer evidências criptográficas para a blockchain e penalizar desonestidade. 

### 5.2 Canais de Pagamento e Estado

Os canais de pagamento são criados entre duas partes que realizam transações com frequência. Eles fornecem uma experiência de pagamento imediato de baixa latência que as transações feitas diretamente em uma blockchain global nunca poderiam fornecer. Os canais de pagamento funcionam de forma semelhante a uma conta de bar - você pode abrir uma conta com um garçom e continuar pedindo bebidas, mas só acertar a conta e pagar o valor final quando estiver pronto para sair do bar. Na operação de um canal de pagamento, os participantes trocam mensagens contendo compromissos criptográficos em seus saldos e podem atualizar esses saldos um número ilimitado de vezes fora da cadeia, antes de estarem prontos para fechar o canal e liquidar os saldos de volta na blockchain.

Os canais de pagamento podem ser unidirecionais ou bidirecionais. Os canais de pagamento unidirecionais fluem da Parte A para a Parte B, semelhante ao exemplo da conta de bar acima. A Parte A deposita o valor máximo que pode gastar com a Parte B e, em seguida, assina fundos lentamente à medida que recebe bens ou serviços.

Os canais de pagamento bidirecionais são mais complicados, mas começam a mostrar o escopo de possibilidades para as tecnologias de camada 2. Nesses canais de pagamento, os fundos fluem para frente e para trás entre as partes. Isso permite o "rebalanceamento" dos canais de pagamento e abre a possibilidade de pagamentos nos canais por meio de uma contraparte compartilhada. Isso permite redes de canais de pagamento, como a Bitcoin's Lightning Network. Os fundos podem ser transferidos da Parte A para a Parte B sem um canal direto entre elas, desde que a Parte A possa encontrar um caminho por meio de um intermediário com conexões abertas para ambas as partes.

Assim como os canais de pagamento podem escalar pagamentos em cadeia, os canais de estado podem escalar qualquer transação em rede. Enquanto um canal de pagamento se limita a gerenciar saldos entre duas partes, um canal de estado é um acordo sobre estado arbitrário, permitindo tudo, desde um jogo de xadrez trustless até aplicativos descentralizados escalonáveis.

Semelhante a um canal de pagamento, as partes abrem um canal, trocam assinaturas criptográficas ao longo do tempo e submetem um estado (ou resultado) final a um contrato inteligente na cadeia. O contrato será executado com base nessa entrada, liquidando a transação de acordo com as regras codificadas no contrato.
 
Um "canal de estado generalizado" é uma construção de canal de estado poderosa, permitindo que um canal de estado único suporte transições de estado em vários contratos inteligentes. Isso reduz o inchaço de estado inerente a uma arquitetura de "um canal por aplicativo" e também permite fácil integração com a capacidade de utilizar canais de estado que os usuários já tenham aberto. 

### 5.3 Side-chains (Cadeias Paralelas)

Uma side-chain é uma cadeia de blocos separada que é anexada a uma blockchain trustless (chain principal) com um pino de duas vias. Para utilizar a side-chain, um usuário enviaria fundos para um endereço especificado na cadeia principal, bloqueando esses fundos sob o controle dos operadores da side-chain. Uma vez que esta transação seja confirmada e um período de segurança tenha passado, uma prova pode ser comunicada aos operadores da side-chain detalhando o depósito de fundos. Os operadores irão então criar uma transação na side-chain, distribuindo os fundos apropriados. Esses fundos podem então ser gastos na side-chain com taxas baixas, confirmação rápida e alto rendimento.

A principal desvantagem das side-chains é que elas exigem mecanismos de segurança adicionais e outros pré-requisitos de segurança. A construção de side-chain mais simples, uma side-chain federada, confia em um grupo de operadores com várias assinaturas. Em plataformas de contratos inteligentes, os modelos de segurança podem ser ajustados com incentivos simbólicos ou jogos econômicos ligativos/desafiadores/redutores.

Em comparação com outras soluções off-chain de dimensionamento de propósito geral, as side-chains são mais fáceis de entender e implementar. Para tipos de aplicativos que permitem a criação de um modelo de confiança aceitável para seus usuários, as side-chains podem ser uma solução prática. 

### 5.4 Cadeias de Comprometimento

Em cadeias de comprometimento [6], como Plasma [7], uma cadeia de camada 2 que alavanca uma raiz de confiança em uma blockchain de camada 1 (cadeia raiz) com amplo consenso global é construída. Essas cadeias de comprometimento são seguras; No caso de um operador de cadeia ser malicioso ou disfuncional, os usuários sempre podem retirar seus ativos por meio de um mecanismo na cadeia raiz.

Um operador de cadeia de comprometimento é confiável para executar transações corretamente e publicar atualizações periódicas na cadeia raiz. Sob todas as condições, exceto para um ataque de censura prolongado na cadeia raiz, os ativos nas cadeias de comprometimento permanecerão seguros. Semelhante a cadeias laterais federadas, os designs de cadeias de comprometimento oferecem uma experiência de usuário superior em comparação com blockchains trustless. No entanto, eles fazem isso enquanto mantêm garantias de segurança mais fortes.
  
A cadeia de comprometimento é protegida por um conjunto de contratos inteligentes executados na cadeia raiz. Os usuários depositam ativos neste contrato e o operador da cadeia de comprometimento fornece a eles ativos na cadeia. O operador publicará periodicamente compromissos para a cadeia raiz, que os usuários podem utilizar posteriormente para provar a propriedade do ativo por meio de provas Merkle, uma "saída", na qual os ativos da cadeia de confirmação são retirados para a cadeia raiz.

Isso descreve a noção geral de projetos de cadeia de comprometimento, a base de uma família emergente de protocolos, incluindo o Plasma. O white paper do Plasma [7] lançado por Vitalik Buterin e Joseph Poon em 2017 apresenta uma visão ambiciosa. Embora todas as cadeias do Plasma sejam atualmente baseadas em ativos e só possam armazenar propriedade (e transferências) de tokens fungíveis e não fungíveis, a execução de código trustless (ou contratos inteligentes) é uma área ativa de pesquisa. 

### 5.5 Computações Off-Chain Verificáveis

A criptografia fornece uma ferramenta aparentemente feita sob medida para a dinâmica de verificação on-chain cara e computação off-chain barata: sistemas de prova interativa. Um sistema de prova interativo é um protocolo com dois participantes, o Provador e o Verificador. Ao enviar mensagens de um lado para outro, o Provador fornecerá informações para convencer o Verificador de que uma determinada afirmação é verdadeira, enquanto o Verificador examinará o que foi fornecido e rejeitará as afirmações falsas. As afirmações que o Verificador não pode rejeitar são aceitas como verdadeiras.

A principal razão pela qual o Verificador não simplesmente verifica a afirmação inocentemente por conta própria é a eficiência - ao interagir com um Provador, o Verificador pode verificar afirmações que seriam proibitivamente caras para verificar de outra forma. Essa lacuna de complexidade pode vir de uma variedade de fontes: 1) o Verificador pode estar executando um hardware leve que pode suportar apenas cálculos limitados por espaço ou tempo (ou ambos), 2) a verificação ingênua pode exigir acesso a uma longa sequência de dados não determinísticos 3) a verificação ingênua pode ser impossível porque o Verificador não possui certas informações secretas.

Embora o sigilo de informações importantes seja certamente um fator de restrição relevante no contexto de criptomoedas, um fator de restrição mais relevante no contexto de escalabilidade é o custo de verificação na cadeia, especialmente em contraste com a computação fora da cadeia relativamente barata.

No contexto de criptomoedas, atenção significativa tem sido direcionada para zk-SNARKs (conhecimento zero, argumentos de conhecimento não interativos sucintos). Esta família de sistemas de prova não interativos gira em torno do circuito aritmético, que codifica uma computação arbitrária como um circuito de adições e multiplicações sobre um campo finito. Por exemplo, o circuito aritmético pode codificar "Eu conheço uma folha nesta árvore Merkle".

As provas zk-SNARK são de tamanho constante (centenas de bytes) e verificáveis ​​em tempo constante, embora essa eficiência do Verificador tenha um custo: uma configuração confiável e uma string de referência estruturada são necessárias, além da aritmética baseada em emparelhamento (dos quais a dureza criptográfica do concreto permanece um objeto de preocupação).

Sistemas de prova alternativos oferecem diferentes compensações. Por exemplo, os Bulletproofs não possuem configuração confiável e contam com a suposição de logaritmo discreto muito mais comum; no entanto, têm provas de tamanho logarítmico (embora ainda muito pequenas) e verificadores de tempo linear. Os zk-STARKs fornecem uma alternativa aos zk-SNARKs em termos de escalabilidade, sem uma configuração confiável e contam apenas com suposições criptográficas sólidas, embora a prova produzida seja logarítmica em tamanho (e bastante grande: centenas de kilobytes).

No contexto de um ecossistema de criptomoeda multi-camadas, como a Rede Nervos, as provas interativas oferecem a capacidade de descarregar cálculos caros do lado do Provador para a camada 2, enquanto exigem apenas um trabalho modesto do lado do Verificador da camada 1. Essa intuição é capturada, por exemplo, no protocolo ZK Rollup de Vitalik Buterin [8]: um relayer sem permissão reúne transações fora da cadeia e atualiza periodicamente uma raiz Merkle armazenada na cadeia. Cada atualização de raiz é acompanhada por um zk-SNARK que mostra que apenas transações válidas foram acumuladas na nova árvore Merkle. Um contrato inteligente verifica a prova e permite que a raiz Merkle seja atualizada apenas se a prova for válida.

A construção descrita acima deve ser capaz de suportar transições de estado mais complexas, além de transações simples, incluindo DEXs, vários tokens e computação com preservação de privacidade.

### 5.6 Modelo Econômico de Soluções de Camada 2

Embora as soluções da camada 2 forneçam escalabilidade impressionante, a economia de token desses sistemas pode representar desafios de design.

A economia de tokens da camada 2 pode envolver compensação para infraestrutura crítica (como validadores e torres de vigilância), bem como design de incentivos específicos para aplicativos. A infraestrutura crítica da camada 2 tende a funcionar melhor com um modelo de assinatura baseado em duração. Na Rede Nervos, essa estrutura de preços pode ser facilmente implementada por meio do método de pagamento baseado em custo de oportunidade da CKB. Os provedores de serviços podem cobrar taxas sobre os "depósitos" de seus usuários por meio do NervosDAO. Os desenvolvedores da camada 2 podem então concentrar os modelos econômicos de token em incentivos específicos para seus aplicativos.

De certa forma, esse modelo de precificação é exatamente como os usuários pagam pelo armazenamento de estado na CKB também. Eles estão essencialmente pagando uma taxa de assinatura aos mineradores com a distribuição de suas recompensas inflacionárias emitidas pelo NervosDAO.

## 6. A Rede Nervos

### 6.1 Camada 1 como uma Plataforma de Armazenamento de Valor de Múltiplos Ativos

Acreditamos que uma blockchain de camada 1 deve ser construída como uma reserva de valor. Para maximizar a descentralização de longo prazo, ela deve ser baseada na prova de consenso de trabalho com um modelo econômico projetado em torno da ocupação do armazenamento do estado, em vez de taxas de transação. A Base de Conhecimento Comum (CKB) é uma prova de blockchain de armazenamento de valor baseado em trabalho, multi-ativo, com seus modelos de programação e econômicos projetados em torno do estado.

A CKB é a camada base da Rede Nervos, com a maior segurança e o maior grau de descentralização. Possuir e transacionar ativos na CKB vem com o custo mais alto, no entanto, fornece o armazenamento de ativos mais seguro e acessível na rede e permite o máximo de composibilidade. A CKB é mais adequado para ativos de alto valor e preservação de ativos de longo prazo.

A Base de Conhecimento Comum é a primeira blockchain da camada 1 construída especificamente para oferecer suporte aos protocolos da camada 2:

- A CKB foi projetada para complementar os protocolos da camada 2, com foco na segurança e descentralização, em vez de sobrepor as prioridades da camada 2, como escalabilidade.
- A CKB modela seu livro-razão em torno do estado, em vez de contas. As células são essencialmente objetos de estado autocontidos que podem ser referenciados por transações e passados ​​entre camadas. Isso é ideal para uma arquitetura em camadas, onde os objetos referenciados e passados ​​entre as camadas são peças de estado, em vez de contas.
- A CKB é projetado como uma máquina de verificação generalizada, em vez de um mecanismo de computação. Isso permite que a CKB atue como um tribunal criptográfico, que verifica as transições de estado fora da cadeia.
- A CKB permite que os desenvolvedores adicionem facilmente primitivos criptográficos personalizados. Isso torna a CKB à prova do futuro, permitindo a verificação das provas geradas por uma variedade de soluções da camada 2.

A Base de Conhecimento Comum visa ser a infraestrutura para armazenar o conhecimento comum mais valioso do mundo, com o melhor ecossistema da camada 2 fornecendo as transações de blockchain mais escalonáveis ​​e eficientes.

### 6.2 Escalabilidade com Soluções de Camada 2

Com sua arquitetura em camadas, a Rede Nervos pode escalar na camada 2 para qualquer número de participantes, enquanto ainda mantém as propriedades vitais de descentralização e preservação de ativos. Os protocolos da camada 2 podem fazer uso de qualquer tipo de compromisso da camada 1 ou primitivo criptográfico, permitindo grande flexibilidade e criatividade no projeto de sistemas transacionais para suportar uma base crescente de usuários da camada 2. Os desenvolvedores da camada 2 podem escolher suas próprias compensações em relação aos modelos de rendimento, finalidade, privacidade e confiança que funcionam melhor no contexto de seus aplicativos e usuários.

Na Rede Nervos, a camada 1 (CKB) é usada para verificação de estado, enquanto a camada 2 é responsável pela geração de estado. Canais de estado e side-chains são exemplos de geração de estado, no entanto, qualquer tipo de padrão generate-verify (gerar-verificar) é suportado, como um cluster de geração à prova de conhecimento zero. As carteiras também operam na camada 2, executando lógica arbitrária, gerando novo estado e submetendo transições de estado à CKB para validação. As carteiras da Rede Nervos são muito poderosas porque são geradoras de estado, com controle total sobre as transições de estado.

As side-chains são amigáveis ​​ao desenvolvedor e fornecem uma boa experiência do usuário. No entanto, elas contam com a honestidade de seus validadores. Se os validadores se comportarem de forma maliciosa, os usuários correm o risco de perder seus ativos. A Nervos Network fornece uma pilha de side-chain de código aberto e fácil de usar para lançar side-chains no CKB, consistindo em uma estrutura de blockchain Proof-of-Stake chamada "Muta" e uma solução de side-chain baseada nela chamada "Axônio".

Muta é uma estrutura de blockchain de alto desempenho altamente personalizável, projetada para oferecer suporte a Proof-of-Stake, consenso BFT e contratos inteligentes. Ele apresenta um consenso BFT de alto rendimento e baixa latência "Overlord" e suporta várias máquinas virtuais, incluindo CKB-VM, EVM e WASM. Diferentes máquinas virtuais podem ser usadas em um único blockchain Muta simultaneamente, com interoperabilidade entre VMs. A Muta reduz bastante a barreira para os desenvolvedores criarem blockchains de alto desempenho, ao mesmo tempo que permite flexibilidade máxima para personalizar seus protocolos.

Axon é uma solução completa construída com Muta para fornecer aos desenvolvedores uma side-chain pronta para uso no topo do Nervos CKB, com uma segurança prática e um modelo econômico simbólico. As soluções Axon usam o CKB para proteger a custódia de ativos e usam o mecanismo de governança baseado em token para gerenciar os validadores de side-chain. Protocolos cross-chain para interações entre uma side-chain da Axon e a CKB, bem como entre as side-chains do Axon, também serão integrados. Com a Axon, os desenvolvedores podem se concentrar na construção de aplicativos, em vez de construir infraestrutura e protocolos de cross-chain (cadeia cruzada).

Ambas Muta e Axon estão atualmente em forte desenvolvimento. Abriremos o código das estruturas em breve, e RFCs para Muta e Axon também estão a caminho.

Os protocolos da camada 2 são uma área fértil de pesquisa e desenvolvimento. Prevemos um futuro no qual todos os protocolos da camada 2 serão padronizados e interoperarão perfeitamente. No entanto, reconhecemos que as soluções de camada 2 ainda estão amadurecendo e, muitas vezes, ainda estamos ultrapassando os limites do que elas podem fazer, bem como encontrando suas compensações aceitáveis. Vimos soluções promissoras no início, mas ainda há muitas pesquisas a serem conduzidas em assuntos como interoperabilidade, segurança e modelos econômicos em projetos da camada 2.

### 6.3 Sustentabilidade

No interesse da sustentabilidade de longo prazo, o estado de limites da Base de Conhecimento Comum da Nervos, impõe um custo ao armazenamento em cadeia e fornece incentivos para que os usuários limpem seu armazenamento de estado. Um estado limitado mantém baixos os requisitos para a participação total do nó, garantindo que os nós possam ser executados em hardware de baixo custo. A participação robusta de todos os nós aumenta a descentralização e, por sua vez, a segurança.

Ao impor um custo de "aluguel estatal" proporcional ao tempo sobre o armazenamento estatal, a Nervos Common Knowledge Base atenua a tragédia dos bens comuns enfrentada por muitas blockchains em um paradigma "pague uma vez, armazene para sempre". Implementado por meio de "inflação planejada", esse mecanismo de aluguel de estado fornece uma experiência de usuário tranquila, ao mesmo tempo que impõe um custo ao armazenamento de estado.

Esse custo de inflação pode ser direcionado porque os usuários são donos do espaço de consenso que seus dados ocupam. Este modelo também inclui um mecanismo nativo para que os usuários removam seu estado do espaço de consenso. Juntamente com os incentivos econômicos do aluguel do estado, isso garante que o tamanho do estado sempre estará se movendo em direção à quantidade mínima de dados exigida pelos participantes da rede.

O estado de propriedade individual também reduz significativamente os custos dos desenvolvedores. Em vez de serem obrigados a comprar CKBytes para os requisitos de estado de todos os seus usuários, os desenvolvedores só precisam comprar CKBytes suficientes para armazenar o código de verificação exigido por seu aplicativo. Cada usuário usaria suas próprias células para armazenar seus tokens e seria totalmente responsável por seus ativos.

Finalmente, o aluguel do estado oferece uma recompensa contínua aos mineradores por meio da emissão de novos tokens. Essa receita previsível incentiva os mineradores a avançar a blockchain, em vez de bifurcar blocos lucrativos para receber as taxas de transação.

### 6.4 Incentivos Alinhados

O modelo econômico da Base de Conhecimento Comum é projetado para alinhar incentivos para todos os participantes do ecossistema.

A Base de Conhecimento Comum da Nervos é construída explicitamente para preservação de valor seguro, em vez de taxas de transação baratas. Esse posicionamento crítico atrairá usuários de armazenamento de valor, semelhantes à comunidade de usuários do Bitcoin, em vez de usuários de meio de troca.

Os casos de uso de meio de troca têm uma tendência a sempre empurrar uma rede de blockchain em direção à centralização, em busca de maior eficiência e taxas baixas. Sem uma receita de taxas significativa para os operadores de infraestrutura que protegem a rede (mineradores ou validadores), a segurança deve ser financiada por meio da inflação monetária ou é simplesmente insuficiente. A inflação monetária é prejudicial para os detentores de longo prazo e a segurança subfinanciada é prejudicial para qualquer participante da rede.

Os usuários de armazenamento de valor, no entanto, têm fortes demandas por resistência à censura e segurança de ativos. Eles contam com os mineradores para fornecer isso e, por sua vez, compensá-los por seu papel. Em uma rede de armazenamento de valor, essas partes têm interesses alinhados.

Ao alinhar os incentivos de todos os participantes, uma comunidade Nervos unida pode crescer, e o sistema econômico alinhado da rede também deve ser resistente ao hard-fork.

### 6.5 Captação de Valor e Geração de Valor

Para que qualquer blockchain permaneça seguro conforme o valor dos ativos garantidos pela plataforma aumenta, o sistema deve ter um mecanismo para capturar valor conforme o valor dos ativos protegidos aumenta. Ao limitar o estado, o CKB torna o espaço do estado um recurso escasso e com preço de mercado. Conforme a demanda por armazenamento de ativos na rede aumenta, o sistema deve compensar melhor os mineradores pela proteção de tais ativos.

Como plataforma de preservação de valor, o valor intrínseco da CKB como plataforma é determinado pela quantidade de segurança que ele fornece aos ativos que preserva. Conforme o valor dos ativos garantidos aumenta, o mecanismo de captura de valor do modelo econômico CKB é capaz de aumentar automaticamente o orçamento de segurança do CKB para atrair mais recursos de mineração, tornando a plataforma mais segura. Isso não é apenas importante para tornar a plataforma sustentável, mas também fornece um caminho de crescimento para o valor intrínseco da plataforma - à medida que a plataforma se torna mais segura, também se torna mais atrativa para ativos de maior valor, gerando mais demanda. Obviamente, isso é limitado pelo valor agregado geral que eventualmente se moverá para o espaço do blockchain.

Com o tempo, esperamos que a densidade econômica da CKB aumente. Os CKBytes serão usados ​​para armazenamento de ativos de alto valor e os ativos de baixo valor serão movidos para blockchains conectadas ao CKB, como side-chains de camada 2. Em vez de proteger ativos diretamente, a CKB pode ser usado como uma raiz de confiança para proteger o ecossistema de uma side-chain inteira por meio de, por exemplo, algumas centenas de bytes de provas criptográficas. A densidade econômica de tais provas é extraordinariamente alta, sustentando ainda mais a curva de demanda de espaço de armazenamento: análogo a uma pequena parcela de terra que aumenta significativamente sua densidade econômica ao apoiar um arranha-céu.

Finalmente, por meio do projeto do NervosDAO e sua função de "proteção contra a inflação", os detentores de tokens de longo prazo sempre reterão uma porcentagem fixa da emissão total, tornando o token nativo em si uma reserva robusta de valor.

### 6.6 Preenchendo a Lacuna Regulatória

As blockchains sem permissão permitem a descentralização total na emissão e transação de ativos. Isso é o que as torna valiosos, mas também é a razão pela qual não são compatíveis com os sistemas financeiros e judiciais do mundo real.

O surgimento de uma arquitetura em camadas oferece a oportunidade de criar partes em conformidade com as regulamentações de uma blockchain não regulamentada e sem permissão. Por exemplo, os usuários podem armazenar seus ativos descentralizados na camada 1, desfrutar da propriedade absoluta desses ativos e também podem processar negócios do mundo real na camada 2, onde estão sujeitos a restrições regulamentares e legais.

Tomemos por exemplo as bolsas de criptomoedas - países como o Japão e Cingapura emitiram licenças para as bolsas e criaram requisitos regulatórios. Uma bolsa em conformidade ou uma filial de uma bolsa global poderia construir uma cadeia comercial de camada 2, importar identidades e ativos de usuários e, então, conduzir negócios legais de acordo com os requisitos regulatórios locais.

A emissão e transação de ativos do mundo real tornam-se possíveis dentro de uma construção de blockchain em camadas. Ativos do mundo real podem fluir para o ecossistema do blockchain por meio de uma side-chain regulada de camada 2 para a blockchain sem permissão de camada 1, permitindo que esses ativos acessem o maior ecossistema de serviços financeiros descentralizados e combináveis.

No futuro, espera-se que a Rede Nervos também use side-chains e aplicativos de camada 2 como base para a adoção em larga escala pelo usuário, em cooperação com empresas líderes neste espaço.

# Referências

[1] Satoshi Nakamoto. "Bitcoin: A Peer-to-Peer Electronic Cash System". 31 Oct 2008, https://bitcoin.org/bitcoin.pdf

[2] Vitalik Buterin. "Ethereum White Paper: A Next Generation Smart Contract & Decentralized Application Platform". Nov 2013 http://blockchainlab.com/pdf/Ethereum_white_paper-a_next_generation_smart_contract_and_decentralized_application_platform-vitalik-buterin.pdf 

[3] With an average Bitcoin transaction size of 250 bytes:
(2 * 250 * 7,500,000,000) / (24 * 6) = 26,041,666,666 byte blocks (every 10 minutes); 
26,041,666,666 * (24 * 6) = 3,750,000,000,000 bytes (blockchain growth each day);
3,750,000,000,000 * 365.25 = 1,369,687,500,000,000 bytes (blockchain growth each year)

[4] Gur Huberman, Jacob Leshno, Ciamac C. Moallemi. "Monopoly Without a Monopolist: An Economic Analysis of the Bitcoin Payment System". Bank of Finland Research Discussion Paper No. 27/2017. 6 Sep 2017, https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3032375

[5] Miles Carlsten, Harry Kalodner, S. Matthew Weinberg, Arvind Narayanan. "On the Instabiliity of Bitcoin Without the Block Reward". Oct 2016, https://www.cs.princeton.edu/~smattw/CKWN-CCS16.pdf

[6] Lewis Gudgeon, Perdo Moreno-Sanchez, Stefanie Roos, Patrick McCorry, Arthur Gervais. "SoK: Off The Chain Transactions". 17 Apr 2019, https://eprint.iacr.org/2019/360.pdf

[7] Joseph Poon, Vitalik Buterin. "Plasma: Scalable Autonomous Smart Contracts". 11 Aug 2017, https://plasma.io/plasma.pdf

[8] Vitalik Buterin. "On-chain scaling to potentially ~500 tx/sec through mass tx validation". 22 Sep 2018, https://ethresear.ch/t/on-chain-scaling-to-potentially-500-tx-sec-through-mass-tx-validation/3477
