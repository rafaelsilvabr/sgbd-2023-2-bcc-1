## Resultado

Clique [AQUI](../media/sgbd-2023-2-bcc-resumo.pdf) para ver as notas.

#### Avaliação em 19/10/2023
1. <ins>**FIFO**:</ins> busca escolher para substituição a página que chegou primeiro ao _buffer pool_; checa o _dirty bit_, para determinar se a página para substituição deve ser gravada em disco. <ins>**_Clock policy_**:</ins> checa o _dirty bit_, para determinar se a página para substituição deve ser gravada em disco. <ins>**LRU**:</ins> checa o _dirty bit_, para determinar se a página para substituição deve ser gravada em disco.
2. <ins>**_Clock policy_**:</ins> ao girar o ponteiro, se o ponteiro passar por um _buffer_ com sinalizador igual a 1 (um), o valor 0 (zero) será atribuído ao sinalizador (e o ponteiro passa para o próximo _buffer_); uma página é substituída de seu _buffer_ somente se não for acessada até que o ponteiro complete uma rotação.
3. <ins>**Princípio da localidade temporal**:</ins> o princípio pode ser aplicado para dados ou instruções acessados recentemente.
4. <ins>**Prevenir a fragmentação dentro do bloco**:</ins> a operação de exclusão pode ocasionar a movimentação dos registros existentes no bloco; a operação de alteração pode ocasionar a movimentação dos registros existentes no bloco.
