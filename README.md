# freguesias-portugal-2013-pre-pos
Repositório com a associação entre nomes de antigas e novas freguesias depois da reorganização administrativa de 2013.

# Conteúdos:
- [freguesias_pre_pos_2013.csv](./freguesias_pre_pos_2013.csv) - *dataset* com os nomes de freguesias antes de 2013 e os nomes das freguesias a que deram origem com a reforma administrativa da Lei n.º 11-A/2013 de 28 de janeiro [(.pdf)](./source/lei_11-a_2013.pdf) [(Diário da República Electrónico - dre.pt)](https://diariodarepublica.pt/dr/detalhe/lei/11-a-2013-373798), bem como o tipo de alteração ocorrida.

| campo                   | descrição                                                   |  valores possíveis                                                                                       |
|-------------------------|-------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| `municipio`             |  Municipio a que pertence a freguesia                       | *texto livre*                                                                                        |
| `pre_2013`                | Nome existente antes de 2013, corrigido do DRE              |  *texto livre*                                                                                       |
| `pos_2013`                | Nome em 2013, da freguesia correspondente, corrigido do DRE |    *texto livre*                                                                                     |
| `sede`                    |   Localização da sede da freguesia em 2013, não corrigido           |    *texto livre*                                                                                     |
| `tipo_alteracao` |    Transformação ocorrida em 2013                          | `sem alteração`,<br>`criada por alteração de limites territoriais`,<br>`criada por agregação` |

- [lei_11-a_2013.csv](./lei_11-a_2013.csv) - *dataset* aproximadamente correspondente ao anexo da Lei n.º 11-A/2013 de 28 de janeiro [(.pdf)](./source/lei_11-a_2013.pdf) [(dre.pt)](https://diariodarepublica.pt/dr/detalhe/lei/11-a-2013-373798), segundo a estrutura original.

| campo        | descricao                                                           |                                                                                                                                                                               |
|--------------|---------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `municipio`  |  Municipio a que pertence a freguesia                               | *texto livre*                                                                                                                                                                   |
| `antiga`     | Nome existente antes de 2013, corrigido do DRE                      | *texto livre*, vazio (se a freguesia não tiver sido agregada)                                                                                                                   |
| `nova`       | Nome, em 2013, da união de freguesias, corrigido do DRE             | *texto livre*, vazio (se a freguesia não resultar de agregação)                                                                                                                 |
| `territorio` | Nome, em 2013, da freguesia com limites alterados, corrigido do DRE | *texto livre*, `Nenhuma` se a freguesia não tiver tido alteração de limites                                                                                                     |
| `total`      | Nome, em 2013, da freguesia resultante                              | *texto livre* (igual a `nova` se a freguesia resultar de agregação,<br>igual a `territorio` se resultar de alteração de limites,<br>única instância do nome novo nos restantes casos) |
| `sede`       | Localização da sede da freguesia em 2013, não corrigido             | *texto livre*                                                                                                                                                                   |

# Metodologia
1. Conversão do anexo [(.pdf)](./source/lei_11-a_2013.pdf) para .html com Adobe Acrobat
2. *Scraping* das tabelas com `pandas` para [lei_11-a_2013.csv](./lei_11-a_2013.csv)
    * Verificações:
    * correcção de erros detectados no scraping, feita à mão e comparada com o .pdf original
    * existência de freguesia final: todas as freguesias em `total` correspondem a uma freguesia existente em 2013, que pode entretanto ter mudado de nome (cf. Wikipedia, DRE, *site* das juntas de freguesia)
    * consistência de nomes: `nova` ou `territorio` e `total` não podem ser diferentes
    * rastreio de omissão de espaços: para cada nome de freguesia em `total`, as freguesias correspondentes não têm palavras maiores que a freguesia resultante (excepções de nomes não resultantes de agregação excluídas manualmente)
3. Transformação das colunas `antiga`, `nova` e `territorio` na coluna `tipo_alteração`, consoante o seu preenchimento ou não. Preenchimento da coluna `antiga` para freguesias cujos nome ou limites territoriais não mudaram

# Aviso
Não tendo verificado todos os campos manualmente, pode haver erros derivados da extracção ou manipulação dos dado. Por favor, submete um `pull-request` com as correcções que encontrares.