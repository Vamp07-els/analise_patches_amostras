# Tutorial de geração e análise de patches em imagens CBERS-4A

## Sobre o projeto

Este repositório apresenta um tutorial introdutório desenvolvido para estudantes, pesquisadores iniciantes e demais interessados em aprender a manipular imagens de sensoriamento remoto utilizando Python no Google Colab.

O objetivo é demonstrar, passo a passo, como analisar imagens de satélite, gerar patches (recortes de imagens) com diferentes dimensões e avaliar suas características para auxiliar na escolha de tamanhos adequados para projetos de classificação de imagens.

O material utiliza uma imagem do satélite CBERS-4A como exemplo prático e apresenta procedimentos de preparação e extração de amostras que podem ser utilizados em etapas posteriores de aprendizado profundo.

## O que você vai aprender?

- Abrir e visualizar imagens GeoTIFF.
- Identificar dimensões, bandas espectrais e resolução espacial.
- Compreender os valores NoData.
- Gerar patches com diferentes dimensões.
- Trabalhar com *stride* e sobreposição entre recortes.
- Comparar a área e o contexto espacial representados pelos patches.
- Selecionar e armazenar patches com dados válidos.
- Preparar imagens RGB para anotação no CVAT.
- Ler e analisar polígonos exportados do CVAT.
- Gerar máscaras de classificação.
- Converter anotações em Shapefile.
- Extrair amostras individuais em GeoTIFF.

## Preparação do ambiente

O tutorial foi desenvolvido no Google Colab e utiliza o Google Drive para armazenar as imagens de entrada e os arquivos gerados.

### 1. Organizar os arquivos no Google Drive

Antes de executar o notebook:

1. Acesse seu Google Drive.
2. Na seção **Meu Drive**, crie uma pasta chamada `CBERS4A`.
3. Coloque dentro dessa pasta a imagem GeoTIFF que será utilizada no processamento.
4. Para executar o código sem alterar o caminho original, a imagem deve estar nomeada exatamente como `Area1.tif`.

A organização inicial deverá ser:

```text
Meu Drive/
└── CBERS4A/
    └── Area1.tif
```

**Importante:** o código utiliza o seguinte caminho para abrir a imagem:

```python
arquivo = "/content/drive/MyDrive/CBERS4A/Area1.tif"
```

Caso a imagem tenha outro nome ou esteja armazenada em uma pasta diferente, será necessário modificar esse caminho no notebook.

O arquivo `Area1.tif` é a imagem de entrada utilizada como exemplo neste projeto. Ele não é criado automaticamente pelo código e deve estar disponível no Google Drive antes da execução.

### 2. Conectar o Google Drive ao Colab

No início do notebook, execute a célula:

```python
from google.colab import drive
drive.mount('/content/drive')
```

Quando solicitado, autorize o acesso à sua conta Google.

### 3. Organização das pastas de saída

Durante o processamento, o código cria automaticamente diferentes pastas dentro do diretório `CBERS4A` para armazenar os resultados.

```text
Meu Drive/
└── CBERS4A/
    ├── Area1.tif
    ├── patches_validos/
    │   ├── 32x32/
    │   ├── 64x64/
    │   ├── 96x96/
    │   └── 128x128/
    ├── CVAT_teste/
    ├── CVAT_anotacoes/
    ├── CVAT_mascaras/
    ├── CVAT_shapefile/
    ├── CVAT_XML_por_classe/
    └── CVAT_TIFF_individuais/
        ├── Vegetacao_arborea/
        └── Outros/
```

**Finalidade das pastas:**

- `patches_validos`: armazena os patches selecionados em GeoTIFF, separados por dimensão.
- `CVAT_teste`: armazena os patches convertidos para PNG RGB, utilizados na anotação.
- `CVAT_anotacoes`: pasta destinada ao arquivo ZIP exportado do CVAT.
- `CVAT_mascaras`: armazena as máscaras de classificação geradas a partir dos polígonos.
- `CVAT_shapefile`: armazena os arquivos vetoriais das anotações.
- `CVAT_XML_por_classe`: contém os arquivos XML separados conforme as classes.
- `CVAT_TIFF_individuais`: contém as amostras individuais extraídas em GeoTIFF, organizadas por classe.

**Atenção:** a pasta `CVAT_anotacoes` precisa ser criada manualmente e receber o arquivo ZIP exportado do CVAT antes da execução das etapas de leitura das anotações. As demais pastas de saída são criadas pelo notebook durante as respectivas etapas.

## Dimensões dos patches analisados

O tutorial trabalha com quatro dimensões de patches:

| Dimensão do patch | Stride | Dimensão no terreno |
|---|---|---|
| 32 × 32 pixels | 16 pixels | 64 × 64 m |
| 64 × 64 pixels | 32 pixels | 128 × 128 m |
| 96 × 96 pixels | 48 pixels | 192 × 192 m |
| 128 × 128 pixels | 64 pixels | 256 × 256 m |

As dimensões no terreno foram calculadas considerando a resolução espacial de 2 metros por pixel da imagem utilizada.

O stride adotado corresponde à metade do tamanho de cada patch, resultando em uma sobreposição de 50% nas direções horizontal e vertical.

A comparação permite compreender como os diferentes tamanhos dos recortes influenciam a área representada e o contexto espacial disponível para análise.

A identificação do tamanho mais adequado depende dos objetivos e dos critérios de avaliação de cada projeto.

## Organização do tutorial

**Caracterização da imagem**

Análise das dimensões, bandas espectrais, resolução espacial, sistema de referência, tipos de dados e visualização RGB.

**Geração e análise dos patches**

Geração dos recortes, identificação dos valores NoData, comparação entre dimensões, seleção e armazenamento dos patches em GeoTIFF.

**Anotação e extração de amostras**

Preparação das imagens para o CVAT, leitura dos polígonos, geração de máscaras, associação das anotações aos patches GeoTIFF e extração de amostras individuais por classe.

## Classes utilizadas

O tutorial apresenta uma proposta de classificação com duas classes:

- **Vegetação arbórea:** árvores e vegetação de porte elevado.
- **Outros:** edificações, vias, solo exposto, vegetação rasteira e demais elementos da paisagem urbana.

O notebook também apresenta uma definição alternativa com cinco classes para exemplificar outra possibilidade de organização das categorias.

## Ferramentas e bibliotecas

- Python
- Google Colab
- Google Drive
- Rasterio
- NumPy
- Matplotlib
- Pandas
- Pillow
- CVAT
- GeoPandas
- Shapely
- Tifffile

## Público-alvo

Este material é destinado principalmente a pessoas que estão iniciando seus estudos em programação Python aplicada ao sensoriamento remoto.

O código está organizado em etapas comentadas para facilitar a compreensão, a reprodução dos procedimentos e a adaptação a outros conjuntos de imagens.

## Observações importantes

- Recomenda-se executar as etapas na sequência apresentada.
- É necessário disponibilizar a imagem de entrada no Google Drive.
- Os caminhos dos arquivos podem ser adaptados conforme a organização de cada usuário.
- A anotação no CVAT é uma etapa externa ao Google Colab.
- A comparação das dimensões dos patches possui caráter exploratório e não determina isoladamente o melhor tamanho para treinamento de redes neurais.

## Autoria e licença

**Autoria:** Carine Lopes Elias

**Ano:** 2026

**Licença:** MIT

O código está disponível para utilização, estudo, modificação e distribuição conforme os termos da licença MIT, preservando-se os avisos de direitos autorais e licença.

Caso utilize este material em trabalhos acadêmicos, solicita-se a citação da autoria e do repositório.
