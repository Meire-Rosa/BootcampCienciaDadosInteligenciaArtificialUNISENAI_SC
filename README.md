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

Com relação à predição de falhas, considere o seguinte quadro resumo das classificações sobre falha de um equipamento. 

<img width="600" height="245" alt="image" src="https://github.com/user-attachments/assets/561998dc-fd81-4c9c-b6c9-77fcb9a62632" />

Segundo o PO, o custo diário de manutenção corretiva de uma máquina, gira em torno de 5.000,00 por dia. Assim, cada parada da máquina, gera um custo de 25.000 (Em média, a máquina fica parada 5 dias para a manutenção corretiva). Enquanto, o custo da manutenção preditiva gera um custo de 2.0589,82 por dia. E um dia, é o suficiente para realizar esta manutenção. 

Obs: Estamos usando a proporção descrita no trabalho Kardec, A., Nascif, J., 2009. Manutenção-função estratégica. Qualitymark Editora Ltda, Tabela 1 (Tab1 https://www.researchgate.net/profile/Gustavo-Fernandes-19/publication/337746795_COSTS_ANALYSIS_BETWEEN_PREDICTIVE_AND_CORRECTIVE_MAINTENANCE_FOR_AN_AGRIBUSINESS_MACHINE/links/629e00c755273755ebd7c80c/COSTS-ANALYSIS-BETWEEN-PREDICTIVE-AND-CORRECTIVE-MAINTENANCE-FOR-AN-AGRIBUSINESS-MACHINE.pdf). 


Usamos 80% dos dados para treino e 20% teste. Desse modo, como resultado dos 20% dados treinados, obtemos a seguinte análise.


- **Modelo CatBoostClassifier:** 
<img width="706" height="129" alt="image" src="https://github.com/user-attachments/assets/49f24da2-b38f-4aa3-a4f6-89cb75eb718f" />

Melhor modelo. Com a aplicação desse modelo nos 20% dos dados de teste, chegamos a uma economia de 31.48% do custo apresentado pelo PO. 


- **Modelo LightGBM:**
<img width="700" height="117" alt="image" src="https://github.com/user-attachments/assets/9c0744b0-8c3b-4c36-8544-aa38e0a8db59" />

Segundo modelo melhor avaliado. Apresenta uma economia de 30.49% em relação à manutenção corretiva.


- **Modelo RandomForestClassifier:**
<img width="720" height="119" alt="image" src="https://github.com/user-attachments/assets/7c1c10fd-104f-45d5-acb9-ea71b04f50ad" />

Terceiro modelo melhor avaliado. Apresenta uma economia de 12.43% dos custos com relação à manutenção corretiva. 


- **Modelo Regressão Logística:**
<img width="704" height="148" alt="image" src="https://github.com/user-attachments/assets/b9fb1c83-344f-441b-94e3-39520b13ec6d" />

Esse modelo apresentou um desempenho ruim. A aplicação deste, geraria um aumento no custo de manutenção de 30.49%.
 
---

