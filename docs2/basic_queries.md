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


# Explorações perto de um rio

A table `cbase_ara.rios` tem os rios da região da ARA-Sul, IP.

<details>
  <summary>Que explorações estão a menos de 1km do rio mais longo</summary>

  ```sql
  SELECT ...
  ```

</details>


