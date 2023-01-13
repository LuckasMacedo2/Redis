# Redis
Repositório com estudos sobre Redis
O Redis é um armazenamento de dados na estrutura chave-valor

# Comandos
	'''echo - cliente manda o echo para o servidor que retorna o que foi enviado para o cliente. Não salva a informação no redis
		echo "Hello World"'''
	set key value -> salva um registro key-value no redis. Salva um conjunto de dados
		set total 10
	get key -> recupara a informação salva com o set
		get total
	del key -> deleta o registro
		del total
	keys * -> retorna todas as chaves
	keys *palavra -> retorna todas as chaves especificadas pela palavra
		keys *resultado
	mset chave:valor chave:valor
		mset "jogo:20-05-2020:futebol:sao paulo x palmeiras:resultado" "2 x 1" "jogo:20-06-2020:futebol:sao paulo x fluminense:resultado" "3 x 2"  "jogo:20-07-2020:futebol:flamengo x fluminense:resultado" "2 x 2"
	caracter ? -> operação like, caracter coringa, limita-se a apenas um caractere
		keys jogo:20-??-2020*
	caracter [] -> limitar a consulta, valores entre colchetes formam o ou
		keys "resultado:1[27]-09-2020:megasena" ** Não forma o número 27, mas sim 2 e 7.
	hset chave hash valor -> armazenar dados em forma de tabela hash
		hset "jogo:20-07-2020:futebol:flamengo x fluminense" biblheteria 200000
		hset "jogo:20-07-2020:futebol:flamengo x fluminense" pagantes 1540
		hset "jogo:20-07-2020:futebol:flamengo x fluminense" seguranca-particular 25
	hget chave chave hash -> retorna o valor da tabela hash definida
		hget "jogo:20-07-2020:futebol:flamengo x fluminense" biblheteria
		hget "jogo:20-07-2020:futebol:flamengo x fluminense" pagantes
	hkeys chave -> retorna as chaves daquele dicionário	
		hkeys "jogo:20-07-2020:futebol:flamengo x fluminense"
	hdel chave hash -> deleta o valor definido pela chave e hash
		hdel "jogo:20-07-2020:futebol:flamengo x fluminense" seguranca-particular
	hgetall chave -> retorna as chaves e valores do dicionário de dados
		 hgetall "jogo:20-07-2020:futebol:flamengo x fluminense"
	expire chave valorEmSegundos -> Configurar o tempo de expiração da chave em segundos
		expire sessao:usuario:1020 1800
	ttl chave -> a quantidade de tempo em segundos até a chave expirar. ttl = time to live, tempo de vida
		ttl sessao:usuario:1020
	incr chave -> incrementa a chave atômicamente
		incr meusite:fotos:08-09-2020
	decr chave -> decrementa a chave atômicamente
		decr meusite:fotos:08-09-2020
	incrby chave valorASerIncrementado -> incrementa o valor atomicamente na chave
		incrby meusite:fotos:08-09-2020 15			(positivo ou negativo)
	decrby chave valorASerIncrementado -> decrementa o valor atomicamente na chave
		decrby meusite:fotos:08-09-2020 15			
		O decrby não consegui remover valores com pontos flutuantes
	incrbyfloat chave valorASerIncrementadoComPontoFlutuante -> incrementa o valor com ponto flutuante atomicamente na chave
		incrbyfloat meusite:fotos:08-09-2020 11.23	(positivo ou negativo)
	setbit chave valor -> Salva um valor de um bit, um booleano
		setbit click:22-10-2021 10 1
	getbit chave -> recupera o valor em bit
		getbit click:22-10-2021 10
	bitcount chave -> conta os 1s da chave
		bitcount click:22-10-2021
	bitop and chaveDaInformacaoCalculada chave -> Operacao and
		bitop and click-sorteio-marketing-22-e-23 click:22-10-2021 click:23-10-2021
	bitop or chaveDaInformacaoCalculada chave -> Operacao and
		bitop or click-sorteio-marketing-22-ou-23 click:22-10-2021 click:23-10-2021
	lpush chave elementos -> Inserir elemento(s) na fila. Uma fila do tipo FIFO
		lpush ultimas-noticias "Restaurante paga bônus"
	llen chave -> retorna o tamanho da fila definida pela chave
		llen ultimas-noticias
	lindex chave indice -> Obter o registro definido pela chave e pelo indice
		lindex ultimas-noticias 0
	lrange chave inicio fim -> Obtém registros da fila definido em range
		lrange ultimas-noticias 1 2
	ltrim chave inicio fim -> Deixa somente os elementos definidos no intervalo entre inicio e fim
		ltrim ultimas-noticias 0 4
	ltrim chave inicio fim -> Deixa somente os elementos definidos no intervalo entre inicio e fim
		ltrim ultimas-noticias 0 4
	rpush -> Fila em que os itens são sempre adicionados no início
		rpush fila:caixa 01
	lpop chave -> pega e remove o dado mais a esquerda
		lpop fila:caixa
	blpop chave tempoEspera -> busy waiting, espere de forma ocupada, de forma que o redis fique parada. Não se perde nada, ganha-se consumo de rede, processamento ... Necessário passar o tempo de espera
		blpop	fila:caixa 60
	SADD nome valor -> cria um conjunto
		SADD relacionamentos:lucas daniela
		SADD relacionamentos:lucas sophia vitor	
	SCARD chave -> scardinalidade, quantidade de elementos no conjunto
		SCARD relacionamentos:lucas
	SMEMBERS chave -> Quais os membros do conjunto
		SMEMBERS relacionamentos:lucas
	SISMEMBER chave elemento -> verificar se um item está dentro do conjunto
		SISMEMBER relacionamentos:lucas jose
	SREM chave elemento -> remove o elemento do conjunto
		SREM relacionamentos:daniela joana
	SINTER chaves -> interseccao entre dois conjuntos
		SINTER relacionamentos:daniela relacionamentos:lucas
	SDIFF chaves -> diferença entre os conjuntos, a ordem dos conjuntos altera a resposta
		SDIFF relacionamentos:lucas relacionamentos:daniela
	type chave -> define o tipo do dado
		type jogador:01
	zadd chave valor -> cria um conjunto (onde cada item será único)
		zadd pontos 100 Lucas
	zrem chave valor -> deleta os dados
		zrem pontos Arroz
	zcard chave -> conta a quantidade de elementos do conjunto
		zcard pontos	
	zscore chave valor -> retorna o valor definido pela chave e valor
		zscore pontos Lucas
	zincrby chave valorASerIncrementado valor -> incrementa o valor com o valor definido em valorASerIncrementado
		zincrby pontos 50 Lucas
	zcount chave valorMinimo valorMaximo -> conta a quantidade de elementos entre os limites definidos
		zcount pontos 50 2500
	zrange chave posInicial posFinal -> Obtém os elementos entre posInicial e posFinal do maior para o menor
		zrange pontos 0 3
	zrevrange chave posInicial posFinal -> Obtém os elementos entre posInicial e posFinal do menor para o maior
		zrevrange pontos 0 3
	zpopmax chave -> remove o item com mais rank
		zpopmax pontos
	zpopmin chave -> remove o item com menos rank
		zpopmin pontos

# Livros sobre Redis
	Redis Essentials
	Redis casa do código
	Mastering Redis
	Redis in Action

docker run -d --name redis-stack-server -p 6379:6379 redis/redis-stack-server:latest
****************************************************************************************************************
