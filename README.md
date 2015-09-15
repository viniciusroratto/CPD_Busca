# CPD_Busca
Sistema de busca em arquivo CSV implementado em Python.

Classificação e Pesquisada de Dados
Trabalho Final da Disciplina

Denyson Jürgen, Felipe Fuhr, Vinícius Roratto

Problema: 
O programa deve receber um arquivo CSV e, a partir dele, desenvolver um catálogo/indexador de publicações. O objetivo é criar o catálogo em um arquivo próprio, binário,  independente do original.

Linguagem:
Para abordar o problema, o grupo escolheu desenvolver o programa na linguagem Python, em sua versão 3.4. Os motivos da utilização do Python foram: aprender uma nova linguagem, a facilidade de encontrar, não apenas documentação, mas a resolução de dúvidas que outros usuários já tiveram (a língua é muito popular e a comunidade em volta dela é muito voltada à colaboração) e as variadas estruturas de dados disponibilizadas. Além disso, o fato da linguagem ser de alto nível ajuda bastante a leitura do código, principalmente quando temos um grupo com três alunos com diferentes conhecimentos e capacidades de programação.

Bibliotecas:
A bliblioteca CSV foi utilizada para facilitar a leitura do arquivo. A principal função utilizada desta biblioteca é a .reader() e é descrita na documentação oficial da linguagem como ideal para buscar linhas do arquivo através de iterações. 
A biblioteca RE foi utilizada para as funções .sub() e split(), ideais para manipulação de strings. A .split() divide uma string em uma lista a partir de um símbolo específico (usamos ‘,’ para o arquivo do trabalho).
A biblioteca Hashlib foi aproveitada para o uso das funções .md5, que constrói hashes, a partir destas delas, transformaremos chaves em códigos binários que são utilizados para a criação das estruturas de árvores Trie, essenciais para a organização do catálogo. A função hexdigest, mostra o código gerado pela hash em hexadecimal. Esse código é transformado em binário e utilizado para facilitação da construção da árvore Trie.
Outra biblioteca utilizada é a Pickle, que serve para transformar objetos em séries de bytes ou uma série de bytes em um objeto. As funções incluídas desta biblioteca são .dump(), que escreve objetos em um arquivo, e load, que carrega um objeto de um arquivo.
A última biblioteca presente foi criada pelos próprios autores, para organizar um pouco a visualização do programa, separando algumas funções de organização da estrutura de dados das funções de interface com o usuário.

Estruturas de dados:
O programa utiliza árvores Trie, que são ideais para busca de cadeias de caracteres. Como a maioria das buscas nos catálogos são feitas a partir de strings, uma árvore de prefixos é uma opção bastante rápida que ocupa espaço reduzido.
Para facilitar nosso trabalho, foi implementado um algoritmo que, através de um processo de hashing, transforma o código de letras em uma string de números binários. Isso facilita a implementação do algoritmo que cria a árvore, aproximando-a de uma ADS (Árvore Digital Binária). O ideal seria associar a árvore a funções que ajudam a reduzir ainda mais o caminho da busca, transformando-a em uma árvore Patrícia. Nas folhas de cada árvore se encontram os offsets com os endereços de todas as repetições de cada palavra em um arquivo serial.

Organização de arquivos:
Um dos diferenciais do programa é o tratamento que dá aos arquivos. Inicialmente, pede o nome do arquivo a ser processado. Se este arquivo base já foi processado anteriormente, ele não será processado novamente, pois o programa mantém salvas, em arquivos binários, as árvores onde serão feitas as pesquisas. 
Se o arquivo nunca foi processado no programa, é criado um arquivo binário serial com os dados organizados, mas sem sequência definida por chaves. Para evitar que o tempo de processamento de arquivos seja muito grande, e evitar que se percorra o arquivo inteiro a cada busca foi usado o seguinte processo:
Para cada tipo de busca pré definida pelo edital do trabalho, foi criado um arquivo específico que contém uma árvore e os offsets de cada chave. Então, ao invés de percorrer o arquivo serial com todos os dados, o arquivo percorre árvores com offsets em suas folhas. Toda vez que a busca encontra uma chave, encontra também os endereços dos dados no arquivo binários. Esse tipo de organização pode ser definida como arquivos indexados invertidos. 

Contribuição:
O trabalho se destaca na utilização da combinação de múltiplas técnicas de organização e pesquisa de dados para desenvolver um catálogo de publicações com respostas eficientes. Ao invés de utilizar apenas uma técnica, o objetivo do grupo foi tentar avaliar qual algoritmos se encaixava melhor para cada problema. Um exemplo foi o momento de organização (alfabética) dos arquivos antes da impressão: em um primeiro momento, se são menos de 15 elementos para ordenação, o programa ordena os dados através de um insertion sort, algoritmo fácil e recomendado para pequenas ordenações. Em caso de ordenações maiores, usa-se o “Tim Sort”, algoritmo que se aproxima do mergesort, e já está implementado nas bibliotecas básicas do python com a função Sorted.
O uso das estruturas foi pensado assim: 
Como cada arquivo é processado apenas uma vez, é aceitável perder algum tempo na primeira execução, desde que as buscas sejam rápidas. Então, o que se fez foi criar todos os arquivos de dados e índices (árvores) na primeira vez que o arquivo é carregado no programa, depois disso o programa usa as árvores para buscas, perdendo um tempo no início, mas compensando isso com um funcionamento rápido em pesquisas.

Manual de Uso:
O programa funciona em modo texto, e será apresentado do notebook de um aluno, na verdade só é necessário ter o python 3.4 instalado no computador para uso. 
Após a compilação no python IDLE, 
O programa inicia pedindo um nome para o arquivo de texto que será processado.
Depois ele pede um nome para o arquivo de texto de saída (para melhor visualização dos resultados, exibimos a saída em tempo real, mas também gravamos ela em um arquivo de texto).
O processamento ocorre.
Um menu com 6 opções é apresentado ao usuário:

	4.1 Se o usuário apertar a tecla “1” do teclado e, pressionar “Enter”, será pedido a ele o nome do autor que ele está buscando. Após digitado o nome do autor, serão exibidas todas as publicações do autor pedido. Tudo que foi exibido também será impresso no arquivo de saída. 
	4.2 Se o usuário apertar “2”, será pedido a ele o nome de um autor e, depois, através de um menu “Sim ou Não” será dada a opção de incluir mais autores, o que pode ser feito sucessivamente, com um “s”, até que o usuário selecione um “N”. Após essa seleção, será feita um cruzamento entre todos os artigos de cada autor e a intersecção entre eles será impressa. Tudo que é exibido também é enviado ao arquivo de saída.
	4.3 Ao apertar “3” o programa pede uma string e busca por ela em todos os títulos.
	4.4 A opção “4” lista todos os autores de uma área pedida pelo usuário.
	4.5 A opção “5” mostra as publicações em um único ano.
	4.6 A opção “0” fecha o arquivo de saída e o programa.
	
	5. Até que seja pressionado “0”, o programa volta para o menu depois de cada execução de uma das opções.

Considerações finais:
Muitos conceitos apresentados na disciplina de “CPD” foram importantes para o desenvolvimento deste catálogo. Inicialmente, a importância de avaliação e reconhecimento de algoritmos de ordenação nos ajudou na parte final do projeto, quase na impressão dos dados. A capacidade de entender para que situação cada algoritmo é melhor utilizado, além de tentar prever a escalabilidade e estabilidade de um programa ajudou o grupo a fazer escolhas melhor embasadas.
O conhecimento de organização de arquivos foi fundamental, pois nos ajudou a implementar a parte mais importante do trabalho: os diferentes índices que agilizam a navegação do programa, com ajuda das árvores, no arquivo serial de dados.
Por fim, os métodos de pesquisa também são de grande importância, uma vez que possibilitam buscas rápidas em grandes quantidades de dados, característica que pode ser vital, dependendo da quantidade de informações presentes no arquivo CSV. É importante lembrar que a disciplina também apresentou métodos de compressão de dados que não foram utilizados neste trabalho, mas que certamente serão aplicados em futuras disciplinas.
*As informações utilizadas neste relatório foram retiradas da documentação do Python 3.4 e dos slides de aula.
