# 🏭 Predição Preventiva de Falhas em Máquinas Industriais

Projeto desenvolvido durante o **Bootcamp de Ciência de Dados e Inteligência Artificial (UNISENAI-SC)** com foco em **manutenção preditiva**.  
O objetivo é criar um sistema capaz de identificar falhas em máquinas industriais a partir de dados de sensores IoT, prevendo não apenas **se ocorrerá falha**, mas também **qual o tipo de falha**.

---

## 📊 Contextualização

Uma empresa do setor industrial forneceu um conjunto de dados coletados por dispositivos IoT monitorando variáveis operacionais de diferentes máquinas.  
A partir desses dados, foi desenvolvido um modelo de **classificação supervisionada** capaz de prever falhas e retornar a probabilidade associada a cada tipo de defeito.

Esse sistema permite **reduzir custos de manutenção, evitar paradas inesperadas e aumentar a eficiência operacional**.

---

## 📂 Descrição dos Dados

Os dados foram disponibilizados em dois arquivos:

- `Bootcamp_train.csv` → Usado para exploração, treino e validação dos modelos.  
- `Bootcamp_test.csv` → Usado para avaliação final via API.  

### Variáveis disponíveis:

- `id` → Identificador da amostra  
- `id_produto` → Identificação única da máquina/produto  
- `tipo` → Tipo de máquina (`L`, `M`, `H`)  
- `temperatura_ar` → Temperatura do ar (K)  
- `temperatura_processo` → Temperatura do processo (K)  
- `umidade_relativa` → Umidade relativa (%)  
- `velocidade_rotacional` → Velocidade rotacional (RPM)  
- `torque` → Torque da máquina (Nm)  
- `desgaste_da_ferramenta` → Tempo de uso da ferramenta (min)  
- `falha_maquina` → Indicador de falha (0 = não, 1 = sim)  
- **Tipos de falha:**  
  - `FDF` → Falha Desgaste Ferramenta  
  - `FDC` → Falha Dissipação de Calor  
  - `FP` → Falha de Potência  
  - `FTE` → Falha de Tensão Excessiva  
  - `FA` → Falha Aleatória  

---

## 🧠 Modelagem

- Foi definida uma **abordagem de classificação multirrótulo (multi-label)**, pois uma falha pode estar associada a diferentes causas.  
- Testes com diferentes algoritmos:
  - `LogisticRegression` 
  - `RandomForestClassifier`  
  - `XGBoostClassifier`  
  - `CatBoostClassifier`  
- Uso de **estratégias de balanceamento** (ex: SMOTE, class weights) devido ao forte desbalanceamento entre falhas e não-falhas.  
- Avaliação com múltiplas métricas:  
  - `Accuracy`  
  - `ROC-AUC (macro/micro)`  
  - `F1-score por classe`  
  - `PR-AUC` (mais informativo em cenários desbalanceados)  

---

## 📈 Resultados

- **Recall alto para falhas** → o modelo conseguiu identificar a maioria das falhas reais (82%).
- **Precisão ainda baixa para falhas específicas** → indicando ocorrência de falsos positivos (alarmes falsos).  
- **ROC-AUC > 0.90** → ótima capacidade discriminativa (91.33%)  
- Insights obtidos foram traduzidos em **gráficos de barras, heatmaps e matrizes de confusão** para melhor interpretação.  

Agora, analisando que houve falha, a classificação do tipo de falha, apresenta bom desempenho geral(F1 médio igual a 0.71) e possui boa separabilidade global (ROC-AUC >0.90).

Desse modo, conclui-se que, dado que existe uma falha, 11.46% está associada ao FDF, 33.69% a FDC, 15.09% a FP e 25.29% a FTE.

<img width="691" height="689" alt="image" src="https://github.com/user-attachments/assets/62018327-7e87-430f-9618-042b6e411385" />


---

