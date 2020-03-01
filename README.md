# SeHablaSQL-Mediana
Calcular mediana en SQL Server

Este es un código que funciona (con un rendimiento algo cuestionable por la condición del Order by...) pero si te sirve! esta genial...


/*** Sacando la media en Transact con SQL 2012 y superiores ***/

/* Variables para el OFFSET */

DECLARE @StarMedian INT, @EndMedian INT;

/* Llenamos las variables */

SELECT @StarMedian = IIF(COUNT(c)%2=1, FLOOR(COUNT(c)/2), COUNT(c)/2-1)
	 , @EndMedian = IIF(COUNT(c)%2=1, 1, 2)
FROM (VALUES(1),(7),(8),(9))AS t(c)
SELECT @StarMedian, @EndMedian

/* Ahora el cálculo */

;WITH Valores AS (
	SELECT 1.0 * (c) AS ValoresMediana /*Para tener la fracción le hago casting*/
	FROM (SELECT DebitoDolar FROM ContabilidadETLUtil.Transacciones)AS t(c)
	ORDER BY 1 
	OFFSET @StarMedian ROWS
	FETCH NEXT @EndMedian ROWS ONLY
)SELECT AVG(ValoresMediana) /* Siempre un promedio, no importa si recibe uno o dos valores */
FROM Valores;
