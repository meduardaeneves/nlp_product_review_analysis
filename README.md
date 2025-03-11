# Atividade de NLP
- Aluna: Maria Eduarda Esteves Neves

# Links de acesso:
- [Link para visualização do vídeo, acesso ao Google Colab e apresentação PPT.](https://drive.google.com/drive/folders/1kaWydw6VNOdP61T2FkxFCTQPrtVABoTN?usp=sharing)

# Objetivo e análise do banco de dados: 
- Utilizar Fazer a análise de NLP para um determinado produto, neste caso o Tablet Mirage 7 Pol 64gb 4gb ram Quad core wi-fi cor Preto;
- O conjunto de dados foi obtido através da API do mercado livre. Id do produto: MLB4241052396
- [Link para visualização do produto no site do Mercado Livre](https://www.mercadolivre.com.br/tablet-mirage-7-pol-64gb-4gb-ram-quad-core-wi-fi-cor-preto-2022/p/MLB28331783#polycard_client=search-nordic&wid=MLB4241052396&sid=search&searchVariation=MLB28331783&position=17&search_layout=grid&type=product&tracking_id=7d53f2f5-5c5f-47f6-b9aa-03c90ab38c71)
- O banco de dados é formato por 1255 linhas, contendo duas colunas "content", referente ao conteúdo da avaliação do usuário e "rate" referente à nota data pelo usuário ao produto (1 - 5)
## Tabela de Quantitativo por Rate
| Rate           | Count |
|----------------|-------|
| 5              | 595   | 
| 1              | 234   |
| 4              | 181   |
| 3              | 160   |
| 2              | 85    |

- Tabela com quantitativos desbalanceados, contendo mais elementos nas extremidades (rate = 1 e rate = 5)

# Metodologia:
## Processamento de Texto
 - Normalização 
    - Utilizando Spacy
    - Transformação de maiúsculas e minúsculas
    - Remoção de caracteres especiais (pontuação)
    - Remoção de algumas StopWords (‘a’, ‘o’, ‘de’, ‘e’)
      - Se encontravam entre os termos de maior frequência 
  - Tokenização
    - Utilizando NLTK Tokenizer
  - Lematização
    - Foi feito um teste, fazendo a lematização das frases, entretanto o resultado encontrado não foi satisfatório, optou-se então por permanecer sem esta etapa do processamento

- Quantitativo pós processamento:
    - Maior vetor de palavras: 108
    - Total de Tokens: 14552
    - Tamanho do vocabulário: 1832
  
## SVM + BOW
   - É uma representação baseada em pesos
   - Não guarda a ordem dos elementos
   - O dataframe va ter em quantidade de colunas, o tamanho do vocabulário extraído
   - Modelo utilizado:
     - SVC: Support Vector Classifier
   - Parâmetros utilizados com o auxílio do GridSearch:
     - C: 100; gamma: 0.1; kernel: rbf
     - Conjunto de teste representou 20%

## SVM + EMBEDDINGS
   - É uma representação distribuída das palavras
   - Cada unidade linguística vai ser representada por um vetor denso
     - Tamanho do vetor escolhido: 75
   - Utilizado o Word2Vec para gerar os vetores das palavras
   - Foi feito uma padronização das frases, com zero padding, para que todas tivessem a mesma dimensão que aquela de maior dimensão (108)
   - Modelo utilizado:
     - SVC: Support Vector Classifier
   - Parâmetros utilizados com o auxílio do GridSearch:
     - C: 100; gamma: scale; kernel: linear
     - Conjunto de teste representou 20%

## BERT
   - Tem a função de mapear a representação contextual bidirecional de toda a frase
   - Ou seja, ele prevê palavras com base nas anteriores e nas seguintes
   - Conjunto de dados foi dividido em três 
     - Treinamento (0.70); Validação (0.20); Teste (0.10)
   - Foi importado através do transformers
   - BertTokenizer: Realiza a tokenização das palavras
   - Modelo pré-treinado:
     - BertForSequenceClassification.from_pretrained('neuralmind/bert-base-portuguese-cased’)
     - 
# Resultados e Conclusão

| CLASSIFICADOR | ACC      | F1-MACRO | F1-AVG   |
|---------------|----------|----------|----------|
| BOW           | 0.709163 | 0.589962 | 0.682820 |
| EMBEDDINGS    | 0.657371 | 0.551792 | 0.653619 |
| BERT          | 0.650794 | 0.434994 | 0.624807 |

 - Devido ao desbalanceamento das classes apresentadas, todos os três classificadores encontraram dificuldades devido à pouca quantidade de amostras apresentadas;
   - Bert não conseguiu classificar elementos da classe “rate = 2”
 - Uma base de dados mais balanceada poderia trazer melhores resultados;
 - Nos três classificadores analisados, eles tiveram mais facilidade de identificar os casos extremos (maior e menor nota). 
   - Isto pode ser devido ao fato deles terem maiores quantidades de amostras ou  por possuírem palavras fortes que ajudam em suas classificações;
 - Por fim, classificador que apresentou melhores resultados entre os três analisados foi o SVM + BOW




