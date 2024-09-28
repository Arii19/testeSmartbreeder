# testeSmartbreeder
Respostas do teste 

# Respostas do Questionário SQL - Ariane Rodrigues

## Perguntas e Respostas

**1. O que é um `INNER JOIN` e como ele difere de um `LEFT JOIN`?**

R: O `INNER JOIN` retorna apenas as linhas em que há uma correspondência em ambas as tabelas. Se não houver correspondência em uma das tabelas, a linha é excluída do resultado. Já o `LEFT JOIN` retorna todas as linhas da tabela à esquerda, mesmo que não haja correspondência na tabela à direita. Quando não há correspondência, os valores da tabela à direita são nulos.


**2. Como você otimiza o desempenho de uma consulta que usa um `JOIN` em grandes conjuntos de dados?**

R: Usando índices apropriados nas colunas envolvidas (geralmente as chaves estrangeiras e primárias), pois o índice ajuda o banco de dados a localizar as linhas que precisam ser combinadas de forma mais rápida. Além disso, o planejamento adequado da consulta é crucial.


**3. Explique a diferença entre um `FULL JOIN` e um `CROSS JOIN`.**

R: A principal diferença entre o `FULL JOIN` e o `CROSS JOIN` é que o `FULL JOIN` combina as linhas de ambas as tabelas buscando correspondência entre elas, enquanto o `CROSS JOIN` combina todas as linhas entre as tabelas envolvidas, sem considerar a correspondência ou a integridade das relações.


**4. Dado o seguinte esquema de tabelas, como você escreveria uma consulta para encontrar todos os clientes que não têm pedidos?**

R:  **(Faltou o esquema de tabelas para responder a esta pergunta.)**


**5. Quais são os riscos de usar um `CROSS JOIN` sem uma cláusula condicional?**

R: Usar `CROSS JOIN` sem uma cláusula condicional pode causar explosão de dados, alto consumo de recursos, resultados irrelevantes e lentidão, impactando negativamente o desempenho do banco de dados, pois produz todas as combinações possíveis de registros de todas as tabelas.


**6. Qual é a principal vantagem de usar o comando `MERGE` em relação a executar instruções `INSERT`, `UPDATE` e `DELETE` separadamente?**

R: O `MERGE` permite executar essas três operações em uma só instrução, simplificando o código, melhorando o desempenho e garantindo a consistência dos dados.


**7. Dado o seguinte cenário, como você escreveria um comando `MERGE` para atualizar os registros de uma tabela `Estoque` com base em uma tabela `NovosDados`?**

Tabela `Estoque` (`id_produto`, `quantidade`)
Tabela `NovosDados` (`id_produto`, `quantidade_nova`)

R: 
```sql
MERGE INTO Estoque AS T
USING NovosDados AS S
ON (T.id_produto = S.id_produto)
WHEN MATCHED THEN
    UPDATE SET T.quantidade = S.quantidade_nova
WHEN NOT MATCHED THEN
    INSERT (id_produto, quantidade)
    VALUES (S.id_produto, S.quantidade_nova);
```


**8. Qual é o efeito da cláusula `WHEN NOT MATCHED BY SOURCE` em um comando `MERGE`?**

R: Ela define o que acontece com os registros presentes na tabela de destino, mas ausentes na tabela de origem. Basicamente, permite excluir ou atualizar esses registros "não correspondentes" na tabela de destino. Mantendo a integridade dos dados.

**9. Quais são as possíveis desvantagens de usar `MERGE` em um ambiente de alta concorrência?**

R: As desvantagens do uso de `MERGE` em um ambiente de alta concorrência incluem bloqueios, deadlocks, desempenho variável, complexidade de diagnóstico, aumento do log de transações e consumo elevado de recursos, que podem impactar a eficiência e a capacidade de resposta do sistema.

**10. Como você garantiria a integridade dos dados ao usar um comando `MERGE` que inclui uma operação de `DELETE`?**

R: Utilizaria a cláusula `WHEN NOT MATCHED BY SOURCE` para excluir apenas registros que realmente não têm correspondência na tabela de origem. Outra forma seria executar o comando `MERGE` dentro de uma transação, porque se algo der errado durante a execução, é possível reverter todas as operações.

**11. O que é um cursor e qual é sua principal finalidade em um banco de dados?**

R: Um cursor é um mecanismo que permite ao usuário processar resultados de consultas SQL de forma linha a linha, um cursor fornece um controle mais granular sobre cada linha do conjunto de resultados.

**12. Quais são as etapas básicas para usar um cursor em SQL?**

R: 
Declarar o cursor: 
```sql
DECLARE employee_cursor CURSOR FOR
SELECT employee_id, employee_name
FROM Employees;
```
Abrir o cursor:
  ``` sql
  OPEN employee_cursor;
  ```
Buscar os dados:
``` sql
FETCH NEXT FROM employee_cursor INTO @id, @name;
```
Processar dados: Lógica de processamento, como exibir os dados:
``` sql
PRINT 'ID: ' + CAST(@id AS VARCHAR) + ', Nome: ' + @name;
```
Passar pelas linhas:
``` sql
WHILE @@FETCH_STATUS = 0
BEGIN
    -- Processar os dados
    FETCH NEXT FROM employee_cursor INTO @id, @name;
END
```
Fechar o cursor:
``` sql
CLOSE employee_cursor;
```
Desalocar o cursor:
``` sql
DEALLOCATE employee_cursor;
```
**13. Qual é a diferença entre um cursor implícito e um cursor explícito?**

R: Cursor Implícito é criado automaticamen te pelo SGBD, sem interação direta do usuário, ideal
para consultas simples, já o Cursor Explícito é criado e controlado pelo usuário, oferecendo maior
flexibilidade e controle sobre a manipulação de dados, mas com maior complexidade.

**14. Por que o uso de cursores pode afetar o desempenho de uma consulta?**

R: Porque a operação é feita linha a linha.

**15. Como você garantiria que um cursor seja fechado e desalocado corretamente após o uso?**

R: Primeiro organizaria o código utilizando o `BEGIN … END`, depois chamaria o comando
`CLOSE`, agora tem que implementar o tratamento de erros e logo após fechar o cursor usando o
comando `DEALLOCATE` para liberar completamente a memória associada a ele.

**16. Dado o seguinte código SQL, identifique e explique um possível problema com o uso do cursor:**

![questão 16](https://github.com/Arii19/testeSmartbreeder/blob/ba155111a6c5d7b1e2b7e938ef8dfddbf13bbf95/16.png)

R: Um possível problema que o uso do cursor traria para esse código, é a lentidão e a ineficiência ,
pois ele processa cada registro da tabela Employees individualmente , o qu e pode ser bem lento ,
ainda mais se a tabela tiver muitos registros. Uma alternativa para esse caso seri a as operações em
conjunto, como um `SELECT` simples. 

**17. Como você pode evitar o uso de cursores ao manipular grandes conjuntos de dados e quais são as alternativas?**
R:Utilizando operações baseadas em conjunto.

**18. Explique oque significa o comando FETCH NEXT FROM cursor_name INTO @variable1, @variable2 e como ele é usado em um loop de cursor:**

R: É usado para recuperar a próxima linha de dados de um cursor e armazenar os valores das
colunas dessa linha em variáveis.

**19. Quais são os diferentes tipos de cursores que você pode encontrar em um SGBD e como eles diferem entre si?**

R: Cursor Implícito: Criado automaticamente pelo sistema de banco de dados quando você faz uma
consulta SELECT. Você não precisa se preocupar com ele, pois o sistema cuida de tudo. Curs or
Explícito: Criado e controlado pelo usuário. Você usa comandos específicos para criar e gerenciar
esse cursor, o que te dá mais controle sobre como os dados são recuperados e manipulados.

**20. Dado o seguinte cenário, qual seria uma alternativa ao uso de cursores para atualizar uma tabela com base em outra tabela?**

![questão 20](https://github.com/Arii19/testeSmartbreeder/blob/ba155111a6c5d7b1e2b7e938ef8dfddbf13bbf95/20.png)

R: Utilizar o comando UPDATE com um JOIN , porque assim é executada como uma única
operação em conjunto , otimizada pelo SQL Server para processar várias linhas ao mesmo tempo.
