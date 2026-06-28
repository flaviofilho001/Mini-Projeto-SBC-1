# 🏋️‍♂️ SBC FitTech - Sistema Especialista de Regras para Dietas e Treinos

# Projeto 2

### Flávio Mesquita Marinho Filho
### John Victor de Oliveira Atanazio

## Link colab (https://colab.research.google.com/drive/13HHPU22172A5HcXe2sIK5CSMPw5pQ_nA?usp=sharing)
## Link Vídeo ()

# 🎛️ SBC FitTech Fuzzy - Controlador Temático de Nutrição Esportiva

Este módulo faz parte do **Mini-Projeto 2**, reescrevendo a lógica do sistema especialista anterior baseada em regras booleanas severas para uma abordagem baseada em **Lógica Fuzzy (Mamdani)**, utilizando a biblioteca `scikit-fuzzy`.

## 🎯 Descrição do Domínio e Justificativa
No ambiente de FitTech real, as características humanas e objetivos não são discretos. Uma pessoa não deixa de ser "ectomorfa" e se torna "mesomorfa" de forma abrupta; existe uma zona de transição contínua. 
O sistema analisa a intensidade do **Objetivo Calórico** e o comportamento do **Metabolismo** do usuário para computar o **Ajuste Calórico Diário** ideal (em kcal) necessário para a sua dieta.

### Variáveis e Termos Linguísticos
1. **Objetivo (Input: 0 a 10):** Representa a direção da meta calórica do usuário.
   * `perda`: Funções trapezoidais na faixa inferior.
   * `manutencao`: Função triangular centralizada no valor 5.
   * `ganho`: Função trapezoidal na faixa superior.
2. **Metabolismo (Input: 0 a 10):** Taxa de eficiência metabólica basal (Equivalência matemática aos biotipos clássicos).
   * `rapido` (Ectomorfo): Dificuldade intrínseca de reter energia.
   * `misto` (Mesomorfo): Equilíbrio metabólico.
   * `lento` (Endomorfo): Facilidade alta de acúmulo lipídico.
3. **Ajuste Calórico (Output: -1000 a +1000 kcal):** Resposta real de calorias acrescidas ou removidas na dieta do indivíduo.
   * `deficit_alto`, `moderado`, `superavit_alto`.

## 📜 Base de Regras (Matriz Combinatória Completa)
Para garantir que o espaço de entradas não apresente nenhuma lacuna (gaps), mapeamos a matriz $3 \times 3$ completa gerando 9 regras consistentes:

* **R1:** SE objetivo é *perda* E metabolismo é *rapido* ENTÃO ajuste é *moderado* (Protege contra o catabolismo).
* **R2:** SE objetivo é *perda* E metabolismo é *misto* ENTÃO ajuste é *deficit_alto*.
* **R3:** SE objetivo é *perda* E metabolismo é *lento* ENTÃO ajuste é *deficit_alto*.
* **R4:** SE objetivo é *manutencao* E metabolismo é *rapido* ENTÃO ajuste é *moderado*.
* **R5:** SE objetivo é *manutencao* E metabolismo é *misto* ENTÃO ajuste é *moderado*.
* **R6:** SE objetivo é *manutencao* E metabolismo é *lento* ENTÃO ajuste é *deficit_alto*.
* **R7:** SE objetivo é *ganho* E metabolismo é *rapido* ENTÃO ajuste é *superavit_alto*.
* **R8:** SE objetivo é *ganho* E metabolismo é *misto* ENTÃO ajuste é *superavit_alto*.
* **R9:** SE objetivo é *ganho* E metabolismo é *lento* ENTÃO ajuste é *moderado* (Abordagem de ganho limpo).

## 🚀 Instruções de Execução e Reprodução
1. Certifique-se de possuir o ambiente Python configurado com suporte ao pip.
2. Instale a biblioteca necessária executando:
   ```bash
   pip install scikit-fuzzy numpy