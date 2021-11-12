Neste documento, apresentamos algumas _queries_ básicas para obter informações do banco de dados.

São indicados tanto para fins de treinamento quanto para fins práticos, para fazer relatórios que podem não ser fáceis de obter diretamente do aplicativo SIRHA.

# Conte as principais entidades do banco de dados

As principais entidades do banco de dados são:

* Utente. https://gitlab.com/icarto/utentes-bd/-/blob/master/docs/glosario.md#utente
* Exploracao. https://gitlab.com/icarto/utentes-bd/-/blob/master/docs/glosario.md#explotacion
* Licença. https://gitlab.com/icarto/utentes-bd/-/blob/master/docs/glosario.md#licencia

<details>
  <summary>Quais são as consultas que permitem obter o número de elementos dessas entidades?</summary>

  ```sql
  -- Conteo de utentes
  SELECT ...
  
  -- Conteo de explorações
  SELECT ...
  
  -- Conteo de licenças
  SELECT ...
  ```
</details>

Mas simplesmente contar as explorações não nos dá muitas informações. Podem estar em vias de obtenção de licença de uso de água, podem estar ativos e faturáveis, podem estar em estado irregular ou ter sido negada, ...

Os estados podem ser agrupados em grupos: https://gitlab.com/icarto/utentes-bd/-/blob/master/docs/glosario.md#estados

O campo `status_lic` indica o status da exploraçaos.

<details>
  <summary>Número de explorações por estado, e explorações por grupo de estados </summary>

  ```sql
  SELECT ...
  ```
</details>

# Balanço hídrico

Para conhecer o balanço hídrico é necessário conhecer a disponibilidade (oferta) e demanda. Podemos obter a demanda de certa forma conforme o consumo licenciado pelos usuários.

<details>
  <summary>Consumo licenciado das explorações de uma bacia</summary>
  
  Usaremos o agrupamento da tabela de exploração pelo campo `loc_bacia` e adicionando o campo` c_licencia` apenas para aquelas já concedidas Licenciadas, Utente de facto, Utente de usos comuns

  ```sql
  SELECT ...
  ```
</details>



# Lista de utentes com seus dados agregados nas explorações (número de explorações e soma do consumo licenciado)

Um utente pode ter várias explorações. Portanto, devemos juntar a tabela de utentes com a de explorações.

Agrupamos pelo pk do utente (gid) ou pelo nome do utente, contamos o número de fazendas e somamos o consumo licenciado.

<details>
  <summary>Lista de utentes com seus dados agregados nas explorações</summary>

  ```sql
  SELECT ...
  ```
</details>

Para melhorar um pouco, geralmente o que nos interessa é que os dados sejam ordenados por:

* O número de fazendas
* Ou o consumo licenciado

<details>
  <summary>Ordenado por consumo licenciado</summary>

  ```sql
  SELECT ...
  ```
</details>


Para facilitar a interpretação dos dados, também podemos filtrar por aqueles que têm duas ou mais explorações

<details>
  <summary>Filtro para aqueles com mais de duas explorações</summary>

  ```sql
  SELECT ...
  ```
</details>

<details>
  <summary>Filtre para listar os 10 com o maior consumo licenciado</summary>

  ```sql
  SELECT ...
  ```
</details>

# Explorações perto de um rio

A table `cbase_ara.rios` tem os rios da região da ARA-Sul, IP.

<details>
  <summary>Que explorações estão a menos de 1km do rio mais longo</summary>

  ```sql
  SELECT ...
  ```

</details>

# Lista de explorações é o período de faturamento (Mensal, Trimestral, Anual)

É uma informação que pode ser do interesse do DHR e do DSU.

É útil incluir na lista o tipo de água (da tabela `utentes.licencias`) e o nome do usuário (da tabela de `utentes.utentes`).+

E deve-se levar em conta que somente as propriedades com estado de `Licenciada` e `Utente de facto` são faturáveis. 

<details>
  <summary>Lista de explorações é o período de faturamento (Mensal, Trimestral, Anual)</summary>
  
  ```sql
SELECT
        u.nome "Nome do cliente/utente" 
        , e.exp_id "Nr. de exploraçào" 
        , e.exp_name "Nome da exploração" 
        , e.loc_divisao "Divisao" 
        , e.loc_bacia "Bacia" 
        , e.loc_provin "Provincia" 
        , e.loc_distri "Distrito" 
        , e.loc_posto "Posto" 
        , e.fact_tipo "Tipo de facturação" 
        , lic. "Tipo de Agua" 
        , e.estado_lic "Estado" 
    FROM
        utentes.exploracaos e
        JOIN utentes.utentes u ON e.utente = u.gid
        JOIN (
            SELECT
                l.exploracao
                , string_agg(l.tipo_agua , ', ') "Tipo de Agua" 
            FROM
                utentes.licencias l
            GROUP BY
                l.exploracao) lic ON lic.exploracao = e.gid
        WHERE e.estado_lic IN ('Licenciada', 'Utente de facto')
        ORDER BY
            exp_name;  
  ```
</details>

