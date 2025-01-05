## RESOLUÇÃO POR [pedrolporto](https://github.com/pedrolporto)
### © (copyright) Todos os direitos reservados

## Desafios SQL
### Moderado
#### 1. Listar os pacientes que realizaram mais de 5 consultas em 2025.

```bash
SELECT 
	p.id 
	,COUNT(c.id) as qtd_consultas 
FROM 
	pacientes p
	INNER JOIN consultas c  ON(p.id = c.paciente_id)
WHERE 
	strftime('%Y', c.data_consulta) = '2025'
GROUP BY 
	p.id 
HAVING 
	qtd_consultas > 5
```


### 2. Identificar os médicos que possuem mais de 3 especialidades diferentes registradas.

```bash
SELECT 
	crm 
	,COUNT(especialidade) as qtd
FROM 
	medicos m 
GROUP BY
	crm
HAVING 
	qtd > 3
ORDER BY 
	crm ASC
```
	
### 3. Encontrar os pacientes que receberam diagnósticos contendo a palavra "crônica".

```bash
SELECT DISTINCT 
	c.paciente_id 
FROM 
	consultas c
	INNER JOIN diagnosticos d ON(d.consulta_id = c.id)
WHERE 
	d.descricao LIKE '%crônica%'
ORDER BY 
	c.paciente_id ASC
```


### 4. Contar a quantidade total de prescrições feitas para cada medicamento.

```bash
	SELECT 
		medicamento_id 
		,COUNT(id) as qtd_prescricoes 
	FROM 
		prescricoes p 
	GROUP BY
		medicamento_id 
	ORDER BY
		medicamento_id ASC
```
	
### 5. Listar consultas realizadas entre 10:00 e 12:00 de qualquer dia.

```bash
SELECT 
	c.id 
FROM 
	consultas c 
WHERE 
	c.hora_consulta BETWEEN TIME('10:00') AND TIME('12:00')
ORDER BY 
	c.id ASC
```
	

-------
	
## Difícil
### 1. Identificar os médicos que realizaram o maior número de consultas em cada especialidade.

```bash
WITH query_base as (
	SELECT 
		especialidades.especialidade
		,dados.id_medico
		,dados.qtd_consultas
		,ROW_NUMBER() OVER ( PARTITION BY especialidades.especialidade ORDER BY dados.qtd_consultas DESC ) as linha
	FROM 
		(
			SELECT DISTINCT
				m_sub1.especialidade
			FROM
				medicos m_sub1
		) as especialidades
		INNER JOIN 
		(
			SELECT 
				m_sub2.id as id_medico
				,m_sub2.especialidade 
				,COUNT(c_sub.id) as qtd_consultas
			FROM 
				consultas c_sub 
				INNER JOIN medicos m_sub2 ON(m_sub2.id = c_sub.medico_id)
			GROUP BY 
				m_sub2.id 
				,m_sub2.especialidade
		) as dados ON(dados.especialidade = especialidades.especialidade)
	GROUP BY 
		especialidades.especialidade
		,dados.id_medico
		,dados.qtd_consultas
)
	
SELECT 
	*
FROM 
	query_base
WHERE 
	linha = 1
```

### 2. Encontrar os pacientes que não possuem consultas registradas no mês de janeiro de 2025.

```bash
SELECT
	p.id
FROM 
	pacientes p 
WHERE 
	NOT EXISTS 
		(
			SELECT
				1
			FROM
				consultas c
			WHERE
				p.id = c.paciente_id
				AND c.data_consulta BETWEEN DATE('01/01/2025') AND DATE('31/01/2025')
		)
```
	
### 3. Determinar qual medicamento foi mais prescrito no ano de 2025.

```bash
SELECT 
	p.medicamento_id 
	,COUNT(p.id) qtd_prescricoes
	,ROW_NUMBER() OVER ( ORDER BY COUNT(p.id) DESC) as ordem_absoluta
FROM 
	prescricoes p
	INNER JOIN diagnosticos d ON (d.id = p.diagnostico_id)
	INNER JOIN consultas c ON (c.id = d.id)
WHERE
	strftime('%Y', c.data_consulta) = '2025'
GROUP BY
	p.medicamento_id
ORDER BY
	qtd_prescricoes DESC
```
		
		
### 4. Listar os diagnósticos que possuem prescrições para mais de 3 medicamentos diferentes.

```bash
SELECT 
	ROW_NUMBER() OVER ( ORDER BY COUNT(p.medicamento_id) DESC) as ordem_absoluta
	,d.id as diagnostico_id 
	,COUNT(p.medicamento_id) as qtd_medicamentos
FROM 
	diagnosticos d 
	INNER JOIN prescricoes p ON(p.diagnostico_id = d.id)
GROUP BY 
	d.id
HAVING 
	qtd_medicamentos > 3
ORDER BY 
	ordem_absoluta ASC
```
	
	
### 5. Listar os pacientes que realizaram consultas com pelo menos 3 médicos diferentes em 2025.

```bash
SELECT 
	paciente_id 
	,COUNT(medico_id) as qtd_medicos_atendentes
FROM 
	consultas c 
WHERE 
	STRFTIME('%Y', c.data_consulta) = '2025'
GROUP BY 
	paciente_id 
HAVING 
	qtd_medicos_atendentes >= 3
ORDER BY 
	qtd_medicos_atendentes DESC
```
	
	
## Muito Difícil
### 1. Criar uma lista de diagnósticos agrupados por especialidade médica e quantidade de diagnósticos.

```bash
SELECT 
	m.especialidade 
	,d.descricao 
	,COUNT(d.id) as qtd_diagnosticos
FROM 
	diagnosticos d 
	INNER JOIN consultas c ON(c.id = d.consulta_id)
	INNER JOIN medicos m ON(m.id = c.medico_id)
GROUP BY 
	m.especialidade 
	,d.descricao 
ORDER BY 
	m.especialidade ASC
	,d.descricao ASC
```
	
	
### 2. Encontrar os pacientes que possuem mais de um diagnóstico relacionado a doenças crônicas.

```bash
SELECT
	c.paciente_id 
	,COUNT(DISTINCT d.descricao) as qtd_diagnosticos
FROM 
	consultas c 
	INNER JOIN diagnosticos d ON (d.consulta_id = c.id)
WHERE 
	d.descricao LIKE '%crônica%'
GROUP BY 
	c.paciente_id 
HAVING 
	qtd_diagnosticos > 1
```

### 3. Determinar o médico com a maior média de consultas realizadas por mês em 2025.

```bash	
WITH dados_medicos as (
	SELECT 
		c.medico_id 
		,STRFTIME('%m', c.data_consulta) as mes_numerico
		,COUNT(c.id) as qtd_consultas
	FROM 
		consultas c 
	WHERE
		STRFTIME('%Y', c.data_consulta) = '2025'
	GROUP BY 
		c.medico_id 
		,mes_numerico
)

SELECT 
	dm.medico_id
	,ROUND( AVG(dm.qtd_consultas), 0 ) as media_consultas_por_mes
FROM 
	dados_medicos as dm
GROUP BY
	dm.medico_id
ORDER BY 
	media_consultas_por_mes DESC
```

### 4. Identificar os diagnósticos que possuem mais de 5 prescrições para medicamentos diferentes e listar os medicamentos.

```bash	
SELECT 
	p.diagnostico_id 
	,COUNT(p.id) as qtd_prescricoes
	,COUNT(DISTINCT p.medicamento_id) as qtd_medicamentos
	,string_agg(m.nome,", " ORDER BY m.nome ASC)
FROM 
	prescricoes p 
	INNER JOIN medicamentos m ON(m.id  = p.medicamento_id)
GROUP BY
	p.diagnostico_id 
HAVING
	qtd_prescricoes > 5
	AND qtd_medicamentos > 5
```
	
### 5. Listar os pacientes que possuem diagnósticos em mais de 3 especialidades médicas diferentes.

```bash
SELECT 
	c.paciente_id 
	,COUNT(m.especialidade) as qtd_especialidades 
FROM 
	diagnosticos d 
	INNER JOIN consultas c ON (c.id = d.consulta_id)
	INNER JOIN medicos m ON (m.id = c.medico_id)
GROUP BY 
	c.paciente_id 
HAVING 	
	qtd_especialidades > 3
```
