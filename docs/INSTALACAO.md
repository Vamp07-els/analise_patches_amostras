## Como baixar e executar o tutorial

O código está disponibilizado em formato Jupyter Notebook (`.ipynb`) e pode ser utilizado no Google Colab ou no Visual Studio Code (VS Code).

Para acompanhar o tutorial, recomenda-se baixar uma cópia do notebook e executá-lo em seu próprio ambiente.

### 1. Como baixar o código do GitHub

1. Na página principal deste repositório, clique no botão verde **Code**.
2. Selecione **Download ZIP**.
3. Após o download, extraia o conteúdo do arquivo ZIP em uma pasta do computador.
4. Localize o arquivo do notebook, com extensão `.ipynb`.

### 2. Como abrir no Google Colab

O Google Colab permite executar notebooks Python diretamente no navegador, sem necessidade de instalar Python no computador.

1. Acesse https://colab.research.google.com/.
2. No menu superior, clique em **Arquivo → Fazer upload de notebook**. Alternativamente, utilize a opção **Upload** na janela de abertura.
3. Selecione o arquivo `.ipynb` que foi baixado do GitHub.
4. Aguarde a abertura do notebook.
5. Antes de executar as células, leia as orientações iniciais sobre a preparação do Google Drive.

**Importante:** o tutorial utiliza o Google Drive como local de leitura e armazenamento dos arquivos. Por isso, é necessário organizar as pastas e modificar o nome da imagem identificada como `Area1.tif` no código, conforme as instruções apresentadas na seção "Preparação do ambiente".

### 3. Como abrir no Visual Studio Code (VS Code)

O Visual Studio Code também permite abrir e executar notebooks Jupyter em um ambiente de desenvolvimento local.

1. Instale o Visual Studio Code em https://code.visualstudio.com/.
2. Instale o Python em seu computador, caso ainda não esteja disponível.
3. Abra o VS Code.
4. Na seção **Extensions (Extensões)**, instale as extensões **Python** e **Jupyter**, disponibilizadas pela Microsoft.
5. Selecione **File → Open File** e abra o notebook `.ipynb` baixado do GitHub.
6. Selecione um ambiente Python (kernel) que tenha as bibliotecas necessárias.
7. Instale as bibliotecas utilizadas no projeto antes de executar as células.

**Atenção:** o notebook foi desenvolvido originalmente no Google Colab e contém comandos específicos desse ambiente, incluindo a conexão ao Google Drive por meio de `google.colab`.

Para executar o projeto localmente no VS Code, será necessário adaptar os caminhos dos arquivos, substituir os comandos exclusivos do Colab e configurar as dependências. Portanto, a execução integral no VS Code não é garantida sem essas adaptações.

### 4. Recomendações para iniciantes

Recomenda-se utilizar inicialmente o Google Colab, pois o tutorial foi desenvolvido e organizado nesse ambiente.

Antes de executar o código, confira:

- A disponibilidade da imagem GeoTIFF de entrada.
- A conferência do nome e do caminho da imagem GeoTIFF de entrada, ajustando-os no código conforme o arquivo utilizado pelo usuário (nomeada como `Area1.tif` no código original).
- A criação da pasta principal `CBERS4A` no Google Drive.
- As instruções para importação das anotações do CVAT.
- A sequência das etapas e as dependências entre as células.

O código pode ser adaptado para diferentes imagens e projetos, desde que sejam consideradas suas características espaciais, espectrais e seus formatos de armazenamento.
