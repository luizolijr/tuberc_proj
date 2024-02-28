# Análise dos dados de TUBERCULOSE do DATASUS

Este é um projeto de análise de dados com Python. O projeto é apresentado no Jupyter, mas nas fases de obtenção e tratamento dos dados brutos fez-se necessário também o uso do R.

Dados: consulta do banco de dados do Sistema de Informações Agravos de Notificação (SINAN) do DATASUS. Abrangendo pacientes no âmbito do Sistema Único de Saúde por todo o Brasil no período de 2022.

Foi investigado dados de tuberculose em diferentes frentes como:  
Casos entre homens e mulheres;  
Casos entre entrangeiros;  
Casos na população carcerária;  
Casos na população de rua;  
Casos na população grávida;  
Escolaridade vs numero de casos;  
Idade vs numero de casos;  


# Obtenção dos dados via FTP do datasus
Os dados podem ser extraídos manualmente através do site do [TabWin](https://datasus.saude.gov.br/transferencia-de-arquivos/), selecionando SINAN e o período desejado. Isso irá gerar um link FTP, que usamos python para fazer o download, vide código em "coleta_dados.ipynb".

# Conversão dos dados .DBC para .CSV utilizando R no Windows
!! Atenção: substituir caminhos de arquivo pelos seus !!  

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
Lê a pasta de arquivos dbc com um FOR, transforma um por um em csv e vai guardando na pasta de arquivos csv  
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
Descobrimos que grande parte dos dados vinham em códigos, sendo assim, precisamos da documentação para tratar os dados, uma das fontes de consulta foi a própria docuemntação disponível no [TabWin](https://datasus.saude.gov.br/transferencia-de-arquivos/) > SINAN > Documentação.


# Conclusão

No gráfico dos casos de tuberculose divididos por sexo, podemos destacar que a doença atinge quase o dobro de homens que mulheres em quase todos os estados do país, com exceção de São Paulo que apresenta grande disparidade com uma alta taxa da doença no sexo feminino. No gráfico sobre a incidência da doença sobre faixa etária, podemos notar que há uma escalada de casos da doença entre os 18 aos 25 anos, se mantendo alta mas numa decrescente até os 60 anos, tendo alguns picos entre 40-50 anos. Dados divulgados pelo Ministério da Saúde (vide links para referência) apontam que homens de 20 a 64 anos apresentam risco três vezes maior de contrair a doença do que as mulheres de mesma faixa etária, o que será analisado em um segundo momento. Contudo, é importante destacar que na nossa análise, na maioria dos estados do país, a doença atinge na maior parte homens e com predominância na faixa etária dos 20 a 60 anos, a princípio para ambos os sexos.

Sobre os dados de Imigrantes - considerados aqueles que estão há menos de 2 anos no país - os estados de fronteira e os principais destinos de imigrantes no país apresentam as maiores taxas de tuberculose, com dados altíssimos em Roraima, com 17.58% dos casos. Isso se explica pela grande concentração de imigrantes que pedem refúgio no Brasil. Para termos de comparação, dados da Unicef indicam que entre 2017 e 2020, o país recebeu mais de 609 mil migrantes venezuelanos. Em comparação com o biênio anterior, isso representa um acréscimo de 922%, sendo o Brasil o país com maior abrigo dessa nacionalidade no mundo. A maioria entra pela fronteira norte do país, no Estado de Roraima, o que destaca o alto índice. É importante ressaltar que, a partir de 2018, diversas notícias de jornal apontavam para uma crise do sistema de saúde na Venezuela com a tuberculose, dentre outras doenças que já haviam sido erradicadas, de modo que a ONU apontou caso de emergência humanitária. Podemos destacar os altos índices do Amapá, Rio Grande do Norte, Mato Grosso do Sul (que recebe bastantes venezuelanos, tendo uma rede de apoio local), dentre outros.

Roraima também possui um máximo de casos da doença na análise de População carcerária e isso reflete surtos da doença em presídios em RR em 2022, com mortes até 2023. A doença tem fácil disseminação e encontra na má nutrição dos presos e na densidade populacional, um ambiente propício. O gráfico acaba por imprimir uma realidade das cadeias do país, concentrando casos em uma escala que envolve Pernambuco (que tem uma das maiores cadeias), Rio Grande do Norte, Roraima, dentre outras.

Analisamos também os dados da população de rua e esses respondem, em média, por cerca de 5% dos casos de tuberculose, por estado. Contudo, estamos cientes da subnotificação dessa população, então como próximo passo gostaríamos de correlacionar esses dados com o gráfico de Tratamentos, para verificar se dentro dessa população há um alto percentual de abandono do tratamento, tendo em vista a situação de vulnerabilidade social extrema em que se encontram.

Destacamos também o mês de máxima de casos de tuberculose em cada estado e encontramos o mês de agosto como o mais recorrente. Esse mês no nosso clima corresponde ao final do inverno, o que pode indicar também uma dificuldade de identificação da doença que pode ser confundida com uma gripe forte devido às mudanças climáticas. No entanto, a demora na busca do tratamento leva a uma maior disseminação da doença, tendo em vista o contato da pessoa infectada com pessoas da família na mesma casa, ambiente de trabalho, dentre outros.

O Rio de Janeiro vive uma crise na saúde, sendo que o estado tem observado um crescente número de mortes pela doença. Em 2019 foram 659 óbitos; em 2020, 795, e em 2021, 805, de acordo com a Secretaria de Saúde do Rio de Janeiro. A pobreza tende a ampliar a disseminação da doença, com as péssimas condições sanitárias, muitas pessoas dividindo o mesmo espaço e a desnutrição, como já foi mencionado anteriormente. Por curiosidade, no Rio, em 6 de agosto, comemora-se o Dia Estadual de Conscientização, Mobilização e Combate à Tuberculose. O gráfico espelha a grande desigualdade social que vivemos no Brasil, e que o Rio de Janeiro tem se tornado símbolo, acompanhado de Rio Grande do Sul, Pernambuco,São Paulo, Pará, dentre outros.

Por fim, ao montar o gráfico de escolaridade, percebemos a importância de trazer os dados por estado para a análise, pois o total de casos por escolaridade obscurece as diferenças regionais que temos no país. Outro dado importante para a análise é o número total da população por estado para termos a compreensão do percentual que os casos de tuberculose representam sobre a população total. Desse modo, o gráfico de pizza, realizado em uma primeira etapa, não leva a conclusões válidas.

Realizamos também a análise da população de grávidas e os dados parecem indicar uma falta de acompanhamento do pré-natal que é realizado pelo SUS e até mesmo desconhecimento dos sintomas da tuberculose, tendo muitos casos da doença em estágios avançados da gravidez.

Por último, temos o gráfico de tratamento que possui um número alto de ?Reingresso após abandono?, mas o que chama mais atenção é o número de pessoas que responderam ?Não sabe? quanto à questão se já tiveram a doença ou não, o que leva mais uma vez à falta de informação da doença. Contudo, como informado anteriormente, esse gráfico será melhor analisado, tendo seus dados cruzados com outras variáveis.

Em 2022, o Brasil teve mais de 78 mil novos casos da doença segundo o Ministério de Saúde e, em 2021, teve recorde de mortes, mais de 5 mil, o maior número em 10 anos, tendo perdido apenas para a COVID. Ela é ainda a principal causa de morte de pessoas com AIDS. É importante destacar também que 48% das famílias afetadas de alguma forma pela tuberculose têm gastos com a doença que comprometem acima de 20% da renda, o que pode levar famílias inteiras à condição de pobreza e miséria. Desse modo, destaca-se a importância dos dados divulgados pelo Sinan, Sistema de Informação de Agravos de Notificação, do SUS, para que possamos fazer análises e criar dados para pautar políticas públicas que façam a diferença para o país.

Viva o SUS!
