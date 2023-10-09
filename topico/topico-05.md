## [Tópico 05] - Estruturas de armazenamento (3/N)
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

### <ins>CONTEÚDO</ins>

|_Item do conteúdo_|_Item do conteúdo_|
|-|-|
|1. Visão geral|8. Cabeçalho do arquivo|
|2. Armazenamento físico|9. Alocação de blocos de arquivo no disco|
|3. Arquivo, bloco e registro|10. Acesso a registros|
|4. _Buffering_ de blocos|11. Armazenamento do dicionário de dados|
|5. Registro de tamanho fixo|12. Organização de arquivos _heap_|
|6. Registro de tamanho variável|13. Organização de arquivos sequenciais|
|7. <ins>**ORGANIZAÇÃO DE REGISTROS EM BLOCOS**</ins><br>**(espalhada e não espalhada)**|14. Organização de arquivos _hashing_|

<hr style="border:2px solid blue">

### 7. <ins>ORGANIZAÇÃO DE REGISTROS EM BLOCOS</ins>

<ins>Bloco de dados</ins>:
- <ins>Unidade de alocação</ins> de dados em arquivos.
- <ins>Unidade de transferência</ins> de dados entre a memória secundária e a memória principal.

Usualmente, o tamanho do bloco é maior do que o tamanho do registro:
- Cada bloco possivelmente conterá vários registros.

Seja um arquivo, cujos <ins>registros têm tamanho fixo</ins>:
- <ins>tamanho do bloco</ins> igual a **B bytes**;
- <ins>tamanho do registro</ins> igual a **R bytes**;
- B ≥ R.

O <ins>fator de bloco</ins> determina o número de registros por bloco:
- **bfr = ⎣B/R⎦** registros por bloco;
  - onde: ⎣(x)⎦ representa a função _floor_ (retorna o maior valor inteiro menor ou igual a **x**).
- em geral, **B** não possui divisão exata em **R**:
  - então, algum espaço não utilizado em cada bloco pode ser desperdiçado:
    - **Desperdício = B − (bfr * R)** bytes.

#### <ins>ORGANIZAÇÃO NÃO ESPALHADA</ins>

A <ins>organização não espalhada</ins> [de registros em blocos] pressupõe que algum <ins>desperdício ocorre em cada bloco</ins>:
- Há espaço não utilizado (desperdício) em arquivos de <ins>registros de tamanho fixo</ins>.
- Há espaço não utilizado (desperdício) em arquivos de <ins>registros de tamanho variável</ins> (ver figura abaixo).

<img src="../media/arquivo-4.jpg" width="500">

#### <ins>ORGANIZAÇÃO ESPALHADA</ins>

A <ins>organização espalhada</ins> busca evitar o desperdício de espaço em cada bloco:
- Para lidar com o espaço não utilizado em cada bloco, um registro é dividido em <ins>duas frações</ins> (ver figura abaixo):
  - as frações do registro são <ins>alocadas em dois blocos</ins> do arquivo;
  - um ponteiro no final do primeiro bloco aponta para o bloco que contém o restante do registro.
- Se o tamanho do registro for superior ao tamanho do bloco (R > B):
  - a organização espalhada deve ser aplicada.

<img src="../media/arquivo-5.jpg" width="500">

Para arquivos com <ins>registros de tamanho variável</ins>:
- Pode ser usada uma organização espalhada ou não espalhada.
  - se o tamanho médio do registro médio for grande, é vantajoso usar a organização espalhada, para reduzir o espaço perdido em cada bloco.
- Cada bloco pode armazenar um <ins>número diferente de registros</ins>.
- O <ins>fator de bloco</ins> (bfr) é o <ins>número médio de registros</ins> por bloco do arquivo.
- O <ins>número de blocos</ins> **b** necessários para ter um arquivo com **r** registros:
  - **b = ⎡(r/bfr)⎤ blocos**
    - onde ⎡(x)⎤ representa a função _ceiling_ (retorna o menor valor inteiro maior ou igual a **x**).

#### <ins>REGISTROS DE TAMANHO VARIÁVEL: ALOCAÇÃO INTERNA AO BLOCO</ins>

O cabeçalho do bloco contém (ver figura abaixo):
- <ins>Número de entradas</ins> de registro.
- <ins>Posição do final</ins> de espaço livre no bloco.
- <ins>Posição inicial</ins> e <ins>tamanho</ins> de cada registro.

Os registros podem ser <ins>movimentados dentro de uma página</ins> para mantê-los contíguos:
- A cada movimentação, a(s) entrada(s) no cabeçalho precisa(m) ser atualizada(s).

Qualquer <ins>ponteiro externo ao bloco</ins>:
- Não deve apontar diretamente para qualquer dos registros no bloco.
- Deve <ins>apontar para o cabeçalho do bloco</ins>. 

<img src="../media/arquivo-6.jpg" width="600">

<hr style="border:2px solid blue">

### Exercício

Um arquivo de clientes tem r = 20.000 registros de comprimento fixo. Cada registro tem os seguintes campos: Nome (30 bytes), CPF (9 bytes), Endereço (40 bytes), Telefone (9 bytes), Data de nascimento (8 bytes), Sexo (1 byte), Estado civil (4 bytes), Cor ou raça (4 bytes), Categoria de cliente (4 bytes, integer), e Grau de escolaridade (3 bytes). Um byte adicional é utilizado como um marcador de exclusão. O arquivo é armazenado no disco com tamanho de bloco B = 512 bytes.<br>
(a) Calcule o tamanho de registro **R** (incluindo o marcador de exclusão) em bytes.<br>
(b) Calcule o fator de bloco **bf**r e o número de blocos de arquivo **b**, considerando uma organização não espalhada.<br>
(c) Calcule o <ins>número médio de blocos acessados</ins> necessários para pesquisar um <ins>registro arbitrário</ins> no arquivo, usando <ins>busca linear</ins>.<br>
(d) Suponha que o arquivo é <ins>ordenado pelo CPF</ins>, calcular o tempo que leva para procurar um registro a partir do valor de CPF, por meio de <ins>busca binária</ins>.

(a) Calcule o tamanho de registro R (incluindo o marcador de exclusão) em bytes.<br>
R = (30 + 9 + 40 + 9 + 8 + 1 + 4 + 4 + 4 + 3) + 1 = 113 bytes

(b) Calcule o fator de bloco bfr e o número de blocos de arquivo b, considerando uma organização não espalhada.<br>
bfr = ⎣B / R⎦ = ⎣512 / 113⎦ = 4 registros por bloco<br>
b = ⎡r / bfr⎤ = ⎡20000 / 4⎤ = 5000 blocos

(c) Calcule o número médio de blocos acessados necessários para pesquisar um registro arbitrário no arquivo, usando busca linear.<br>
i. procurar no campo de chave:<br>
se o registro for encontrado, a metade dos blocos de arquivos são pesquisados em média: b / 2 = 5000/2 blocos<br>
se o registro não for encontrado, todos os blocos de arquivos são pesquisados: B = 5000 blocos<br>
ii. pesquisar no campo não-chave:<br>
todos os blocos de arquivos devem ser pesquisados: B = 5000 blocos

(d) Suponha que o arquivo é ordenado pelo Cpf, calcular o número de blocos acessados a partir do seu valor Cpf, fazendo uma busca binária.<br>
⎡log2 b⎤ = ⎡log2 5000⎤

<hr style="border:2px solid blue">
