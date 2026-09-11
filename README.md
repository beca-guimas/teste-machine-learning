# 📂 Portfólio de Ciência e Análise de Dados

Bem-vindo(a) ao meu portfólio! Aqui estão reunidos os meus projetos práticos focados em extração de inteligência de negócios com SQL e modelos preditivos usando Machine Learning.

## 🧭 Menu de Navegação
* [🎵 Projeto 1: Análise de Dados - E-commerce Musical (SQL)](#-projeto-1-análise-de-dados-e-commerce-musical-banco-chinook)
* [🏠 Projeto 2: Previsão de Preços de Casas (Machine Learning)](#-projeto-2-previsão-de-preços-de-casas-machine-learning)



# 🎵 Projeto de Análise de Dados: E-commerce Musical (Banco Chinook)

##  Sobre o Projeto
Este projeto demonstra a aplicação prática de **SQL** para a exploração e extração de inteligência de negócio a partir de uma base de dados real de um e-commerce de música (Banco Chinook). Utilizando o cliente **DBeaver**, as consultas cobrem desde a auditoria inicial dos dados até análises complexas de faturamento e comportamento de consumo.

---

## Estrutura e Exploração Inicial (Sanity Check)

Antes de iniciar as análises de negócio, foi realizada uma validação estrutural do banco de dados para mapear as Tabelas e as suas respetivas **Chaves Primárias (PK)** e **Chaves Estrangeiras (FK)** via Diagrama ER no DBeaver.

---

## 🚀 Desafios de Negócio e Soluções SQL

### 📋 Nível 1: Consultas Simples e Filtros

#### 1. Identificação de Clientes no Mercado Brasileiro
* **Problema:** A equipa de marketing precisa de listar o contacto de todos os clientes que residem no Brasil para o envio de uma campanha localizada.
```sql
SELECT FirstName, LastName, Email 
FROM Customer 
WHERE Country = 'Brazil';
```
* **Resultado Obtido:**

| FirstName | LastName | Email |
| :--- | :--- | :--- |
| Luís | Gonçalves | luisg@embraer.com.br |
| Eduardo | Martins | eduardo@woodstock.com.br |
| Alexandre | Rocha | alero@uol.com.br |
| Roberto | Almeida | roberto.almeida@riotur.gov.br |
| Fernanda | Ramos | fernandaramos4@uolcom.br |


#### 2. Análise do Catálogo de Músicas Longas
* **Problema:** O setor de curadoria da plataforma deseja identificar quais são as 10 faixas mais longas do catálogo para criar uma playlist especial de "músicas épicas".
```sql
SELECT Name, Milliseconds 
FROM Track 
ORDER BY Milliseconds DESC 
LIMIT 5;
```
* **Resultado Obtido:**

| Name | Milliseconds |
| :--- | :--- |
| Occupation / Precipice | 5286953 |
| Through a Looking Glass | 5088838 |
| Greetings from Earth, Pt. 1 | 2960293 |
| The Man With Nine Lives | 2956998 |
| Battlestar Galactica, Pt. 2 | 2956081 |


#### 3. Auditoria de Faturas de Alto Valor
* **Problema:** O departamento financeiro solicitou uma listagem das faturas com valor superior a 15 dólares para uma auditoria de transações premium.
```sql
SELECT InvoiceId, Total 
FROM Invoice 
WHERE Total > 15
ORDER BY Total DESC;
```

* **Resultado Obtido (Amostra das maiores faturas):**

| InvoiceId | Total |
| :--- | :--- |
| 404 | 25.86 |
| 299 | 23.86 |
| 96 | 21.86 |

---


### 📊 Nível 2: Agrupamentos e Métricas de Negócio

#### 4. Faturamento Consolidado por País
* **Problema:** A diretoria precisa de um relatório que mostre o total histórico de vendas acumulado por cada país para identificar as regiões mais lucrativas.
```sql
SELECT 
    BillingCountry, 
    SUM(Total) AS Total_Vendido
FROM Invoice
GROUP BY BillingCountry
ORDER BY SUM(Total) DESC;
```

* **Resultado Obtido (Top 5 países líderes em vendas):**

| BillingCountry | Total_Vendido |
| :--- | :--- |
| USA | 523.06 |
| Canada | 303.96 |
| France | 195.10 |
| Brazil | 190.10 |
| Germany | 156.48 |


#### 5. Análise de Preço Médio por Tipo de Mídia
* **Problema:** A equipa de precificação precisa de entender se existe variação no preço médio cobrado de acordo com o formato digital/físico da música (`MediaTypeId`).
```sql
SELECT 
    MediaTypeId, 
    ROUND(AVG(UnitPrice), 2) AS Preco_Medio 
FROM Track 
GROUP BY MediaTypeId;
```

* **Resultado Obtido:**

| MediaTypeId | Preco_Medio |
| :--- | :--- |
| 1 | 0.99 |
| 2 | 0.99 |
| 3 | 1.99 |
| 4 | 0.99 |
| 5 | 0.99 |


#### 6. Identificação de Clientes Fiéis (Frequência de Compra)
* **Problema:** O programa de fidelidade da loja quer mapear o ID de todos os clientes recorrentes que realizaram mais de 5 compras no total.
```sql
SELECT 
    CustomerId, 
    COUNT(InvoiceId) AS Total_De_Compras
FROM Invoice
GROUP BY CustomerId
HAVING COUNT(InvoiceId) > 5
ORDER BY Total_De_Compras DESC;
```

* **Resultado Obtido (Clientes com mais de 5 compras):**

| CustomerId | Total_De_Compras |
| :--- | :--- |
| 1 | 7 |
| 2 | 7 |
| 3 | 7 |
*(Nota: O banco possui dezenas de clientes empatados com exatamente 7 compras)*


---

### 🔗 Nível 3: Relacionamentos e Cruzamento de Dados (JOINs)

#### 7. Mapeamento de Álbuns e os seus Respetivos Artistas
* **Problema:** Para corrigir inconsistências visuais na interface do usuário, foi solicitada uma consulta que vincule diretamente o título do álbum ao nome por extenso do artista.
```sql
SELECT 
    Album.Title AS Titulo_Album, 
    Artist.Name AS Nome_Artista
FROM Album
JOIN Artist ON Album.ArtistId = Artist.ArtistId;
```

* **Resultado Obtido (Amostra das primeiras linhas):**

| Titulo_Album | Nome_Artista |
| :--- | :--- |
| For Those About To Rock We Salute You | AC/DC |
| Balls to the Wall | Accept |
| Restless and Wild | Accept |


#### 8. Extração do Catálogo Específico de 'Rock'
* **Problema:** A equipa editorial vai criar um banner temático de Rock e precisa do nome de todas as faixas classificadas especificamente sob este gênero musical.
```sql
SELECT 
    Track.Name AS Nome_Musica, 
    Genre.Name AS Genero
FROM Track
JOIN Genre ON Track.GenreId = Genre.GenreId
WHERE Genre.Name = 'Rock';
```

* **Resultado Obtido (Amostra do catálogo de Rock):**

| Nome_Musica | Genero |
| :--- | :--- |
| For Those About To Rock (We Salute You) | Rock |
| Put On The Heat | Rock |
| Inject The Venom | Rock |


#### 9. Histórico Detalhado de Compras por Cliente
* **Problema:** O suporte técnico precisa de um relatório completo que liste o nome do cliente, o número do pedido e o título exato de cada música comprada por ele para resolver problemas de downloads corrompidos.
```sql
SELECT 
    Customer.FirstName AS Nome_Cliente, 
    Invoice.InvoiceId AS ID_Fatura, 
    Track.Name AS Nome_Musica
FROM Customer
JOIN Invoice     ON Customer.CustomerId = Invoice.CustomerId
JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId
JOIN Track       ON InvoiceLine.TrackId = Track.TrackId
ORDER BY Invoice.InvoiceId;
```

* **Resultado Obtido (Amostra do cruzamento de dados):**

| Nome_Cliente | ID_Fatura | Nome_Musica |
| :--- | :--- | :--- |
| Luís | 1 | Balls to the Wall |
| Luís | 1 | Fast as a Shark |
| Leonie | 2 | Restless and Wild |


---

## 🛠️ Tecnologias Utilizadas
* **Banco de Dados:** Chinook (Relacional)
* **Linguagem:** ANSI SQL
* **Ferramenta de Acesso:** DBeaver Community Edition

---

---


# 🏠 Projeto 2: Previsão de Preços de Casas (Machine Learning)

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
