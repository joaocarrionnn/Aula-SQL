# Resumo dos Seminários de SQL

## Introdução
Este documento oferece uma síntese dos principais temas abordados nos seminários de SQL, explorando como suas funcionalidades contribuem para a manipulação eficiente de dados. O objetivo é fornecer um entendimento claro sobre operadores, joins, subconsultas, funções de agregação, indexação e controle de transações, além de abordar Stored Procedures e Triggers. Esses conceitos são fundamentais para a otimização de consultas, melhorando a performance e garantindo a confiabilidade no gerenciamento de bancos de dados.

## Seções por Tema

### Operadores de Agregação
Os operadores de agregação permitem realizar cálculos em conjuntos de dados, facilitando a análise e resumo de informações. São essenciais para consultas que envolvem soma, contagem, média, e identificação de valores máximos ou mínimos.

| Operador | Descrição                                      |
|----------|------------------------------------------------|
| **SUM**  | Retorna a soma dos valores de uma coluna numérica. |
| **AVG**  | Calcula a média dos valores.                   |
| **COUNT**| Conta o número de registros.                   |
| **MIN**  | Identifica o menor valor de uma coluna.       |
| **MAX**  | Identifica o maior valor de uma coluna.       |

**Exemplo**:
```sql
SELECT departamento, SUM(salario), AVG(salario) FROM funcionarios GROUP BY departamento;
```
Esse exemplo retorna o total e a média salarial por departamento.

### Joins em SQL
Os `JOINs` combinam dados de duas ou mais tabelas com base em condições definidas. São usados para relacionar informações de diferentes tabelas e permitem criar consultas complexas que envolvem múltiplas fontes de dados.

| Tipo de Join       | Descrição                                          |
|--------------------|--------------------------------------------------|
| **INNER JOIN**     | Retorna registros com correspondência em ambas as tabelas. |
| **LEFT JOIN**      | Retorna todos os registros da tabela à esquerda e os correspondentes da direita. |
| **RIGHT JOIN**     | Retorna todos os registros da tabela à direita e os correspondentes da esquerda. |
| **FULL OUTER JOIN**| Combina registros de ambas as tabelas, mesmo que não haja correspondência. |

**Exemplo**:
```sql
SELECT clientes.nome, pedidos.numero_pedido FROM clientes 
LEFT JOIN pedidos ON clientes.id = pedidos.cliente_id;
```

### Operadores Lógicos
Os operadores lógicos são usados para refinar consultas com múltiplas condições.

| Operador | Descrição                          |
|----------|------------------------------------|
| **AND**  | Ambas as condições devem ser verdadeiras. |
| **OR**   | Pelo menos uma das condições deve ser verdadeira. |
| **NOT**  | Inverte o resultado de uma condição. |

**Exemplo**:
```sql
SELECT * FROM produtos WHERE preco > 100 AND estoque > 10;
```
Essa consulta retorna produtos cujo preço é superior a 100 e que têm mais de 10 unidades em estoque.

### Subconsultas
Subconsultas são consultas dentro de outra consulta principal. Elas são utilizadas para resolver problemas que envolvem consultas intermediárias e podem ser correlacionadas ou não correlacionadas.

| Tipo de Subconsulta      | Descrição                                      |
|--------------------------|-----------------------------------------------|
| **Não Correlacionada**   | Executada uma vez; resultados usados na consulta externa. |
| **Correlacionada**       | Depende dos valores da consulta principal; executada para cada linha da consulta externa. |

**Exemplo**:
```sql
SELECT nome FROM produtos WHERE preco > (SELECT AVG(preco) FROM produtos);
```
Retorna produtos cujo preço é maior que a média.

### Funções de Agregação com GROUP BY
O `GROUP BY` permite agrupar resultados com base em valores de uma ou mais colunas e aplicar funções de agregação.

| Comando | Descrição                                      |
|---------|------------------------------------------------|
| **GROUP BY** | Agrupa os dados por uma coluna específica.   |
| **HAVING**   | Filtra grupos após a agregação.              |

**Exemplo**:
```sql
SELECT departamento, COUNT(*), SUM(salario) FROM funcionarios GROUP BY departamento HAVING COUNT(*) > 5;
```
Retorna departamentos com mais de 5 funcionários e a soma de seus salários.

### Indexação e Performance
Índices são essenciais para melhorar a performance das consultas em grandes conjuntos de dados. Eles permitem buscas mais rápidas, especialmente em colunas frequentemente usadas em condições de pesquisa.

| Aspecto                   | Descrição                                      |
|---------------------------|------------------------------------------------|
| **Importância da Indexação** | Reduz o tempo de busca em tabelas grandes. |
| **Quando Usar**          | Use índices em colunas frequentemente usadas em cláusulas `WHERE` e `JOIN`. |

**Exemplo**:
```sql
CREATE INDEX idx_nome_produto ON produtos(nome);
```
Isso cria um índice na coluna `nome` da tabela `produtos`.

### Transações e Controle de Confiabilidade
As transações garantem que uma série de operações seja executada com sucesso, ou que sejam completamente revertidas em caso de erro. Elas são cruciais para garantir a integridade e confiabilidade dos dados.

| Comando    | Descrição                                     |
|------------|-----------------------------------------------|
| **COMMIT** | Finaliza a transação, aplicando as alterações. |
| **ROLLBACK** | Reverte a transação, desfazendo alterações. |

**Exemplo**:
```sql
BEGIN;
UPDATE contas SET saldo = saldo - 100 WHERE id = 1;
UPDATE contas SET saldo = saldo + 100 WHERE id = 2;
COMMIT;
```

### Stored Procedures e Triggers
- **Stored Procedures**: São blocos de código SQL armazenados no banco de dados, que podem ser executados sob demanda para realizar tarefas repetitivas.

**Exemplo**:
```sql
CREATE PROCEDURE atualiza_estoque() 
BEGIN
    UPDATE produtos SET estoque = estoque - 1 WHERE id_produto = 10;
END;
```

- **Triggers**: São mecanismos automáticos que disparam uma ação específica quando certos eventos ocorrem no banco de dados, como uma inserção ou atualização.

**Exemplo**:
```sql
CREATE TRIGGER atualiza_data_modificacao BEFORE UPDATE ON produtos
FOR EACH ROW
SET NEW.data_modificacao = NOW();
```

## Conclusão
Neste documento, foram explorados diversos aspectos essenciais de SQL, como operadores de agregação, joins, operadores lógicos, subconsultas, funções de agregação com `GROUP BY`, indexação, controle de transações, e stored procedures e triggers. A inter-relação entre esses conceitos é crucial para o desenvolvimento de sistemas de banco de dados eficientes, otimizando tanto a performance quanto a confiabilidade das operações. Entender esses elementos facilita o gerenciamento de dados e a implementação de soluções robustas em ambientes corporativos.