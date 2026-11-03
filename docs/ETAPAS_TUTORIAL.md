# Guia das etapas do tutorial

## Apresentação

Este documento apresenta uma visão geral das etapas desenvolvidas no notebook de geração, análise e extração de patches em imagens CBERS-4A.

O objetivo é auxiliar estudantes e pesquisadores iniciantes na compreensão dos procedimentos e dos resultados produzidos durante o processamento.

Recomenda-se acompanhar este guia juntamente com o notebook, executando as células na sequência apresentada.

---

## Parte 1 — Caracterização da imagem

### Etapas 1 a 3 — Preparação do ambiente

Inicialmente, são importadas as bibliotecas necessárias e estabelecida a conexão entre o Google Colab e o Google Drive.

O Google Drive é utilizado para acessar a imagem de entrada e armazenar os arquivos produzidos.

### Etapas 4 a 7 — Propriedades da imagem

A imagem GeoTIFF é aberta com a biblioteca Rasterio.

São identificadas suas principais características:

- Largura e altura em pixels.
- Quantidade de bandas espectrais.
- Resolução espacial.
- Sistema de referência de coordenadas (CRS).
- Formato do arquivo.
- Tipo de dado dos pixels.
- Valor NoData.

Essas informações são importantes para compreender as características da imagem antes de realizar os recortes.

### Etapas 8 e 9 — Valores dos pixels e visualização

São analisados os valores mínimos e máximos dos pixels em cada banda.

Em seguida, é produzida uma composição RGB para visualização da imagem.

O ajuste dos valores utilizado nessa visualização tem finalidade de exibição e não substitui os dados espectrais originais.

---

## Parte 2 — Geração, análise e seleção dos patches

### Etapa 1 — Geração dos patches

A imagem é dividida em recortes quadrados denominados *patches*.

São utilizadas quatro dimensões:

| Patch | Stride |
|---|---|
| 32 × 32 pixels | 16 pixels |
| 64 × 64 pixels | 32 pixels |
| 96 × 96 pixels | 48 pixels |
| 128 × 128 pixels | 64 pixels |

O stride corresponde ao deslocamento da janela de recorte entre posições consecutivas.

Neste tutorial, o deslocamento equivale à metade da dimensão do patch, produzindo uma sobreposição de 50% em cada direção.

### Etapas 2 a 4 — Análise dos pixels NoData

Após a geração dos patches, é verificada a presença de pixels NoData.

Esses pixels representam posições sem dados válidos, de acordo com o valor NoData definido no arquivo.

O notebook calcula:

- Quantidade de patches com e sem NoData.
- Percentual de NoData por patch.
- Percentuais mínimo, máximo e médio.
- Quantidade de patches completamente vazios.
- Distribuição dos resultados por dimensão.

Essa análise permite identificar recortes que possuem informações úteis e aqueles que não contêm dados válidos.

### Etapa 5 — Comparação visual

São apresentados patches com diferentes proporções de NoData.

O objetivo é facilitar a interpretação dos percentuais calculados e observar como a ausência de dados aparece nas imagens.

### Etapa 6 — Seleção dos patches

Os patches são separados em três grupos:

- Sem NoData.
- Com NoData parcial.
- Completamente vazios.

No procedimento implementado, são preservados os patches que apresentam pelo menos alguma informação válida.

Os patches com 100% de NoData são descartados.

### Etapas 7 a 10 — Salvamento e conferência

Os patches preservados são salvos individualmente em formato GeoTIFF, organizados em pastas conforme suas dimensões.

Durante o salvamento, são mantidos os dados espectrais e as informações de georreferenciamento.

Também é gerada uma tabela CSV com informações dos patches, incluindo dimensão, stride, localização na imagem e percentual de NoData.

As etapas seguintes verificam a existência dos arquivos gerados e apresentam exemplos para inspeção visual.

### Etapa 11 — Definição das classes

O notebook apresenta duas propostas de classificação.

**Classificação com duas classes:**

- Vegetação arbórea.
- Outros.

**Classificação com cinco classes:**

- Edificado.
- Superfície impermeável.
- Solo exposto.
- Vegetação arbórea.
- Vegetação rasteira.

A etapa de anotação apresentada posteriormente utiliza a classificação com duas classes.

### Etapa 12 — Área coberta e contexto espacial

São calculadas as dimensões dos patches no terreno, considerando a resolução espacial da imagem utilizada.

Para a resolução de 2 metros por pixel:

| Patch | Dimensão no terreno |
|---|---|
| 32 × 32 pixels | 64 × 64 m |
| 64 × 64 pixels | 128 × 128 m |
| 96 × 96 pixels | 192 × 192 m |
| 128 × 128 pixels | 256 × 256 m |

Os patches menores permitem observar regiões mais localizadas, enquanto os maiores abrangem uma área mais extensa.

Essa comparação ajuda a compreender como a escolha das dimensões influencia o contexto espacial representado.

### Etapa 13 — Síntese dos resultados

São apresentados os totais de patches gerados, preservados e descartados, separados por dimensão.

O resumo também inclui informações sobre a área representada no terreno e as classes definidas.

O notebook contém ainda uma análise técnica dos arquivos salvos, incluindo quantidade de bandas, tipos de dados, profundidade em bits e organização das páginas TIFF.

### Etapa 14 — Comparação visual entre dimensões

São apresentados recortes de diferentes tamanhos extraídos de uma mesma região da imagem.

Essa comparação permite observar como a quantidade de elementos e o contexto espacial variam conforme as dimensões dos patches.

**Importante:** essa análise é exploratória. A escolha do tamanho mais adequado para uma aplicação depende dos objetivos do projeto e de avaliações posteriores.

---

## Parte 3 — Anotação e extração de amostras

### Etapa 15 — Preparação das imagens para o CVAT

São selecionados cinco patches de 64 × 64 pixels sem NoData.

Esses recortes são convertidos em imagens PNG RGB para serem utilizados na ferramenta CVAT.

Os arquivos GeoTIFF originais permanecem disponíveis para as etapas posteriores.

### Etapas 16 a 18 — Importação das anotações

Após a anotação das imagens no CVAT, o arquivo ZIP exportado é armazenado no Google Drive.

O notebook localiza esse arquivo, lê as anotações XML e identifica:

- Quantidade de imagens anotadas.
- Quantidade de polígonos.
- Classes utilizadas.
- Quantidade de pontos dos polígonos.

### Etapa 19 — Visualização dos polígonos

Os polígonos exportados são sobrepostos às imagens RGB.

Essa visualização permite conferir a localização das regiões anotadas e as classes atribuídas.

### Etapa 20 — Geração das máscaras

Os polígonos são convertidos em máscaras de classificação.

No exemplo utilizado, os pixels recebem os valores:

| Valor | Significado |
|---|---|
| 0 | Outros |
| 1 | Vegetação arbórea |
| 255 | Não anotado |

As máscaras são armazenadas como arquivos PNG.

### Etapa 21 — Comparação entre imagens e máscaras

São apresentadas as imagens originais ao lado das máscaras correspondentes.

Essa etapa facilita a inspeção visual das regiões classificadas.

### Etapa 22 — Associação das máscaras aos GeoTIFFs

O notebook verifica a correspondência entre os arquivos GeoTIFF originais e as máscaras produzidas.

São conferidas as dimensões das imagens e a quantidade de bandas dos recortes espectrais.

### Etapas posteriores — Exportação e extração das amostras

O notebook também apresenta procedimentos para:

- Converter os polígonos anotados em Shapefile.
- Separar as anotações XML por classe.
- Extrair individualmente as regiões delimitadas pelos polígonos.
- Salvar as amostras em formato GeoTIFF com quatro bandas.
- Organizar os arquivos extraídos nas pastas das respectivas classes.

Esses procedimentos permitem preparar conjuntos de amostras para análises posteriores.

---

## Recomendações para acompanhar o tutorial

1. Leia as orientações de preparação dos arquivos antes de iniciar.
2. Execute as células na sequência indicada.
3. Observe as informações apresentadas nas saídas de cada etapa.
4. Confira os arquivos gerados no Google Drive.
5. Compare os resultados de diferentes dimensões de patches.
6. Antes de adaptar o tutorial a outras imagens, verifique suas características espaciais e espectrais.

##
