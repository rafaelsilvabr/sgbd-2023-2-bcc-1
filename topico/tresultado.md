## Resultado

Clique [AQUI](../media/sgbd-2023-2-bcc-resumo.pdf) para ver as notas.

#### Avaliação em 19/10/2023
1. <ins>**FIFO**:</ins> busca escolher para substituição a página que chegou primeiro ao _buffer pool_; checa o _dirty bit_, para determinar se a página para substituição deve ser gravada em disco. <ins>**_Clock policy_**:</ins> checa o _dirty bit_, para determinar se a página para substituição deve ser gravada em disco. <ins>**LRU**:</ins> checa o _dirty bit_, para determinar se a página para substituição deve ser gravada em disco.
2. <ins>**_Clock policy_**:</ins> ao girar o ponteiro, se o ponteiro passar por um _buffer_ com sinalizador igual a 1 (um), o valor 0 (zero) será atribuído ao sinalizador (e o ponteiro passa para o próximo _buffer_); uma página é substituída de seu _buffer_ somente se não for acessada até que o ponteiro complete uma rotação.
3. <ins>**Princípio da localidade temporal**:</ins> o princípio pode ser aplicado para dados ou instruções acessados recentemente.
4. <ins>**Prevenir a fragmentação dentro do bloco**:</ins> a operação de exclusão pode ocasionar a movimentação dos registros existentes no bloco; a operação de alteração pode ocasionar a movimentação dos registros existentes no bloco.

#### Avaliação em 09/11/2023

1(a): R = (40+1) + (9+1) + (0,4 * (39+1)) + (9+1) + (0,5*(7+1)) + (1+1) + (0,25*(3+1)) + (3+1) + 1 = 89 bytes<br>
1(b): Função Piso &#8213; bfr = ⎣ B / R ⎦ = ⎣ 1024 / 89 ⎦ =  ⎣ 11.5 ⎦ = 11 registros por bloco<br>
1(c): Função Teto &#8213; b = ⎡ (r * R) / bfr ⎤ = ⎡ 40000 * 89 / 1024 ⎤ = ⎡ 3476,6 ⎤ = 3477 blocos<br>
2. Aplicar uma marcação lógica para os registros excluídos. Ter uma lista de registros excluídos.<br>
3. É necessário conhecer os endereços dos blocos do arquivo. O desempenho é melhor que busca linear.<br>
4. O bucket A1A1A1A1 estiver cheio. d = d', para o bucket A1A1A1A1.

#### Avaliação em 16/11/2023

1(a): bfr = ⎣(B/R)⎦ = ⎣(4096/100)⎦ = 40 registros por bloco.<br>
1(b): b = ⎡(r/bfr)⎤ = ⎡(300000/40)⎤ = 7500 blocos.<br>
1(c): c = ⎡log<sub>2</sub> b⎤= ⎡(log<sub>2</sub> 7500)⎤ = 13 blocos.<br>
2(a). R<sub>i</sub> = (9 + 6) = 15 bytes.<br>
2(b). bfr<sub>i</sub> = ⎣(B/R<sub>i</sub>)⎦ = ⎣(4096/15)⎦ = 273 entradas por bloco.<br>
2(c). r<sub>i</sub> = 7500 registros (mesmo número de blocos no arquivo de dados).<br>
2(d). b<sub>i</sub> = ⎡(r<sub>i</sub>/bfr<sub>i</sub>)⎤ = ⎡(7500/273)⎤ = 28 blocos.<br>
2(e). c<sub>i</sub> = ⎡(log<sub>2</sub> b<sub>i</sub>)⎤ = ⎡(log<sub>2</sub> 28)⎤ = 5 blocos acessados.<br>
2(d). c<sub>dados</sub> = c<sub>i</sub> + 1 = 5 + 1 = 6 blocos acessados.<br>

#### Avaliação em 30/11/2023

1. Sobre o acesso à memória: Se os registradores do processador e a memória cache têm o mesmo tempo de acesso, ainda assim seria vantajoso utilizar cache, porque o seu objetivo é justamente fornecer dados e instruções na velocidade do processador. Se a memória principal e a memória cache operassem com os mesmos tempos, não haveria mais razão para se usar a memória cache.
1. Ainda sobre o acesso à memória: A cache busca armazenar cópias dos dados e instruções frequentemente usados, reduzindo o tempo de acesso à memória em comparação com a RAM. Um buffer é uma área reservada contígua na memória principal (armazenamento primário), usualmente para armazenar um bloco.
1. A transferência de dados por grupos de bytes (bloco/página): Promove a diminuição de custo de transferência.
1. Seja uma matriz N x N .... : O princípio da localidade espacial.
1. Um bloco de dados é: Unidade de transferência de dados.
1. O gerenciamento de buffer busca: nenhum.
1. Quando uma determinada página é solicitada ... : Escolhe uma página para substituição, usando uma política de substituição de buffer.
1. Continuando a questão anterior ... : Copia a página solicitada no buffer liberado da página para substituição.
1. Seja um bloco em memória (página) ... : Clock policy. LRU.
1. Em registros de tamanho variável ... : Ponteiros externos ao bloco não devem apontar fisicamente para qualquer dos registros no bloco. Algumas operações sobre os registros podem ocasionar a movimentação interna de registros na página, para evitar a fragmentação de espaço não utilizado.
1. Sobre organização de arquivos e método de acesso: _nenhum_. 
1. Em arquivos do tipo _heap_, estudamos ... : Mover vários registros do arquivo. Mover um único registro do arquivo.   
1. Baseado nos passos abaixo, determine ... : Checa se os dados estão em memória principal. Checa se os dados estão em memória principal. Copia os dados da memória secundária para a memória principal. Processa os dados. Regrava (salva) os dados na memória secundária, se for o caso.
1. Considere os seguintes tipos de índice ...<br>
O campo de indexação é o campo de ordenação. (primário, agrupamento)<br>
O campo de indexação é um campo chave. (primário, secundário)<br>
O campo de indexação é um campo não-chave. (agrupamento, secundário)<br>
Vários índices desse tipo podem ser criados para um mesmo arquivo de dados, mesmo que seus campos de indexação sejam distintos entre si. (secundário)<br>
O índice é esparso. (primário, agrupamento, secundário)<br>
O índice é denso. (secundário)<br>
A busca binária pode ser aplicada na pesquisa via o índice. (primário, agrupamento, secundário)<br>
O arquivo de índice é um arquivo ordenado ou não ordenado. ()<br>
Pode ser transformado em índice multinível. (primário, agrupamento, secundário)
1. 
|n|M (qtde. inicial de buckets)|Valor da chave|h0 ou  h1 ?|Valor da função hash|
|-|-|-|-|-|
|4|7|122|h1|10|
|2|7|11|h0|4|
|3|13|19|h0|6|
|2|13|37|h0|11|

#### Avaliação em 07/12/2023

a. Ri = 10 + 6 = 16 bytes.
a. bfri = piso(B / R) = piso(1024 / 16) = piso(64) = 64 registros por bloco.
