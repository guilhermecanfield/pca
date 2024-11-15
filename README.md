# Análise de Componente Principais

É utilizada para diminuir as dimensões (colunas), quando estas forem métricas, pois a técnica se baseia no cálculo de correlação de Pearson e com ela conseguimos diminuir a quantidade de variáveis através da combinação linear destas em fatores.

O objetivo é analisar o comportamento conjunto das variáveis métricas para agrupá-las em fatores e assim reduzindo as dimensões do conjunto de dados.
Depois do agrupamento é possível verificar quais variáveis compõem cada componente e rankear os componente que tiveram maior demepenho o que torna a análise fatorial importante também para analytics.
Como o output da técnica de PCA é a criação de fatores ortogonais (sem correlação nenhuma entre si), estes são utilizados em modelos supervisionados para evitar problemas como a multicolinearidade (causada pela alta correlação entre variáveis).

### Implementação Teórica

O primeiro passo é criar a matriz de correlação de Pearson entre as variáveis originais para criação dos fatores. O próximo passo é o agrupamento das variáveis em fatores:
- Variáveis com coeficiente de correlação extremos (próximos de -1 e 1), irão compor o mesmo fator.
- Variáveis com coeficiente de correlação baixos (próximos de zero), irão compor fatores diferentes.

### Adequação Global da Análise
- Para que a análise fatorial seja viável, devem existir altos e estatísticamente significante valores de correlação entre as variáveis. 
- Para investigar a adequação global da análise fatorial, utiliza-se o teste de esferacidade de Barlett para determinar se os coeficientes de correlação de Pearson são estatísticamente diferentes de zero.

``Teste de Esferacidade de Barllet``
- Compara a matriz de correlações com a matriz identidade de mesma dimensão e espera-se que tais matrizes sejam diferentes para que a análise seja aplicável:

![alt text](png/image.png)

Obs.: *A matriz identidade nada mais é do que uma matriz onde todos os valores são zero, exceto os da diagonal, ou seja, se a matriz de correlação for estatísticamente igual a matriz identidade, significa que ela é estatísticamente igual a zero!*

- *Hipótese Nula: A matriz de correlações é ``igual`` a matriz identidade*
- *Hipótese Alternativa: A matriz de correlações é ``diferente`` a matriz identidade*

Se o p-value for menor que 0,05, rejeita-se a hipótese nula, a matriz é estatísticamente diferente da matriz identidade!

## Autovalores e Autovetores

### Matriz de Autovalores

A matriz de correlação de dimensões KxK possui K autovalores ( λ² ). Os autovalores indicam o percentual de variância compartilhada pelas variáveis originais para formação de cada fator.

Exemplo do resultado final dos autovalores:

|             | Autovalor | % Variância | % Variância Ac. |
|-------------|-----------|-------------|-----------------|
| λ² 1        | 2,518     | 63,0%       | 63,0%           |
| λ² 2        | 1,000     | 25,0%       | 88,0%           |
| λ² 3        | 0,298     | 7,4%        | 95,4%           |
| λ² 4        | 0,184     | 4,6%        | 100,0%          |
| **Total**   | **4,000** | **100%**    | **100,0%**      |


- Pode-se observar que a soma dos autovalores sempre resulta no número de variáveis.
- Através do resultado dos autovalores é possível determinar a importancia de cada fator criado.
- Cada fator vai conter um percentual de informação sobre as variáveis, no exemplo acima podemos observar que os 2 primeiros fatores trazem 88% da variância deste conjunto de dados.

### Matriz de Autovetores

| Autovetores | Autovetor 1 | Autovetor 2 | Autovetor 3 | Autovetor 4 |
|-------------|-------------|-------------|-------------|-------------|
| v1          | 0,5643     | 0,0071      | 0,8005      | 0,2018      |
| v2          | 0,5886     | 0,0486      | -0,2196     | -0,7765     |
| v3          | -0,0268    | 0,9987      | -0,0007     | 0,0424      |
| v4          | 0,5783     | -0,0101     | -0,5576     | 0,5954      |

*Cada valor da matriz de autovetor é calculado com base no seu autovalor, assim sendo, todos os valores do autovetor 1 são calculados com base no autovalor 1, e assim por diante.*

### Score Fatorial

| Scores    | Score Fatorial 1 | Score Fatorial 2 | Score Fatorial 3 | Score Fatorial 4 |
|-----------|------------------|------------------|------------------|------------------|
| finanças  | 0,356           | 0,007            | 1,467            | 0,471            |
| custos    | 0,371           | 0,049            | -0,402           | -1,811           |
| marketing | -0,017          | 0,999            | 0,099            | -0,001           |
| atuária   | 0,364           | -0,010           | -1,022           | 1,389            |

*Cada score é o resultado do seu autovetor divido pela raiz quadrada de seu respectivo autovalor. Ou seja, ele relaciona o fator com a variável original, assim sendo, ele representa o peso de cada variável em cada fator. No score fatorial 2, pode-se observar que apenas marketing está sendo bem representada, ou seja, o 2o fator representa quase que exclusivamente esta variável. E as outras 3 variáveis estão sendo bem representadas no fator 1.*

### Cargas Fatoriais

- Representam as correlações de Pearson entre os fatores e os variáveis originais.
- Pode ser interpretado como a importância de cada variável na constituição de cada fator.
- Quanto maior a carga fatorial da variável, mais o fator é influenciado por esta variável.

| Cargas Fatoriais | Fator 1 | Fator 2 | Fator 3 | Fator 4 |
|------------------|---------|---------|---------|---------|
| finanças         | 0,895   | 0,007   | 0,437   | 0,087   |
| custos           | 0,934   | 0,049   | -0,120  | -0,333  |
| marketing        | -0,042  | 0,999   | -0,0004 | 0,018   |
| atuária          | 0,918   | -0,010  | -0,304  | 0,255   |
| **Autovalores**  | **2,518** | **1,000** | **0,298** | **0,184** |

*Interpretando a tabela de cargas fatoriais:* 

- A correlação de finanças, custos e atuária com o fator 1 é, respectivamente, 0,895, 0,934 e 0,918.
- A correlação de marketing com o fator 2 é de 0,999.

**O Loading Plot é um gráfico de pontos que ajuda a observar a correlação das vairiáveis com 2 fatores**

![alt text](png/image-1.png)

## Escolha dos Fatores:

Apesar de ser possível estabelecer incialmente quantos fatores são desejados, é importante realizar uma análise por meio dos autovalores, pois estes indicam a variância compartilhada pelas variáveis originais para a formação de cada fator.

### Critério de Kaiser ou Critério da Raíz Latente

Fatores formados por autovalores menores que 1 podem não ter representatividade, nesses fatores você acaba tendo menos informação do que você teria em uma variável original. O critério de Kaiser ou da Raíz Latente, indica que sejam utilizados apenas fatores correspondentes a autovalores maiores que 1.