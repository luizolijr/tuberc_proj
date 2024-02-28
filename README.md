# An�lise dos dados de TUBERCULOSE do DATASUS

Este � um projeto de an�lise de dados com Python. O projeto � apresentado no Jupyter, mas nas fases de obten��o e tratamento dos dados brutos fez-se necess�rio tamb�m o uso do R.

Dados: consulta do banco de dados do Sistema de Informa��es Agravos de Notifica��o (SINAN) do DATASUS. Abrangendo pacientes no �mbito do Sistema �nico de Sa�de por todo o Brasil no per�odo de 2022.

Foi investigado dados de tuberculose em diferentes frentes como:  
Casos entre homens e mulheres;  
Casos entre entrangeiros;  
Casos na popula��o carcer�ria;  
Casos na popula��o de rua;  
Casos na popula��o gr�vida;  
Escolaridade vs numero de casos;  
Idade vs numero de casos;  


# Obten��o dos dados via FTP do datasus
Os dados podem ser extra�dos manualmente atrav�s do site do [TabWin](https://datasus.saude.gov.br/transferencia-de-arquivos/), selecionando SINAN e o per�odo desejado. Isso ir� gerar um link FTP, que usamos python para fazer o download, vide c�digo em "coleta_dados.ipynb".

# Convers�o dos dados .DBC para .CSV utilizando R no Windows
!! Aten��o: substituir caminhos de arquivo pelos seus !!  

1 - Baixar a biblioteca [read.dbc](https://cran.r-project.org/src/contrib/Archive/read.dbc/) (baixei a mais recente)  

2 - Instalar a biblioteca  
path<-"C:/read.dbc_1.0.6.tar.gz"  
install.packages(path, repos = NULL, type="source")

3 - Criar path do arquivo dbc e rodar comandos  
path_dbc<-"C:/REPOS/data_sus/data_dbc/RDRJ2301.dbc"  
library(read.dbc)  
df<-read.dbc(path_dbc)  
head(df)

4 - Salvar em csv  
load_path<-"C:/REPOS/data_sus/data_dbc/RDRJ2301.dbc"  
export_path<-"C:/REPOS/data_sus/data_csv/RDRJ2301.csv"  
write.csv2(df, export_path)

5 - Transformar em grande escala - dbc em csv  
L� a pasta de arquivos dbc com um FOR, transforma um por um em csv e vai guardando na pasta de arquivos csv  
dbc_folder <- "C:/REPOS/data_sus/data_dbc/"  
csv_folder <- "C:/REPOS/data_sus/data_csv/"  
for(f in files) {  
  print(f)  
  df = read.dbc(f)  
  lista = strsplit(f, "/")[[1]]  
  file = gsub(".dbc", ".csv", lista[length(lista)])  
  write.csv2(df, paste(csv_folder, file, sep="/"), row.names=FALSE)  
}

# Tratamento dos Dados
Descobrimos que grande parte dos dados vinham em c�digos, sendo assim, precisamos da documenta��o para tratar os dados, uma das fontes de consulta foi a pr�pria docuemnta��o dispon�vel no [TabWin](https://datasus.saude.gov.br/transferencia-de-arquivos/) > SINAN > Documenta��o.


# Conclus�o

No gr�fico dos casos de tuberculose divididos por sexo, podemos destacar que a doen�a atinge quase o dobro de homens que mulheres em quase todos os estados do pa�s, com exce��o de S�o Paulo que apresenta grande disparidade com uma alta taxa da doen�a no sexo feminino. No gr�fico sobre a incid�ncia da doen�a sobre faixa et�ria, podemos notar que h� uma escalada de casos da doen�a entre os 18 aos 25 anos, se mantendo alta mas numa decrescente at� os 60 anos, tendo alguns picos entre 40-50 anos. Dados divulgados pelo Minist�rio da Sa�de (vide links para refer�ncia) apontam que homens de 20 a 64 anos apresentam risco tr�s vezes maior de contrair a doen�a do que as mulheres de mesma faixa et�ria, o que ser� analisado em um segundo momento. Contudo, � importante destacar que na nossa an�lise, na maioria dos estados do pa�s, a doen�a atinge na maior parte homens e com predomin�ncia na faixa et�ria dos 20 a 60 anos, a princ�pio para ambos os sexos.

Sobre os dados de Imigrantes - considerados aqueles que est�o h� menos de 2 anos no pa�s - os estados de fronteira e os principais destinos de imigrantes no pa�s apresentam as maiores taxas de tuberculose, com dados alt�ssimos em Roraima, com 17.58% dos casos. Isso se explica pela grande concentra��o de imigrantes que pedem ref�gio no Brasil. Para termos de compara��o, dados da Unicef indicam que entre 2017 e 2020, o pa�s recebeu mais de 609 mil migrantes venezuelanos. Em compara��o com o bi�nio anterior, isso representa um acr�scimo de 922%, sendo o Brasil o pa�s com maior abrigo dessa nacionalidade no mundo. A maioria entra pela fronteira norte do pa�s, no Estado de Roraima, o que destaca o alto �ndice. � importante ressaltar que, a partir de 2018, diversas not�cias de jornal apontavam para uma crise do sistema de sa�de na Venezuela com a tuberculose, dentre outras doen�as que j� haviam sido erradicadas, de modo que a ONU apontou caso de emerg�ncia humanit�ria. Podemos destacar os altos �ndices do Amap�, Rio Grande do Norte, Mato Grosso do Sul (que recebe bastantes venezuelanos, tendo uma rede de apoio local), dentre outros.

Roraima tamb�m possui um m�ximo de casos da doen�a na an�lise de Popula��o carcer�ria e isso reflete surtos da doen�a em pres�dios em RR em 2022, com mortes at� 2023. A doen�a tem f�cil dissemina��o e encontra na m� nutri��o dos presos e na densidade populacional, um ambiente prop�cio. O gr�fico acaba por imprimir uma realidade das cadeias do pa�s, concentrando casos em uma escala que envolve Pernambuco (que tem uma das maiores cadeias), Rio Grande do Norte, Roraima, dentre outras.

Analisamos tamb�m os dados da popula��o de rua e esses respondem, em m�dia, por cerca de 5% dos casos de tuberculose, por estado. Contudo, estamos cientes da subnotifica��o dessa popula��o, ent�o como pr�ximo passo gostar�amos de correlacionar esses dados com o gr�fico de Tratamentos, para verificar se dentro dessa popula��o h� um alto percentual de abandono do tratamento, tendo em vista a situa��o de vulnerabilidade social extrema em que se encontram.

Destacamos tamb�m o m�s de m�xima de casos de tuberculose em cada estado e encontramos o m�s de agosto como o mais recorrente. Esse m�s no nosso clima corresponde ao final do inverno, o que pode indicar tamb�m uma dificuldade de identifica��o da doen�a que pode ser confundida com uma gripe forte devido �s mudan�as clim�ticas. No entanto, a demora na busca do tratamento leva a uma maior dissemina��o da doen�a, tendo em vista o contato da pessoa infectada com pessoas da fam�lia na mesma casa, ambiente de trabalho, dentre outros.

O Rio de Janeiro vive uma crise na sa�de, sendo que o estado tem observado um crescente n�mero de mortes pela doen�a. Em 2019 foram 659 �bitos; em 2020, 795, e em 2021, 805, de acordo com a Secretaria de Sa�de do Rio de Janeiro. A pobreza tende a ampliar a dissemina��o da doen�a, com as p�ssimas condi��es sanit�rias, muitas pessoas dividindo o mesmo espa�o e a desnutri��o, como j� foi mencionado anteriormente. Por curiosidade, no Rio, em 6 de agosto, comemora-se o Dia Estadual de Conscientiza��o, Mobiliza��o e Combate � Tuberculose. O gr�fico espelha a grande desigualdade social que vivemos no Brasil, e que o Rio de Janeiro tem se tornado s�mbolo, acompanhado de Rio Grande do Sul, Pernambuco,S�o Paulo, Par�, dentre outros.

Por fim, ao montar o gr�fico de escolaridade, percebemos a import�ncia de trazer os dados por estado para a an�lise, pois o total de casos por escolaridade obscurece as diferen�as regionais que temos no pa�s. Outro dado importante para a an�lise � o n�mero total da popula��o por estado para termos a compreens�o do percentual que os casos de tuberculose representam sobre a popula��o total. Desse modo, o gr�fico de pizza, realizado em uma primeira etapa, n�o leva a conclus�es v�lidas.

Realizamos tamb�m a an�lise da popula��o de gr�vidas e os dados parecem indicar uma falta de acompanhamento do pr�-natal que � realizado pelo SUS e at� mesmo desconhecimento dos sintomas da tuberculose, tendo muitos casos da doen�a em est�gios avan�ados da gravidez.

Por �ltimo, temos o gr�fico de tratamento que possui um n�mero alto de ?Reingresso ap�s abandono?, mas o que chama mais aten��o � o n�mero de pessoas que responderam ?N�o sabe? quanto � quest�o se j� tiveram a doen�a ou n�o, o que leva mais uma vez � falta de informa��o da doen�a. Contudo, como informado anteriormente, esse gr�fico ser� melhor analisado, tendo seus dados cruzados com outras vari�veis.

Em 2022, o Brasil teve mais de 78 mil novos casos da doen�a segundo o Minist�rio de Sa�de e, em 2021, teve recorde de mortes, mais de 5 mil, o maior n�mero em 10 anos, tendo perdido apenas para a COVID. Ela � ainda a principal causa de morte de pessoas com AIDS. � importante destacar tamb�m que 48% das fam�lias afetadas de alguma forma pela tuberculose t�m gastos com a doen�a que comprometem acima de 20% da renda, o que pode levar fam�lias inteiras � condi��o de pobreza e mis�ria. Desse modo, destaca-se a import�ncia dos dados divulgados pelo Sinan, Sistema de Informa��o de Agravos de Notifica��o, do SUS, para que possamos fazer an�lises e criar dados para pautar pol�ticas p�blicas que fa�am a diferen�a para o pa�s.

Viva o SUS!
