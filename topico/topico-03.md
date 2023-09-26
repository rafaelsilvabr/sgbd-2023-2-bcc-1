## [Tópico 03] - Estruturas de armazenamento
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

### CONTEÚDO

|_Item do conteúdo_|_Item do conteúdo_|
|-|-|
|<ins>1. Visão geral<ins>|9. _Buffering_ de blocos|
|<ins>2. Armazenamento físico<ins>|10. Alocação de blocos de arquivo no disco|
|<ins>3. Arquivo, bloco e registro<ins>|11. Acesso a registros|
|4. Registro de tamanho fixo|12. Armazenamento do dicionário de dados|
|5. Registro de tamanho variável|13. Organização de arquivos _heap_|
|6. Objetos grandes|14. Organização de arquivos sequenciais|
|7. Organização de registros em blocos<br>(espalhada e não espalhada)|15. Organização de arquivos _hashing_|
|8. Cabeçalho do arquivo||

<hr style="border:2px solid blue">

### 1. Visão Geral

A cada 'necessidade de processamento':
- a CPU acessa os dados em memória principal (volátil);
- <ins>aplicações de banco de dados</ins> utilizam uma pequena parte do banco de dados.

Dados persistentes _vs._ dados transitórios:
- bancos de dados são maiores que a memória principal (caso usual);
- bancos de dados residem em armazenamento não volátil.

Sempre que uma determinada parte dos dados for necessária:
- localizar os dados na memória secundária;
- copiar os dados para a memória principal;
- processar os dados;
- regravar (salvar) os dados na memória secundária, se for o caso.

> A maioria dos bancos de dados usa "arquivos do sistema operacional" como uma camada intermediária para armazenar registros.

<hr style="border:2px solid blue">

### 2. Armazenamento Físico

As mídias de armazenamento são classificadas por: 
- <ins>velocidade</ins> com que os dados podem ser acessados;
- <ins>custo por unidade de dados</ins> para selecionar/adquirir a mídia;
- <ins>confiabilidade da mídia</ins>.

Conceitos para ter atenção:
- armazenamento volátil/não volátil; armazenamento online de longo prazo; armazenamento primário/secundário/terciário; armazenamento de acesso sequencial/acesso direto.

#### <ins>HIERARQUIA DE MEMÓRIA</ins>:
- organização em níveis de memória;
- cada nível contêm uma cópia do código e dados mais usados em cada instante; 
- quanto mais "perto" a memória se encontra do processador, mais rápido será o acesso aos dados (maior o custo por byte, menor a capacidade de armazenamento).

<img src="../media/memoria-hierarquia-1.jpg" width="500"><img src="../media/memoria-hierarquia-2.jpg" width="400">

Nas figuras acima:
- os dados no armazenamento secundário ou terciário não podem ser processados diretamente pela CPU;
- embora a _memória flash_ seja classificada na figura como 'memória principal', porque a leitura de dados pode ser tão rápida quanto a leitura da memória principal, ela requer que um bloco inteiro seja apagado e gravado de uma só vez;
- há várias camadas de cache (L1, L2, L3, etc.), cada com capacidades e velocidades diferentes;
- a cache busca armazenar cópias dos dados e instruções frequentemente usados, reduzindo o tempo de acesso à memória em comparação com a RAM.

#### PRINCÍPIO DA LOCALIDADE

A hierarquia de memória objetiva que:
- a grande maioria dos dados/instruções estejam disponíveis nos níveis de memória mais altos (cache, memória principal), e pouquíssimos acessos sejam necessários à memória secundária.
- a maioria dos programas apresenta características de localidade.

```diff
- O que é o PRINCÍPIO DA LOCALIDADE ?
```

<ins>Localidade Temporal</ins>: 
- os dados (ou instruções) acessados recentemente tendem a ser acessados novamente em um futuro próximo;
- a cache mantém cópias de dados recentemente usados para o acesso rápido.

<ins>Localidade Espacial</ins>:
- os dados próximos a um endereço de memória tendem a ser acessados conjuntamente;
- a cache e a RAM carregam <ins>blocos</ins> de dados adjacentes, para reduzir o tempo de acesso subsequente.

#### <ins>PARA REFLETIR:<br>As respostas às questões abaixo estão corretas?</ins>

<ins>[Questão 01]</ins> Que características um programa deve ter para que o uso de memória cache seja muito vantajoso?
- O programa deve ter trechos pequenos que sejam executados várias vezes, e os dados devem estar localizados próximos uns dos outros OU dados e instruções devem ter localidade espacial (próximos uns dos outros) e localidade temporal (serem usados várias vezes em um certo instante de tempo).

<ins>[Questão 02]</ins>Se registradores do processador e a memória _cache_ operassem com os mesmos tempos de acesso, ainda haveria vantagem em se utilizar a memória _cache_? E se a memória _cache_ e a memória principal operassem com os mesmos tempos de acesso, ainda haveria vantagem em se utilizar a memória _cache_? 
- se os registradores do processador e a memória _cache_ e processador , ainda assim seria vantajoso utilizar _cache_, porque o seu objetivo é justamente fornecer dados e instruções na velocidade do processador;
- se a memória principal e a memória _cache_ operassem com os mesmos tempos, não haveria mais razão para se usar a memória _cache_;
- obviamente, aséctos de custo e de confiabilidade devem ser considerados nessas respostas.

<ins>[Questão 03]</ins>Ao medir o desempenho de um certo sistema, verificou-se que este passava muito tempo com a CPU ociosa e tinha um alto volume de acessos a disco. Assinale a alternativa que apresenta a solução traduzida na melhoria de desempenho desse sistema:<br>
a) Troca da CPU por uma mais rápida<br>
b) Aumento na capacidade de memória do sistema<br>
c) Aumento na capacidade de armazenamento do disco<br>
d) Uso de memória cache<br>
e) Troca do sistema operacional

<hr style="border:2px solid blue">

### 3. Arquivo, Bloco e Registro

Um <ins>banco de dados</ins> é mapeado em <ins>um número arquivos diferentes</ins>, que geralmente são mantidos pelo sistema operacional subjacente:
- assuma a existência de um sistema de arquivos subjacente.

Um <ins>arquivo</ins> é organizado <ins>logicamente</ins> como uma <ins>sequência de registros</ins>:
- cada <ins>registro</ins> consiste em uma <ins>coleção</ins> de valores ou itens de <ins>dados relacionados</ins>;
- cada <ins>valor</ins> é formado por <ins>um ou mais bytes</ins> e corresponde a um determinado <ins>campo do registro</ins>;
- os <ins>valores dos dados</ins> podem ser <ins>interpretados como fatos</ins> sobre entidades, seus atributos e seus relacionamentos.

#### BLOCO

Um arquivo também é <ins>particionado logicamente</ins> em unidades de armazenamento de comprimento fixo chamadas <ins>**blocos**</ins>:
- um <ins>bloco</ins> é a <ins>unidade de alocação</ins> de _**armazenamento**_ e _**transferência**_ de dados;
- a maioria dos bancos de dados usa <ins>tamanhos de bloco de 4 a 8 kilobytes</ins> por padrão, mas eles podem ser especificados quando uma instância de banco de dados é criada (por exemplo, múltiplos de 512 bytes).

Um bloco tipicamente <ins>contém vários registros</ins>:
- o conjunto exato de registros que um bloco contém depende da forma de organização física dos dados utilizada;
- assume-se que nenhum registro é maior que um bloco: é sempre verdade?
- quais itens de dados podem ser significativamente maiores que um bloco?

Atenção: rever a 'visão geral' (acima).
