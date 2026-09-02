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
