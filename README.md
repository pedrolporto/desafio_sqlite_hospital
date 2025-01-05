
# Desafio de SQL para Gestão Hospitalar

## Visão Geral

Este repositório apresenta um desafio abrangente de SQL inspirado nas operações em um **Hospital**. Ele foi projetado para avaliar e demonstrar habilidades avançadas em SQL usando um conjunto de dados robusto. O banco de dados simula cenários reais em um ambiente hospitalar, incluindo pacientes, médicos, consultas, diagnósticos, prescrições e medicamentos.

## Objetivos

Este projeto é ideal para recrutadores, gestores de contratação e entusiastas de SQL que desejam avaliar competências em:

- Design e normalização de banco de dados.
- Construção e otimização de consultas complexas.
- Raciocínio analítico com dados do mundo real.
- Proficiência em SQL em diferentes níveis de dificuldade.

## Estrutura do Banco de Dados

O banco de dados é composto por seis tabelas interconectadas:

1. **`pacientes`**: Armazena informações dos pacientes, como nome, data de nascimento, gênero e contato.
2. **`medicos`**: Contém detalhes sobre os médicos, incluindo especialidades e números de registro.
3. **`consultas`**: Registra consultas, ligando pacientes e médicos com data e hora.
4. **`diagnosticos`**: Captura diagnósticos para cada consulta.
5. **`medicamentos`**: Lista os medicamentos disponíveis na farmácia do hospital.
6. **`prescricoes`**: Rastreia prescrições, ligando diagnósticos a medicamentos.

## População de Dados

O banco de dados inclui **registros gerados aleatóriamente**, com relações de dados realistas. O conjunto de dados foi cuidadosamente ajustado para garantir relevância e fornecer resultados significativos para todos os desafios.

## Desafios

### Moderado

1. Listar os pacientes que realizaram mais de 5 consultas em 2025.
2. Identificar os médicos com mais de 3 especialidades.
3. Encontrar pacientes diagnosticados com condições "crônicas".
4. Contar o total de prescrições feitas para cada medicamento.
5. Listar consultas realizadas entre 10:00 e 12:00.

### Difícil

1. Determinar o médico com o maior número de consultas em cada especialidade.
2. Identificar pacientes sem consultas registradas em janeiro de 2025.
3. Encontrar o medicamento mais prescrito em 2025.
4. Listar diagnósticos associados a mais de 3 medicamentos.
5. Identificar pacientes que consultaram pelo menos 3 médicos diferentes em 2025.

### Muito Difícil

1. Agrupar diagnósticos por especialidade médica e contar.
2. Identificar pacientes com múltiplos diagnósticos de condições "crônicas".
3. Determinar o médico com maior média mensal de consultas em 2025.
4. Encontrar diagnósticos associados a mais de 5 prescrições e listar os medicamentos.
5. Identificar pacientes com diagnósticos em mais de 3 especialidades.

## Como Usar

1. Clone este repositório.
2. Use o script SQL fornecido (`Hospital_SQLite.txt`) para configurar o banco de dados em um ambiente SQLite.
3. Execute os desafios usando as consultas fornecidas ou crie suas próprias soluções.
4. Avalie seu desempenho em SQL com base nos resultados dos desafios.

## Resolução do Autor
1. Acesse a resolução do autor neste  [Link](https://github.com/pedrolporto/desafio_sqlite_hospital/blob/main/Resolucao_Desafio_pedrolporto.md) 

## Por Que Este Projeto?

Este desafio simula a complexidade de sistemas de banco de dados do mundo real, fornecendo insights sobre habilidades de resolução de problemas, proficiência em SQL e raciocínio analítico. Ele é ideal para:

- **Recrutadores**: Avaliar a proficiência em SQL durante processos de seleção.
- **Desenvolvedores**: Aperfeiçoar habilidades de SQL com exemplos práticos.
- **Estudantes**: Ganhar experiência na resolução de problemas reais de banco de dados.

## Autor

Acompanhe mais detalhes nos repositórios do meu [GitHub](https://github.com/pedrolporto)! 🚀

---

**Pronto para o desafio? Faça um fork deste repositório e mostre suas habilidades em SQL!**


