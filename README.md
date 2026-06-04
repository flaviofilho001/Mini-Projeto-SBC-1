# 🏋️‍♂️ SBC FitTech - Sistema Especialista de Regras para Dietas e Treinos

### Flávio Mesquita Marinho Filho
### John Victor de Oliveira Atanazio

## Link Colab https://colab.research.google.com/drive/1-6fGhw_nT36-_NOSgCv7ohQ0ehv_JFnZ?usp=sharing
#### (Tem o notebook que eu também rodei na minha máquina, mas é o mesmo código)

Este repositório contém o código de um Sistema Baseado em Conhecimento (SBC) estruturado em regras **IF-THEN** utilizando a biblioteca Python `experta`. O sistema atua como um consultor virtual de FitTech, inferindo planos de treino, dieta e suplementação personalizados a partir de dados físicos e objetivos fornecidos pelo usuário.

## 🎯 Descrição do Domínio
O domínio escolhido foca na triagem e prescrição preliminar de estratégias de nutrição e educação física baseadas no perfil metabólico individual. O sistema adota uma inferência em **3 níveis de encadeamento**:
1. **Nível 1:** Mapeamento de Biotipo + Objetivo para determinar a velocidade metabólica basal.
2. **Nível 2:** Cruzamento do Perfil Metabólico com possíveis restrições alimentares para geração do macro-planejamento.
3. **Nível 3:** Adequação das intensidades e tipos de suplementos com base no nível de experiência prática do usuário.

---

## ⚖️ Estratégia de Resolução de Conflito
O motor utiliza a estratégia de **Salience (Prioridade)**. 
* A **Regra 1** possui `salience=10`, o que garante que inconsistências de dados (ex: um ectomorfo puro buscando emagrecimento severo) sejam interceptadas imediatamente na memória de trabalho antes que regras de menor prioridade entrem em conflito na agenda de execução.
* A regra de exibição final possui `salience=-10`, assegurando que o resultado só seja renderizado após todo o encadeamento de dados terminar.

---

## 📜 Listagem das 10 Regras (Linguagem Natural)

* **Regra 1 (Incompatibilidade):** **SE** o usuário quer emagrecimento **E** o biotipo é ectomorfo, **ENTÃO** force o Perfil Metabólico para "gasto_rápido" (prioridade alta) para evitar perda extrema de massa magra.
* **Regra 2 (Perfil Ectomorfo):** **SE** o objetivo é hipertrofia **E** o biotipo é ectomorfo, **ENTÃO** classifique o Perfil Metabólico como "gasto_rapido".
* **Regra 3 (Perfil Endomorfo):** **SE** o objetivo é emagrecimento **E** o biotipo é endomorfo, **ENTÃO** classifique o Perfil Metabólico como "acumulo_gordura".
* **Regra 4 (Dieta Padrão Ganho):** **SE** o Perfil Metabólico for "gasto_rapido" **E** não houver restrições alimentares, **ENTÃO** defina o Planejamento como Dieta Hipercalórica Padrão e Treino focado em força.
* **Regra 5 (Dieta Restrita Ganho):** **SE** o Perfil Metabólico for "gasto_rapido" **E** a restrição for intolerância à lactose, **ENTÃO** defina o Planejamento como Dieta Hipercalórica Sem Lactose e Treino focado em força.
* **Regra 6 (Plano de Perda):** **SE** o Perfil Metabólico for "acumulo_gordura", **ENTÃO** defina o Planejamento como Dieta Hipocalórica Restrita e Treino com déficit calórico + cardio.
* **Regra 7 (Conclusão Ecto-Iniciante):** **SE** o Planejamento for Hipercalórico Padrão **E** o nível de experiência for iniciante, **ENTÃO** recomende superávit de 500kcal com laticínios, treino ABC 3x na semana e suplementação com hipercalórico e creatina.
* **Regra 8 (Conclusão Ecto-Restrito):** **SE** o Planejamento for Hipercalórico Sem Lactose **E** o nível de experiência for iniciante, **ENTÃO** recomende superávit de 500kcal com substitutos vegetais, treino ABC 3x na semana e suplementação com proteína isolada da carne e creatina.
* **Regra 9 (Conclusão Endo-Iniciante):** **SE** o Planejamento for de déficit com cardio **E** a experiência for iniciante, **ENTÃO** recomende déficit de 400kcal, treino AB 4x na semana com cardio leve e suplementação de Whey com café pura.
* **Regra 10 (Conclusão Endo-Avançado):** **SE** o Planejamento for de déficit com cardio **E** a experiência for avançado, **ENTÃO** recomende déficit de 600kcal com ciclagem de carboidratos, treino ABCDE de alta intensidade com HIIT e suplementação de Whey isolado e termogênicos ultra.

---

## 🧪 Casos de Teste Avaliados

### Caso de Teste 1: Ectomorfo Iniciante Padrão
* **Entrada:** `objetivo="hipertrofia"`, `biotipo="ectomorfo"`, `restricao="nenhuma"`, `experiencia="iniciante"`
* **Resultado Esperado:** Perfil metabólico rápido $\rightarrow$ Dieta hipercalórica com leite $\rightarrow$ Treino ABC 3x + Hipercalórico.

### Caso de Teste 2: Ectomorfo com Restrição Alimentar
* **Entrada:** `objetivo="hipertrofia"`, `biotipo="ectomorfo"`, `restricao="intolerante_lactose"`, `experiencia="iniciante"`
* **Resultado Esperado:** Perfil metabólico rápido $\rightarrow$ Dieta hipercalórica sem lactose $\rightarrow$ Treino ABC 3x + Beef Protein (proteína da carne).

### Caso de Teste 3: Endomorfo Avançado para Definição
* **Entrada:** `objetivo="emagrecimento"`, `biotipo="endomorfo"`, `restricao="nenhuma"`, `experiencia="avancado"`
* **Resultado Esperado:** Perfil acúmulo de gordura $\rightarrow$ Dieta hipocalórica restrita $\rightarrow$ Treino ABCDE de alta intensidade + HIIT + Ciclagem de carbo.

## Exemplo de teste

🚀 Caso de Teste 1: Ectomorfo Iniciante Padrão
[TRACE - Regra 2 disparada] Usuário é Ectomorfo focado em Hipertrofia.
  -> Decisão: Declarado PerfilMetabolico = gasto_rapido.
[TRACE - Regra 4 disparada] Perfil gasto_rapido sem restrições alimentares.
  -> Decisão: Definido Planejamento com Dieta Hipercalórica Padrão.
[TRACE - Regra 7 disparada] Customizando para Iniciante em Hipertrofia Padrão.

==================================================
🎯 RECOMENDAÇÕES GERADAS PELO SISTEMA ESPECIALISTA
==================================================
🥗 DIETA:       Superávit de 500kcal com ingestão alta de leite integral e derivados.
🏋️‍♂️ TREINO:      Treino ABC 3x na semana, foco em exercícios compostos, descanso longo.
💊 SUPLEMENTOS: Hipercalórico e Creatina.
==================================================