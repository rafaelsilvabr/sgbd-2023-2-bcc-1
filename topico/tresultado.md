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


