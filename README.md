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

A extração automática de conhecimento é uma área das ciências da computação voltada para o Processamento de Linguagem Natural (NLP). Ela visa auxiliar os pesquisadores a extrair informações de grandes quantidades de textos não estruturados e, em última análise, identificar relações entre conjuntos de assuntos. Essa monografia buscou implementar e avaliar um *pipeline* de: 1) identificação e recuperação de artigos por palavras-chave a partir da base de dados [PubMed](https://pubmed.ncbi.nlm.nih.gov/); 2) extração de texto não estruturado; 3) pré-processamento e limpeza de dados não relevantes, e; 4) análise semântica. A análise semântica busca encontrar o significado do texto a partir da estrutura gramatical e das relações entre as palavras individuais de cada frase em contextos específicos. Como estudo de caso, analisamos os artigos publicados sobre o "fator de crescimento e diferenciação 11" (GDF11; do inglês, *growth and differentiation factor 11*). Essa molécula faz parte da superfamília de TGF-β (do inglês, *transforming growth factor*), que foi descrita inicialmente em 1999. Em seguida, McPherron et al. (1999) relataram que camundongos mutantes para Gdf11 exibem transformações homeóticas anteriores, com costelas extras e membros posteriores deslocados caudalmente. Nos anos seguintes, no entanto, tal ligante caiu em relativo esquecimento, talvez porque os animais mutantes morrem após o nascimento devido a defeitos renais. Foi somente no ano de 2013, com a descrição de que os níveis de Gdf11 diminuem com a idade e que sua administração pode reverter fenótipos associados ao envelhecimento em camundongos, que o interesse por esse ligante ganhou força e refloresceu. Desde então, inúmeros estudos abrangem aspectos distintos das suas atividades, envolvendo desde a biologia do desenvolvimento até o câncer.


### 2. Modelagem

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

### 3. Resultados

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

### 4. Conclusões

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin pulvinar nisl vestibulum tortor fringilla, eget imperdiet neque condimentum. Proin vitae augue in nulla vehicula porttitor sit amet quis sapien. Nam rutrum mollis ligula, et semper justo maximus accumsan. Integer scelerisque egestas arcu, ac laoreet odio aliquet at. Sed sed bibendum dolor. Vestibulum commodo sodales erat, ut placerat nulla vulputate eu. In hac habitasse platea dictumst. Cras interdum bibendum sapien a vehicula.

Proin feugiat nulla sem. Phasellus consequat tellus a ex aliquet, quis convallis turpis blandit. Quisque auctor condimentum justo vitae pulvinar. Donec in dictum purus. Vivamus vitae aliquam ligula, at suscipit ipsum. Quisque in dolor auctor tortor facilisis maximus. Donec dapibus leo sed tincidunt aliquam.

---

Matrícula: 192.110.216

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
