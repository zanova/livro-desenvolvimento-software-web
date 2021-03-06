#Inserção de dados

Uma vez que o banco de dados tenha sido criado, é possível fazer operações sobre ele. A primeira operação que será apresentada aqui é a de inserção. Existem diferentes alternativas para realizar tal procedimento via PDO. Serão mostradas aqui apenas duas alternativas possíveis:

##Inserindo Dados: Alternativa I

Uma das maneiras de realizar as operações no banco de dados é diretamente através de instruções *SQL (Structured Query Language)*. Assim, define-se uma string contendo a instrução SQL com os respectivos parâmetros e logo em seguida executa-se esta instrução. 

Para realizar inserção via PDO com instrução SQL, a seguinte sintaxe pode ser utilizada:

```php
$conn = //abrir conexão com o banco de dados
$sql = //definir a instrução SQL utilizando a cláusula INSERT
$conn->exec($sql);
```

De acordo com o código apresentado anteriormente, primeiramente estabelece-se uma conexão com o banco de dados, logo em seguida pode ser definida uma instrução SQL que irá inserir um conjunto de dados em uma específica tabela e, por fim, basta executar a referida consulta a partir do método `exec`. O método `exec` retorna o número de linhas afetadas com a instrução SQL. Com o método `exec` é possível também executar instruções de **UPDATE** e **DELETE**, mas **não SELECT**(recuperação de dados - seção [Seleção de dados](recupera-dados.md)). 

##Inserindo Dados: Alternativa II

Outra alternativa para realizar operações de inserções é utilizar uma instância de `PDOStatement`,  que permite a criação de sentenças que possibilita a mesma (ou uma similar) instrução SQL ser executada várias vezes. 

Com o uso de um objeto `PDOStatement`, uma instrução SQL pode ter zero ou mais parâmetros que podem ser definidos de duas maneira distintas: através do seu próprio nome `“:nome_do_parâmetro”` ou através do caracter `“?”`. Os valores reais dos parâmetros são substituídos quando a instrução SQL é executada. 

**Sintáxe com parâmetros nomeados**
```php
$conn = //abrir conexão com o banco de dados
$sql = "INSERT INTO `nome_da_tabela`(`coluna1`, `coluna2`)
            VALUES (:coluna1, :coluna2)";
$stmt = $conn->prepare($sql);
$valor1=// valor da coluna 1
$valor2=// valor da coluna 2
$stmt->bindParam(":coluna1",$valor1);
$stmt->bindParam(":coluna2",$valor2);
$stmt->execute();
```
**Sintáxe com parâmetros definidos pela `“?”**`

```php
$conn = //abrir conexão com o banco de dados
$sql = "INSERT INTO `nome_da_tabela`(`coluna1`, `coluna2`)
            VALUES (?, ?)";
$stmt = $conn->prepare($sql);
$valor1=// valor da coluna 1
$valor2=// valor da coluna 2
$stmt->bindParam(1,$valor1);
$stmt->bindParam(2,$valor2);
$stmt->execute();
```

##Executando várias instruções com um mesmo objeto `PDOStatement`

Conforme mencionado anteriormente, um mesma instância de um objeto `PDOStatement` permite executar várias instruções SQL, conforme mostrado a seguir: 

```php
$conn = //abrir conexão com o banco de dados
$sql = "INSERT INTO `nome_da_tabela`(`coluna1`)
            VALUES (:coluna1)";
$stmt= $conn->prepare($sql);
$stmt->bindParam(":coluna1",$coluna1);

$coluna1=4;
$stmt->execute();

$coluna1=5;
$stmt->execute();
```

No código apresentado acima, é possível observar que uma mesma instrução SQL foi executada duas vezes, através da chamada ao método execute. Em cada execução, foi passado por parâmetro um valor diferente para o campo `“:coluna1”`, `4` e `5`. O método `bindParam`, que define o valor para o respectivo parâmetro, recebe a referência da variável, por isso que é possível apenas alterar o valor da variável passada por parâmetro, neste caso `$coluna1` e executar novamente a instrução SQL, através do método `execute()` do objeto `PDOStatement`. 



