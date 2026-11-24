# algoritmo-recomendacao-musicas-grafos-neo4j
Sistema de recomendação de músicas utilizando Neo4j e Cypher com base em conexões entre usuários, músicas, artistas e gêneros.
# Algoritmo de Recomendação de Músicas com Base em Grafos (Neo4j)

Projeto desenvolvido para o desafio **"Criando um Algoritmo de Recomendação de Músicas com Base em Grafos"** da plataforma DIO, utilizando o banco de dados orientado a grafos **Neo4j** e a linguagem de consulta **Cypher**.

## Contexto do Problema
Plataformas de streaming precisam recomendar músicas relevantes para cada usuário com base no seu histórico de escuta, preferências de gêneros, artistas favoritos e padrões de usuários com gostos semelhantes.

## Por que usar Grafos neste cenário?
Em bancos relacionais tradicionais, cruzar gostos de usuários, artistas, gêneros e músicas exige múltiplos `JOIN`s que se tornam mais custosos à medida que a rede cresce. Em um grafo:
- **Entidades** (usuários, músicas, artistas, gêneros) são representadas como **Nós** (`(:Usuario)`, `(:Musica)`, `(:Artista)`, `(:Genero)`).
- **Conexões** (`:OUVIU`, `:CURTIU`, `:INTERPRETA`, `:PERTENCE_A`) são tratadas como cidadãs de primeira classe, permitindo travessias rápidas e intuitivas.

## Modelo do Grafo
- **Nós e propriedades principais**:
  - `Usuario`: `id`, `nome`, `pais`
  - `Musica`: `id`, `titulo`, `duracao_seg`, `ano`
  - `Artista`: `id`, `nome`
  - `Genero`: `nome`
- **Relacionamentos**:
  - `(:Usuario)-[:OUVIU {vezes: int}]->(:Musica)`
  - `(:Usuario)-[:CURTIU]->(:Musica)`
  - `(:Artista)-[:INTERPRETA]->(:Musica)`
  - `(:Musica)-[:PERTENCE_A]->(:Genero)`

*(Dica: Gere a visualização do esquema no Neo4j Browser com `CALL db.schema.visualization()` ou desenhe no Arrows.app e salve a imagem em `imagens/modelo-grafo.png`.)*

## Como Executar
1. Inicie uma instância do **Neo4j** (Neo4j Desktop ou Neo4j AuraDB).
2. Abra o **Neo4j Browser**.
3. Execute o script de carga em `cypher/01_carga_dados.cypher`.
4. Execute as consultas de recomendação em `cypher/02_recomendacoes.cypher`.

## Troubleshooting (Dificuldades e Soluções)
- **Duplicidade ao reexecutar scripts**: usar `MERGE` em vez de `CREATE` para nós e relacionamentos chave evita criar registros duplicados ao rodar o script mais de uma vez.
- **Falha ao excluir nós com conexões**: ao limpar dados de teste, usar `DETACH DELETE` remove o nó junto com seus relacionamentos sem erro de integridade.

## Evidências Visuais
Adicione aqui os prints das consultas executadas no Neo4j Browser:
- Visualização do grafo completo / esquema
- Resultado da recomendação por gênero e por usuários semelhantes
