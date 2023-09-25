# webscrapipng-ReclameAqui
Função para raspagem de reclamações do site Reclame Aqui. 
A função recebe como argumentos o url do perfil da empresa no site, a quantidade de reclamações que se deseja coletar e o formato e diretório para exportação de um arquivo em excel, csv ou ambos.

O script pode ser usado por não programadores a partir de um tutorial amigável disponível no Google Colab, adaptado para salvar os dados em um arquivo no Google Drive do usuário:
https://colab.research.google.com/drive/10NL4wL9tQA1xuZZT_fkkiyDwVQfZLpVk?usp=sharing

# Como funciona

## Funções internas
A função scrape_RA( ) trabalha com três funções em seu interior, definidas no notebook, que resolvem etapas diferentes do processo de raspagem:

### figure_out_target_pages(n_of_complaints) 
A função recebe como parâmetro o número de reclamações que se deseja coletar (n_of_complaints) e, a partir desse input, define os números de páginas corretos para raspagem.
O site Reclame Aqui apresenta um comportamento específico: cada nova página de busca oferece uma nova reclamação e repete as 9 anteriores. 
A função calcula os números de páginas que oferecem apenas novas reclamações a serem inseridas na URL requisitada pelo script e retorna uma lista com a sequência de números corretos para raspar a quantidade de reclamações indicada.
#### Parâmetros:
**n_of_complaints** (int)

Número inteiro que indica o número de reclamações a serem coletadas. Aceita até o número máximo de reclamações registradas na página de reclamações no perfil da empresa no site Reclame Aqui.


### scrape_search_results_RA (company_name_url, n_of_complaints)
A função recebe o padrão URL do nome da empresa e chama a função figure_out_target_pages( ) e guarda a lista de números de páginas corretas.
Em seguida itera sobre a lista de números de páginas, adicionando-os a novos requests para acessar os resultados da busca por reclamações no perfil da empresa.
Cria uma variável contendo o conteúdo html com BeautifulSoup e identifica os padrões html que identificam cada reclamação na página (cards) e os armazena em uma lista.
Em seguida itera sobre a lista de  cards e armazena título, data e link em listas separadas na mesma ordem.
Verifica os lengths das listas para garantir que possuem o mesmo número de dados e salva as listas como value de keys em um dicionário.
#### Parâmetros:
**n_of_complaints** (int)

Número inteiro que indica o número de reclamações a serem coletadas. Aceita até o número máximo de reclamações registradas na página de reclamações no perfil da empresa no site Reclame Aqui.

**company_name_url** (str)

String contendo o nome da empresa conforme registrado na URL do perfil da empresa no site Reclame Aqui


### scrape_complaints_RA(RA_dict)
A função recebe como argumento o dicionário gerado pela função scrape_search_results_RA( ).
Itera sobre a lista de links armazenadas no dicionário e realiza um request.
Acessa as páginas completas de reclamações, cria uma variável contendo o conteúdo html com BeautifulSoup e identifica os padrões html que identificam reclamações e respostas.
Coleta reclamações e respostas separadamente e adiciona as strings em uma lista armazenada como value das keys compaints e answers do dicionário recebido como parâmetro da função.
Páginas com respostas inexistentes geram um valor Null, que são transformados na string "Não Respondido" e adcionadas na lista de answers.
#### Parâmetros:
**RA_dict** (dict)

Variável contendo dicionário gerado pela função scrape_search_results_RA( )


## Função scrape_RA(company_url, n_of_complaints, file_type, save_path)
A função scrape_RA( ) recebe parâmetros simples de serem acessados pelo usuário e trabalha com as funções definidas acima para coletar títulos, datas, links, textos de reclamações e repostas das reclamações registradas na página de perfil da empresa no Reclame Aqui.
Produz um dicionário que armazena os dados em listas de mesmo tamanho e cria um pandas dataframe a partir do dicionáirio.
Em seguinda transforma o dataframe em um arquivo de dados e o salva no disco local.
#### Parâmetros:
**company_url**
A url da do perfil da empresa no site Reclame Aqui. 
O script remove automaticamente dessa url apenas o nome padrão da empresa utilizado nas URLs do site.

**n_of_complaints**
Define o número de reclamações que o usuário deseja coletar do site. Suporta até o número máximo de reclamações registradas no perfil da empresa no site.

**file_type**
Indica o formato de arquivo desejado: "excel", "csv" ou "csv + excel"

**save_path**
Indica o diretório no qual se deseja salvar o arquivo com os dados coletados. 
A string contendo o path deve duplicar as barras invertidas '\' para funcionar corretamente.
Ex: "C:\\Users\\João\\Desktop"




