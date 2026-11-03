# Organização dos dados e das pastas

## 1. Apresentação

Este documento apresenta a organização dos arquivos utilizados no tutorial de geração, análise e extração de patches em imagens CBERS-4A.

O processamento foi desenvolvido no Google Colab, utilizando o Google Drive para acessar as imagens de entrada e armazenar os resultados produzidos durante a execução.

As orientações a seguir auxiliam principalmente usuários iniciantes na preparação do ambiente.

## 2. Preparação do Google Drive

Antes de executar o notebook, é necessário organizar os arquivos de entrada.

1. Acesse seu Google Drive.
2. Na seção **Meu Drive**, crie uma pasta chamada `CBERS4A`.
3. Coloque dentro dessa pasta a imagem GeoTIFF que será utilizada no processamento.
4. Confira o nome e a localização do arquivo.
5. Ajuste o caminho da imagem no código, caso necessário.

No exemplo utilizado neste tutorial, a imagem foi denominada `Area1.tif` e está localizada em:

```text
Meu Drive/
└── CBERS4A/
    └── Area1.tif
```

**Importante:** `Area1.tif` é apenas o nome da imagem utilizada no desenvolvimento deste tutorial. O usuário pode utilizar outra imagem GeoTIFF, com nome diferente, desde que ajuste o caminho correspondente no código.

Outras imagens podem apresentar diferentes resoluções espaciais, quantidades de bandas, dimensões e valores NoData. Essas características devem ser consideradas ao adaptar os procedimentos.

## 3. Configuração do caminho da imagem

No notebook, a imagem de entrada é definida pelo seguinte comando:

```python
arquivo = "/content/drive/MyDrive/CBERS4A/Area1.tif"
```

Caso o usuário utilize, por exemplo, uma imagem denominada `imagem_teste.tif`, armazenada na mesma pasta, o caminho deverá ser alterado para:

```python
arquivo = "/content/drive/MyDrive/CBERS4A/imagem_teste.tif"
```

O nome informado no código deve corresponder exatamente ao nome do arquivo armazenado no Google Drive.

## 4. Conexão ao Google Drive

Para acessar os arquivos no Google Colab, o notebook utiliza:

```python
from google.colab import drive
drive.mount('/content/drive')
```

Após a execução da célula, será necessário autorizar o acesso ao Google Drive.

A conexão deve ser realizada antes das etapas que leem ou salvam arquivos.

## 5. Organização das pastas do projeto

Durante a execução do notebook, diferentes pastas são utilizadas para armazenar os resultados intermediários e finais.

A estrutura do projeto é apresentada a seguir:

```text
Meu Drive/
└── CBERS4A/
    ├── Area1.tif
    │
    ├── patches_validos/
    │   ├── 32x32/
    │   ├── 64x64/
    │   ├── 96x96/
    │   └── 128x128/
    │
    ├── CVAT_teste/
    │
    ├── CVAT_anotacoes/
    │
    ├── CVAT_mascaras/
    │
    ├── CVAT_shapefile/
    │
    ├── CVAT_XML_por_classe/
    │
    └── CVAT_TIFF_individuais/
        ├── Vegetacao_arborea/
        └── Outros/
```

A estrutura apresentada corresponde à organização utilizada no tutorial. A pasta principal, o arquivo de entrada e a pasta de anotações devem ser preparados pelo usuário; as demais pastas de saída são criadas pelo código durante as respectivas etapas.

## 6. Descrição das pastas

### patches_validos

Armazena os patches selecionados em formato GeoTIFF.

Os arquivos são organizados automaticamente em quatro subpastas, conforme as dimensões utilizadas:

- `32x32`
- `64x64`
- `96x96`
- `128x128`

Os arquivos preservam as bandas espectrais, os tipos de dados e as informações de georreferenciamento da imagem de origem.

Essa pasta também recebe tabelas CSV produzidas durante as etapas de análise e conferência.

### CVAT_teste

Armazena as imagens PNG RGB preparadas para anotação na ferramenta CVAT.

No exemplo apresentado, são selecionados cinco patches de 64 × 64 pixels sem NoData para demonstrar a etapa de anotação.

### CVAT_anotacoes

Armazena o arquivo ZIP contendo as anotações exportadas do CVAT.

**Esta pasta precisa ser criada manualmente pelo usuário.**

Após realizar as anotações no CVAT, o arquivo ZIP exportado deve ser colocado nessa pasta para que as etapas seguintes possam localizar e processar os polígonos.

### CVAT_mascaras

Armazena as máscaras de classificação geradas a partir dos polígonos anotados.

As máscaras utilizam os códigos:

- `0`: Outros.
- `1`: Vegetação arbórea.
- `255`: Pixels não anotados.

### CVAT_shapefile

Armazena os arquivos vetoriais produzidos a partir da conversão das anotações do CVAT para o formato Shapefile.

Os polígonos são associados às coordenadas geográficas dos patches GeoTIFF correspondentes.

### CVAT_XML_por_classe

Armazena arquivos XML separados conforme as classes utilizadas na anotação.

No exemplo do tutorial, são gerados arquivos para as classes Vegetação arbórea e Outros.

### CVAT_TIFF_individuais

Armazena os recortes individuais das regiões delimitadas pelos polígonos anotados.

As amostras são salvas em formato GeoTIFF, preservando quatro bandas e as informações de georreferenciamento, conforme as características dos arquivos utilizados no exemplo.

Os arquivos são separados nas pastas:

- `Vegetacao_arborea`
- `Outros`

## 7. Pastas criadas automaticamente e manualmente

| Pasta ou arquivo | Preparação |
|---|---|
| `CBERS4A` | Criar manualmente |
| Imagem GeoTIFF de entrada | Adicionar manualmente |
| `patches_validos` | Criada pelo código |
| `CVAT_teste` | Criada pelo código |
| `CVAT_anotacoes` | Criar manualmente |
| ZIP de anotações do CVAT | Exportar e adicionar manualmente |
| `CVAT_mascaras` | Criada pelo código |
| `CVAT_shapefile` | Criada pelo código |
| `CVAT_XML_por_classe` | Criada pelo código |
| `CVAT_TIFF_individuais` | Criada pelo código |

## 8. Cuidados durante a execução

Para evitar erros, recomenda-se:

1. Verificar se o Google Drive está conectado ao Colab.
2. Conferir o caminho completo da imagem de entrada.
3. Verificar se o arquivo GeoTIFF pode ser aberto corretamente.
4. Executar as etapas do notebook na ordem apresentada.
5. Conferir a existência dos arquivos gerados antes de utilizá-los nas etapas seguintes.
6. Criar a pasta `CVAT_anotacoes` e adicionar o ZIP exportado do CVAT antes de executar as etapas que dependem dele.
7. Adaptar os parâmetros quando utilizar imagens com características diferentes da imagem de exemplo.

## 9. Observação final

A estrutura apresentada corresponde ao ambiente utilizado durante o desenvolvimento do tutorial.

Os caminhos, nomes dos arquivos, dimensões dos patches e demais parâmetros podem ser adaptados conforme os objetivos e as características dos dados utilizados em outros projetos.
