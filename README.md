# Extração automática de conhecimento a partir de artigos científicos <!--Título do Trabalho-->

#### Aluno: [Arnon Dias Jurberg](https://github.com/ajurberg), PhD
#### Orientador: [Leonardo Alfredo Forero Mendoza](https://github.com/leofome8), PhD


---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- Códigos:
    - [01_fetch_pubmed_gdf11.ipynb](01_fetch_pubmed_gdf11.ipynb)
    - [02_exploratory_analysis_gdf11.ipynb](02_exploratory_analysis_gdf11.ipynb)
    - [03_download_pdfs_scidown_gdf11.ipynb](03_download_pdfs_scidown_gdf11.ipynb)
    - [04_text_extraction_gdf11_v2.ipynb](04_text_extraction_gdf11_v2.ipynb)
    - [05_text_cleaning_gdf11.ipynb](05_text_cleaning_gdf11.ipynb)
    - [06_gensim_word2vec_gdf11_v2.ipynb](06_gensim_word2vec_gdf11_v2.ipynb)
    - [07_doc2vec_gensim_gdf11_v2.ipynb](07_doc2vec_gensim_gdf11_v2.ipynb)
    - [08_LDA_wordcloud_gdf11.ipynb](08_LDA_wordcloud_gdf11.ipynb)

- Produção acadêmica relacionada:
    - [The Role of Embryonic Chick Muscle Cell Culture in the Study of Skeletal Myogenesis](https://www.frontiersin.org/articles/10.3389/fphys.2021.668600/full).
    - [Do medicine and cell biology talk to each other? A study of vocabulary similarities between fields](https://www.scielo.br/j/bjmbr/a/zWC4JBdSfB3NYvTwgR5BHNR/?lang=en).
    - A GDF11-centric glimpse in health and disease through natural language processing (*manuscrito em preparação*)
    - Neuroendocrine Control of Macrophage Development and Function: A Natural Language Processing Approach (*manuscrito em preparação*)

---

### Dedicatória

À minha mãe, *in memoriam*, com muito amor e admiração. Obrigado por tudo.

---

### Resumo

A consistente expansão do conhecimento científico ao longo dos anos é facilmente identificada a partir da observação do número crescente de artigos científicos revisados por pares disponibilizados em bases de dados especializadas, como o [PubMed](https://pubmed.ncbi.nlm.nih.gov/) e o [Dimensions](Dimensions.ai). Tais estimados avanços, contudo, provocam alguns desafios inusitados, como manter-se constantemente atualizado apesar da profusão de novas publicações. Essa monografia buscou implementar e avaliar um *pipeline* de extração automática de conhecimento para a obtenção de conhecimento estruturado a partir da extração, organização e síntese de dados não estruturados, como aqueles encontrados nos textos científicos. Tal abordagem pode auxiliar a identificar padrões e relações antes desconhecidas entre genes, tipos de células, processos fisiológicos, doenças e/ou medicamentos, de modo a auxiliar no desenvolvimento de novas estratégias de intervenção terapêutica e no progresso de conhecimento científico adicional.


### Abstract

The consistent expansion of scientific knowledge over the years is easily identified from the observation of the increasing number of peer-reviewed scientific articles available in specialized databases, such as [PubMed](https://pubmed.ncbi.nlm.nih.gov/) and [Dimensions](Dimensions.ai). Such esteemed advances, however, cause some unusual challenges, such as keeping constantly updated despite the profusion of new publications. This work sought to implement and evaluate a *pipeline* of automatic knowledge extraction to obtain structured knowledge from the extraction, organization and synthesis of unstructured data, such as those found in scientific texts. Such an approach can help to identify previously unknown patterns and relationships between genes, cell types, physiological processes, diseases and/or drugs in order to assist in the development of new therapeutic intervention strategies and in the progress of additional scientific knowledge.


### 1. Introdução

O conhecimento científico reúne evidências empíricas verificáveis obtidas a partir da observação sistemática e controlada de um determinado evento por meio da experimentação ou realização de pesquisas de campo. Tal atividade permite, portanto, a compreensão de eventos físicos, químicos e biológicos que nos cercam, além de propiciar o desenvolvimento de inovações que visam mitigar ameaças, como doenças e catástrofes climáticas, e aumentar a qualidade de vida da sociedade. Tais conhecimentos têm permitido que a civilização humana se adapte aos diversos ambientes e cresça ao redor do mundo ao longo dos anos. A busca constante por melhorias parece ser inerente ao comportamente humano, o que reflete no extenso acúmulo de informações em documentos de texto (impressos ou disponíveis *online*), como os artigos científicos revisados por pares. Os artigos científicos relatam os detalhes e achados experimentais de uma determinada investigação, de modo a contribuirem para o crescimento de uma área de pesquisa e permitirem que outros grupos de pesquisa possam reproduzir os experimentos realizados. Com isso, a literatura científica têm crescido praticamente exponencialmente ao longo das últimas décadas e, embora indiscutivelmente valiosa, a prática de registro tem provocado um desafio inusitado para os pesqusiadores: manter-se constantemente atualizado apesar da profusão de novas publicações.

A extração automática de conhecimento é uma área das ciências da computação voltada para o Processamento de Linguagem Natural (NLP). Ela visa auxiliar os pesquisadores a extrair informações de grandes quantidades de textos não estruturados e, em última análise, identificar relações entre conjuntos de assuntos. Essa monografia buscou implementar e avaliar um *pipeline* de: 1) identificação e recuperação de artigos por palavras-chave a partir da base de dados [PubMed](https://pubmed.ncbi.nlm.nih.gov/); 2) extração de texto não estruturado; 3) pré-processamento e limpeza de dados não relevantes, e; 4) análise semântica. A análise semântica busca encontrar o significado do texto a partir da estrutura gramatical e das relações entre as palavras individuais de cada frase em contextos específicos. Como estudo de caso, analisamos os artigos publicados sobre o "fator de crescimento e diferenciação 11" (GDF11; do inglês, *growth and differentiation factor 11*). Essa molécula faz parte da superfamília de TGF-β (do inglês, *transforming growth factor*), que foi descrita inicialmente em 1999. Em seguida, [McPherron et al. (1999)](https://www.nature.com/articles/ng0799_260) relataram que camundongos mutantes para Gdf11 exibem transformações homeóticas anteriores, com costelas extras e membros posteriores deslocados caudalmente. Nos anos seguintes, no entanto, tal ligante caiu em relativo esquecimento, talvez porque os animais mutantes morrem após o nascimento devido a defeitos renais. Foi somente no ano de 2013, com a descrição de que os níveis de Gdf11 diminuem com a idade e que sua administração pode reverter fenótipos associados ao envelhecimento em camundongos, que o interesse por esse ligante ganhou força e refloresceu. Desde então, inúmeros estudos abrangem aspectos distintos das suas atividades, envolvendo desde a biologia do desenvolvimento até o câncer.


### 2. Modelagem

Todas as etapas aqui descritas foram realizadas usando Python 3.7.6 no Google Colaboratory. Para a recuperação dos artigos científicos relacionados ao tema, foi realizada uma consulta da base de dados [PubMed](https://pubmed.ncbi.nlm.nih.gov/) no dia 03 de janeiro de 2022 usando a biblioteca [metapub](https://github.com/metapub) com os descritores: (gdf11 OR bmp11 OR gdf-11 OR bmp-11). Tal busca retornou 436 identificadores únicos do PubMed (PMID; do inglês *PubMed Unique Identifier*), que foram então utilizados para recuperar os seguintes parâmetros: a) título do artigo; b) autores; c) título do periódico; d) ano de publicação; e) volume; f) número; g) texto do resumo; h) identificador de objeto digital (DOI; do inglês, *Digital Object Identifier*), e; i) URL (ou localizador uniforme de recursos), quando disponível. Nessa etapa, não foram utilizados critérios de exclusão, exceto a remoção de entradas duplicatas quando pertinente. A integridade do conjunto de dados foi avaliada pela geração de matrizes de nulidade usando a biblioteca [missingno](https://github.com/ResidentMario/missingno). Em seguida, uma análise exploratória dos dados permitiu avaliarmos a evolução do campo de GDF11 ao longo dos anos (i.e. análise bibliométrica). Nela, examinamos o número de artigos publicados por ano, o número de artigos publicados por periódico e o número de artigos publicados por autor. Os gráficos foram construídos a partir das bibliotecas [seaborn](https://seaborn.pydata.org/) e [matplotlib](https://matplotlib.org/). Ainda, fizemos a limpeza e o pré-processamento dos textos (incluindo a remoção de *stop words* com a biblioteca [nltk](https://www.nltk.org/) e lematização com a biblioteca [stanza](https://stanfordnlp.github.io/stanza/)) obtidos a partir dos títulos dos trabalhos e de seus resumos para avaliar a frequência relativa das palavras por meio de *word clouds*. Por fim, a análise semântica dos vocabulários foi realizada nos textos completos dos artigos científicos. Os textos foram extraídos dos arquivos PDF com a biblioteca [Tika Apache](https://tika.apache.org/), pré-processados e então treinados em conjunto com a biblioteca [Gensim](https://radimrehurek.com/gensim/) utilizando os algoritmos [Word2Vec](https://radimrehurek.com/gensim/models/word2vec.html) e [Doc2Vec](https://radimrehurek.com/gensim/models/doc2vec.html) e vetores de dimensão 300. Para comparação, nós também utilizamos os vetores pré-treinados do repositório público da [BioASQ](http://bioasq.org/news/bioasq-releases-continuous-space-word-vectors-obtained-applying-word2vec-pubmed-abstracts). As representações vetorias geradas contras as palavras mais semelhantes ou as palavras mais diferentes foram visualizadas utilizando o algoritmo de redução de dimensionalidade [t-SNE](https://scikit-learn.org/0.15/modules/generated/sklearn.manifold.TSNE.html).


### 3. Resultados

#### 3.1. *Análise Exploratória*

O conjunto de dados de artigos GDF11 recuperados do PubMed retornou 436 entradas, dos quais 433 entradas eram únicas (99.31%). A visualização de sua matriz de nulidade revelou que a completude do conjunto de dados era predominantemente alta, exceto pelos parâmetros 'número' e 'url'. Os parâmetros 'volume', 'resumo' e 'doi' exibiram cerca de 97.93%, 94.49% e 98.16% de completude, respectivamente. Essas características não foram consideradas para análises posteriores. O primeiro artigo sobre GDF11 foi publicado em 1999 por [Nakashima et al. 1999](https://linkinghub.elsevier.com/retrieve/pii/S0925477398002056). Inicialmente, o interesse pelo GDF11 foi apenas marginal, com média de 6.53 artigos publicados por ano até 2013. Contudo, após a publicação do trabalho de [Loffredo et al. (2013)](https://linkinghub.elsevier.com/retrieve/pii/S0092-8674(13)00456-X) o número de publicações começou a crescer e atingiu uma média de 37.22 artigos publicados por ano. Os 433 artigos sobre GDF11 disponíveis no PubMed até o momento foram publicados por 263 periódicos diferentes. Entre eles, o periódico *Developmental Biology* publicou 9 artigos e os periódicos *Circulation Research* e *Scientific Reports* publicaram 8 artigos cada um. Nós ainda identificamos que havia 353 primeiros autores únicos neste conjunto de dados, o que significa que 80.96% dos artigos do GDF11 foram publicados por pesquisadores distintos como primeiro autor. Entre os primeiros autores mais prolíficos, Luc Rochette contribuiu com cinco artigos de revisão sobre GDF11 no período examinado. Já entre os últimos autores, foi identificado 333 autores únicos (76.38%), com David J. Glass tendo sido o mais prolífico com oito artigos até o momento.

#### 3.2. *Word clouds*

Nós geramos *word clouds* para representar as palavras mais frequentes nos títulos e nos resumos dos artigos publicados sobre GDF11. Esse tipo de visualização destaca as palavras mais frequentes, que são representadas com um tamanho de fonte maior enquanto as palavras menos frequentes são representadas com fontes cada vez menores. Após a remoção dos termos "gdf11", "growth differentiation factor 11" e "factor", os termos mais frequentes foram as palavras "*muscle*", "*cell*", "*mouse*" e "*myostatin*". De fato, a *word cloud* revelou os temas e modelos experimentais mais comuns abordados no campo. Em particular, a proteína miostatina/GDF8 exibe alta (>90%) similaridade de sequência com o GDF11 e ambas são expressas na musculatura. Já os termos "*cell*" e "*mouse*" representam os modelos experimentais mais utilizados no estudo desses dois ligantes. Outras palavras menos frequentes revelam também que GDF11 tem efeito pleiotrópico, i.e. exerce inúmeras funções diferentes em tipos celulares e tecidos distintos. 


#### 3.3. *Análise semântica*

A análise textual semântica visa examinar a similaridade entre dois textos ou trechos de texto. Para tal, nós utilizamos três abordagens diferentes: [Word2Vec](https://radimrehurek.com/gensim/models/word2vec.html) e [Doc2Vec](https://radimrehurek.com/gensim/models/doc2vec.html) para a criação de representações vetoriais numéricas (ou *word/document embeddings*) de palavras e parágrafos/documentos, respectivamente. Esses *embeddings* referem aos pesos dos neurônios da camada densa
(i.e. pesos dos neurônios)
e [Alocação Latente de Dirichlet (LDA)](https://github.com/bmabey/pyLDAvis) para visualização interativa do modelo de tópico e termos mais frequentes.


Word2Vec é um modelo para formar/criar Word Embeddings. É uma maneira moderna de representar Word , em que cada palavra é representada por um vetor (Array de números com base no Embedding Size). Os vetores nada mais são do que os pesos dos neurônios, portanto, se definirmos os neurônios de tamanho 100, teremos 100 pesos e esses pesos são nossos Word Embeddings ou simplesmente vetor Dense.



### 4. Conclusões

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

---

Matrícula: 192.110.216

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
