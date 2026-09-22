# 🏠 Previsão de Preços de Casas (Machine Learning)

[![Abrir no Colab](https://google.com)](https://colab.research.google.com/drive/1bApraSfVYN_VMtd5QiTRX8bJlc0sOuk0?usp=sharing)

## 📝 Descrição do Projeto
Este é um exercício prático de **Machine Learning** desenvolvido em Python no Google Colab. O objetivo foi treinar um modelo de inteligência artificial para entender os padrões de preços de imóveis e **prever o valor de venda de novas casas** na região de Campina Grande.

O projeto foi construído em duas etapas evolutivas, motivadas pela necessidade de tornar as previsões mais realistas e detalhadas.

---

## 🛠️ Tecnologias e Bibliotecas Utilizadas
* **Python**: Linguagem base do projeto.
* **Pandas**: Para a criação e estruturação da base de dados dos imóveis.
* **NumPy**: Para manipulação das matrizes de dados.
* **Scikit-Learn**: Biblioteca para importação e treinamento do algoritmo de `LinearRegression`.

---

## 🚀 Evolução da Solução e Blocos de Código

### 📋 Código 1: Regressão Linear Simples (Abordagem Inicial)
* **Motivação:** Validar o funcionamento inicial do algoritmo relacionando apenas o tamanho do imóvel (m²) ao preço final.
```python
import numpy as np
from sklearn.linear_model import LinearRegression

# 1. Tamanho das casas em m² (Dados de treino)
tamanho_casas = np.array([[50], [70], [80], [120], [150]])

# 2. Preços reais dessas casas (em milhares de reais)
precos_reais = np.array([150, 210, 240, 360, 450])

# 3. Criando e treinando o modelo de IA para achar o padrão
modelo = LinearRegression()
modelo.fit(tamanho_casas, precos_reais)

# 4. Fazendo a IA prever o preço de uma casa de 100m² que ela nunca viu
casa_nova = np.array([[100]])
preco_previsto = modelo.predict(casa_nova)

print(f"Para uma casa de 100m², a IA previu o valor de: R$ {preco_previsto[0]:.2f} mil")
```
* **Resultado em Tela:**
```text
Para uma casa de 100m², a IA previu o valor de: R$ 300.00 mil
```
* **⚠️ Crítica à Abordagem Inicial:** A resposta gerada pela máquina foi considerada **muito rasa e inadequada**. O modelo calculou um valor linear genérico e caro, sem qualquer especificidade sobre o que o imóvel possuía (falta de dados sobre localização, número de quartos, etc.). Diante disso, houve a necessidade de refatorar a abordagem.

---

### 📊 Código 2: Regressão Linear Múltipla (Modelo Avançado e Regional)
* **Solução Aplicada:** Para trazer especificidade ao modelo, foram criadas novas variáveis estruturais e geográficas baseadas no contexto de Campina Grande, permitindo que a IA fizesse previsões multifatoriais e mais precisas.
```python
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression

# 1. Base de dados de Campina Grande com valores preenchidos explicitamente
dados_cg = pd.DataFrame({
    'tamanho_m2':,
    'quartos':,
    'possui_primeiro_andar':,
    'arborizacao_alta':,
    'bairro_nobre':,
    'preco_real_mil': [120, 140, 160, 190, 180, 450, 600, 800, 950, 1200]})

# Separando contexto (X) e resposta (y)
X = dados_cg[['tamanho_m2', 'quartos', 'possui_primeiro_andar', 'arborizacao_alta', 'bairro_nobre']]
y = dados_cg['preco_real_mil']

# Treinando o modelo
modelo_cg = LinearRegression()
modelo_cg.fit(X, y)

# Criando os cenários como DataFrames do Pandas para evitar avisos de warning
casa_periferia_limpa = pd.DataFrame([[100, 3, 0, 0, 0]], columns=X.columns)
casa_nobre_limpa = pd.DataFrame([[280, 4, 1, 1, 1]], columns=X.columns)

# Fazendo as previsões sem gerar alertas na tela
prev_periferia = modelo_cg.predict(casa_periferia_limpa)
prev_nobre = modelo_cg.predict(casa_nobre_limpa)

print(f"Previsão para casa em Bodocongó/Malvinas: R\$ {prev_periferia[0]:.2f} mil")
print(f"Previsão para casarão em Jardim Tavares/Conj. Professores: R\$ {prev_nobre[0]:.2f} mil")
```

* **Resultados Obtidos após a Especificidade:**

| Tipo de Imóvel / Cenário | Características Adicionadas | Previsão Atualizada da IA |
| :--- | :--- | :--- |
| **Casa em Bairro Popular** (Ex: Bodocongó / Malvinas) | 100m², 3 quartos, sem andar, sem arborização alta, fora de bairro nobre | **R$ 153.33 mil** |
| **Casarão em Bairro Nobre** (Ex: Jardim Tavares / Conj. Professores) | 280m², 4 quartos, com andar, arborização alta, em bairro nobre | **R$ 966.67 mil** |

---

## 🚀 Como Executar este Projeto
1. Clique no botão **Abrir no Colab** localizado logo abaixo do título deste projeto.
2. Com a página do Google Colab aberta, clique em **Ambiente de execução** no menu superior.
3. Escolha a opção **Executar tudo** (ou use o atalho `Ctrl + F9`).
4. Role a página do notebook até o final para ver as duas saídas de previsão impressas na tela.
