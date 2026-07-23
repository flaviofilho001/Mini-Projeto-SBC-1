# Mini-Projeto 3 — FitTech Knowledge Graph

**Autores:** Flávio Mesquita Marinho Filho e John Victor de Oliveira Atanazio

## Resumo

Este projeto incrementa os Mini-Projetos 1 e 2 do SBC FitTech. O domínio de dietas, treinos, perfis metabólicos, objetivos e suplementação foi convertido em uma base de conhecimento RDF/RDFS/OWL, armazenada em Turtle e consultada em Python com `rdflib` e SPARQL.

> A base é acadêmica e serve para demonstrar modelagem semântica. Ela não substitui avaliação médica, nutricional ou de educação física.

## Link Colab (https://colab.research.google.com/drive/1SaJE0dJUo6dIXgQwGSoGN8UWtOZoYjyy?usp=sharing)
## Link Vídeo (https://drive.google.com/file/d/1AtlSR-krV6qEJeJnVcA729AwUps8bsf6/view?usp=sharing)

## Arquivos

- `fittech.ttl`: ontologia e indivíduos.
- `MiniProjeto3_FitTech.ipynb`: carregamento, validações, consultas `g.triples()` e consultas/updates SPARQL.
- `requirements.txt`: dependência do projeto.

## Taxonomia principal

A raiz é `EntidadeFitTech`. Abaixo dela existem `Pessoa`, `PerfilMetabolico`, `Objetivo`, `Plano` e `Suplemento`. A hierarquia possui mais de dois níveis, por exemplo:

- `EntidadeFitTech > Pessoa > Profissional > Nutricionista`
- `EntidadeFitTech > Pessoa > Profissional > PersonalTrainer`
- `EntidadeFitTech > PerfilMetabolico > PerfilRapido`
- `EntidadeFitTech > Objetivo > Hipertrofia`
- `EntidadeFitTech > Plano > PlanoAlimentar`

## Principais relações

Usuários possuem perfil, objetivo, plano alimentar, plano de treino, suplementos e profissionais responsáveis. Os dados incluem nome, idade, peso, altura, experiência, restrição alimentar, calorias e frequência de treino.

## Construções OWL utilizadas

A base usa, com justificativa no domínio:

- `owl:inverseOf`: `possuiPerfil/perfilDe` e `orientadoPor/orienta`.
- `owl:FunctionalProperty`: um usuário possui um perfil e um objetivo; pessoas possuem um único nome cadastrado.
- `owl:InverseFunctionalProperty`: um perfil individual identifica seu usuário.
- `owl:SymmetricProperty`: relação entre planos complementares.
- `owl:TransitiveProperty`: derivação entre versões de planos.
- `owl:disjointWith`: plano alimentar versus plano de treino e perfil rápido versus perfil lento.

## Consultas implementadas

O notebook contém 5 consultas com `g.triples()` e 11 operações SPARQL:

1. `SELECT` de usuários e objetivos.
2. `SELECT` com `FILTER` por idade.
3. `SELECT` com `ORDER BY` por peso.
4. `SELECT` com agregação e `GROUP BY`.
5. `ASK`.
6. `CONSTRUCT`.
7. `INSERT DATA`.
8. Verificação do `INSERT`.
9. `DELETE DATA`.
10. `DELETE/INSERT WHERE`.
11. Verificação final dos updates.

## Execução

### Jupyter/VS Code

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook MiniProjeto3_FitTech.ipynb
```

Execute as células em ordem. O arquivo `fittech.ttl` precisa permanecer na mesma pasta do notebook.

### Google Colab

1. Abra o notebook no Colab.
2. Envie `fittech.ttl` para a área de arquivos da sessão.
3. Execute todas as células em ordem.

O notebook instala `rdflib` automaticamente quando necessário.

## Requisitos atendidos

- Mais de 8 classes, com hierarquia de pelo menos dois níveis abaixo da raiz.
- Mais de 10 propriedades, incluindo pelo menos 5 de objeto e 5 de dados.
- Mais de 25 indivíduos e mais de 50 triplas.
- Mais de 5 construções OWL justificadas.
- 5 consultas com `g.triples()`.
- Mais de 8 consultas/operações SPARQL, cobrindo `SELECT`, `FILTER`, `ORDER BY`, agregação, `ASK`, `CONSTRUCT`, `INSERT`, `DELETE` e `DELETE/INSERT`.
