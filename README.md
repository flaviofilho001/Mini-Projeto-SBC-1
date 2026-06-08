# 🏋️‍♂️ SBC FitTech - Sistema Especialista de Regras para Dietas e Treinos

### Flávio Mesquita Marinho Filho
### John Victor de Oliveira Atanazio

## Link Colab https://colab.research.google.com/drive/1-6fGhw_nT36-_NOSgCv7ohQ0ehv_JFnZ?usp=sharing
#### (Tem o notebook que eu também rodei na minha máquina, mas é o mesmo código)
## Link video https://drive.google.com/drive/folders/1VX30ZKDkOgsXu6rpAW3dEnvrgzloZfhU?usp=sharing

Este repositório contém o código de um Sistema Baseado em Conhecimento (SBC) estruturado em regras **IF-THEN** utilizando a biblioteca Python `experta`. O sistema atua como um consultor virtual de FitTech, inferindo planos de treino, dieta e suplementação personalizados a partir de dados físicos e objetivos fornecidos pelo usuário.

## 🎯 Descrição do Domínio
O domínio foca na triagem e prescrição preliminar de estratégias de nutrição e educação física baseadas no perfil metabólico individual. O sistema adota uma inferência em **3 níveis de encadeamento**:
1. **Nível 1 (Entrada ➔ Perfil Metabólico):** Mapeamento de Biotipo + Objetivo para determinar o funcionamento metabólico basal.
2. **Nível 2 (Perfil Metabólico ➔ Planejamento Base):** Cruzamento do Perfil Metabólico com possíveis restrições alimentares para geração do macro-planejamento de dieta e treino.
3. **Nível 3 (Planejamento Base ➔ Recomendação Final):** Adequação das intensidades dos treinos e tipos de suplementos com base na experiência prática do usuário.

---

## ⚖️ Estratégia de Resolução de Conflito
O motor utiliza a estratégia de **Salience (Prioridade de Execução)**:
* A **Regra 1** possui `salience=10`, o que garante que inconsistências biológicas (ex: um ectomorfo puro buscando emagrecimento severo) sejam interceptadas e tratadas imediatamente na memória de trabalho antes que regras de menor prioridade entrem em conflito na agenda.
* A regra de exibição final possui `salience=-10`, assegurando que o resultado textual e a explicabilidade só sejam renderizados após todo o encadeamento de dados da árvore terminar.

---

## 📜 Listagem das 10 Regras (Linguagem Natural)

* **Regra 1 (Incompatibilidade):** **SE** o usuário quer emagrecimento **E** o biotipo é ectomorfo, **ENTÃO** force o Perfil Metabólico para "gasto_rápido" (prioridade alta) para evitar perda extrema de massa magra.
* **Regra 2 (Perfil Ectomorfo):** **SE** o objetivo é hipertrofia **E** o biotipo é ectomorfo, **ENTÃO** classifique o Perfil Metabólico como "gasto_rapido".
* **Regra 3 (Perfil Endomorfo):** **SE** o objetivo é emagrecimento **E** o biotipo é endomorfo, **ENTÃO** classifique o Perfil Metabólico como "acumulo_gordura".
* **Regra 4 (Dieta Padrão Ganho):** **SE** o Perfil Metabólico for "gasto_rapido" **E** não houver restrições alimentares, **ENTÃO** defina o Planejamento como Dieta Hipercalórica Padrão e Treino de força.
* **Regra 5 (Dieta Restrita Ganho):** **SE** o Perfil Metabólico for "gasto_rapido" **E** a restrição for intolerância à lactose, **ENTÃO** defina o Planejamento como Dieta Hipercalórica Sem Lactose e Treino de força.
* **Regra 6 (Plano de Perda):** **SE** o Perfil Metabólico for "acumulo_gordura", **ENTÃO** defina o Planejamento como Dieta Hipocalórica Restrita e Treino com déficit calórico + cardio.
* **Regra 7 (Conclusão Ecto-Iniciante):** **SE** o Planejamento for Hipercalórico Padrão **E** a experiência for iniciante, **ENTÃO** recomende superávit de 500kcal com laticínios, treino ABC 3x e suplementação com hipercalórico e creatina.
* **Regra 8 (Conclusão Ecto-Restrito):** **SE** o Planejamento for Hipercalórico Sem Lactose **E** a experiência for iniciante, **ENTÃO** recomende superávit de 500kcal com substitutos vegetais, treino ABC 3x e suplementação com proteína isolada da carne e creatina.
* **Regra 9 (Conclusão Endo-Iniciante):** **SE** o Planejamento for de déficit com cardio **E** a experiência for iniciante, **ENTÃO** recomende déficit de 400kcal, treino AB 4x com cardio leve e suplementação de Whey com café puro.
* **Regra 10 (Conclusão Endo-Avançado):** **SE** o Planejamento for de déficit com cardio **E** a experiência for avançado, **ENTÃO** recomende déficit de 600kcal com ciclagem de carboidratos, treino ABCDE de alta intensidade e suplementação de Whey isolado e termogênico.

---

## 🧪 Casos de Teste e Saídas Esperadas

O sistema foi validado utilizando 3 cenários distintos com o rastreamento (trace) de regras ativo:

### Caso de Teste 1: Ectomorfo Iniciante Padrão
* **Entradas:** `objetivo="hipertrofia"`, `biotipo="ectomorfo"`, `restricao="nenhuma"`, `experiencia="iniciante"`
* **Saída Esperada no Notebook:**
  * **🥗 DIETA:** Superávit de 500kcal com ingestão alta de leite integral e derivados.
  * **🏋️‍♂️ TREINO:** Treino ABC 3x na semana, foco em exercícios compostos, descanso longo.
  * **💊 SUPLEMENTOS:** Hipercalórico e Creatina.
  * **🔍 EXPLICABILIDADE:** Esta decisão foi tomada porque as regras **Regra 2, Regra 4, Regra 7** dispararam em cadeia.

### Caso de Teste 2: Ectomorfo com Restrição Alimentar
* **Entradas:** `objetivo="hipertrofia"`, `biotipo="ectomorfo"`, `restricao="intolerante_lactose"`, `experiencia="iniciante"`
* **Saída Esperada no Notebook:**
  * **🥗 DIETA:** Superávit de 500kcal utilizando leite de amêndoas, aveia e carnes magras.
  * **🏋️‍♂️ TREINO:** Treino ABC 3x na semana, foco em execução correta.
  * **💊 SUPLEMENTOS:** Proteína isolada da carne (Beef Protein) e Creatina.
  * **🔍 EXPLICABILIDADE:** Esta decisão foi tomada porque as regras **Regra 2, Regra 5, Regra 8** dispararam em cadeia.

### Caso de Teste 3: Endomorfo Avançado para Definição
* **Entradas:** `objetivo="emagrecimento"`, `biotipo="endomorfo"`, `restricao="nenhuma"`, `experiencia="avancado"`
* **Saída Esperada no Notebook:**
  * **🥗 DIETA:** Déficit de 600kcal com estratégia de ciclagem de carboidratos.
  * **🏋️‍♂️ TREINO:** Treino ABCDE de alta intensidade (Drop-sets) + 40 min HIIT alternados.
  * **💊 SUPLEMENTOS:** Whey Isolado, Termogênico e BCAA.
  * **🔍 EXPLICABILIDADE:** Esta decisão foi tomada porque as regras **Regra 3, Regra 6, Regra 10** dispararam em cadeia.

