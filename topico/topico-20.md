## [Tópico 19] - Estruturas de indexação (8/)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

### <ins>CONTEÚDO</ins>

|_Item do conteúdo_|_Item do conteúdo_|
|-|-|
|1. Visão geral|4. Índice secundário|
|2. Índice primário|5. Índice multinível|
|3. Índice de agrupamento|6. <ins>**ÍNDICE EM ÁRVORE (2/3)**</ins>|

<hr style="border:2px solid blue">

### 6. <ins>ÍNDICE EM ÁRVORE (2/3)</ins>

**`Árvores de Busca`** impõem retrições às estruturas tradicionais em árvore, tal como explanado no [Tópico 19](./topico-19.md):<br>
&#10004; Por exemplo, ver a estrutura interna de cada nó da árvore de busca.

Contudo, <ins>novas restrições são necessárias</ins> para lidar com os desafios apresentados no [Tópico 19](./topico-19.md), a saber:<br>
&#9888; Minimizar o número de níveis na árvore (balanceamento!?).<br>
&#9888; Minimizar a necessidade de reestruturação (reorganização) da árvore.

### &#x270D;&#x270D;&#x270D; `ÍNDICE EM ÁRVORE` &#8212; <ins>`ÁRVORE B`</ins>

A **`Árvore B`** possui restrições adicionais às árvores de busca, visando a lidar com ambos os desafios acima:<br>
&#9745; Os algoritmos de inserção e exclusão, porém, tornam-se mais complexos.<br>
&#9745; A maioria das inserções e exclusões são processos simples, mas<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; se tornam complicados apenas em circunstâncias especiais:,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ..... uma inserção em um nó que já está cheio, ou<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ..... uma exclusão de um nó que o deixa menos da metade cheio.

&#9752;&#9752; Seja uma **`ÁRVORE B DE ORDEM p`**:

&#9918; Cada **<ins>NÓ INTERNO</ins>** possui a **`ESTRUTURA`**:

|P|Dados|P|Dados| ... | ... |P|Dados|P|
|-|-|-|-|-|-|-|-|-|
|P<sub>1</sub>|_&#8249;&#8249; K<sub>1</sub> , Pr<sub>1</sub> &#8250;&#8250;_|P<sub>2</sub>|_&#8249;&#8249; K<sub>2</sub> , Pr<sub>2</sub> &#8250;&#8250;_| ... | ... |P<sub>q-1</sub>|_&#8249;&#8249; K<sub>q-1</sub> , Pr<sub>q-1</sub> &#8250;&#8250;_|P<sub>q</sub>|

Onde:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Como  árvore de ordem **p**, então **q &#8924; p**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cada **P<sub>i</sub>** é um ponteiro de árvore:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;... um ponteiro para outro nó na árvore B.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cada **Pr<sub>i</sub>** é um ponteiro de dados:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;... um ponteiro de dados (ponteiro de registro ou de bloco !?) cujo valor do <ins>campo-chave de pesquisa</ins> é igual a **K<sub>i</sub>**.

&#9918; Cada **<ins>NÓ INTERNO</ins>** possui as **`RESTRIÇÕES`**:

&#9745; K<sub>1</sub> < K<sub>2</sub> < … < K<sub>q−1</sub>.<br>
&#9745; &#8704; X : X &#8712; { valores do campo-chave de pesquisa na subárvore apontada por P<sub>i</sub> }<br>

|Cenário|Restrição|
|-|-|
|i = 1|X < K<sub>i</sub>|
|1 < i < q|K<sub>i−1</sub> < X < K<sub>i</sub>|
|i = q|K<sub>i−1</sub> < X|

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ... para i = 1: X < K<sub>i</sub><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ... para 1 < i < q: K<sub>i−1</sub> < X < K<sub>i</sub><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ... para i = q: K<sub>i−1</sub> < X

&#9745; Cada nó possui <ins>no máximo **p** ponteiros</ins> de árvore.<br>
&#9745; Cada nó, exceto os nós raiz e folhas, possui pelo menos ⎡(p/2)⎤ ponteiros de árvore:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ... o nó raiz possui pelo menos dois ponteiros de árvore, a menos que seja o único nó na árvore.<br>
&#9745; Um nó com **q** ponteiros de árvore (**q &#8924; p**) possui **q &#8212; 1** valores de campo-chave de pesquisa:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ...  e, portanto, possui **q &#8212; 1** ponteiros de dados.<br>
&#9745; Todos os nós folha estão no mesmo nível.<br>
&#9745; Os nós folhas têm a mesma estrutura dos nós internos.<br>
&#9745; Em nós folhas, todos os ponteiros de árvore P<sub>i</sub> possuem valor NULO.

A estrutura e algumas das restrições da `Árvore B` (acima mencinadas) estão evidentes na figura abaixo:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="../media/arquivo-43.jpg" width="540">

Um exemplo de uma `Árvore B de ordem 3` é exibido abaixo:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="../media/arquivo-44.jpg" width="500">

Com respeito a `Árvore B` acima:<br>
&#9752; Os valores de pesquisa K na árvore são únicos?<br>
&#9752; Se usarmos uma Árvore B em um campo não-chave:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ... os ponteiros de arquivo Pr<sub>i</sub> apontariam para um bloco &#8212; ou um cluster de blocos ...<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; onde o bloco apontado conteria ponteiros para os registros que compartilham os mesmos valores de pesquisa ? <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; o nível extra de indireção é semelhante à <ins>Opção 3</ins>, discutida no [Tópico 17](./topico-17.md).

&#9752;&#x270D;&#9745;&#9888;
