# Pasar una explotación existente a ser una renovación de otra

En la base de datos tenemos algunas explotaciones que en realidad deberían haber sido consideradas renovaciones de licencia. A medida que se van descubriendo se van actualizando a mano.

En principio no parece posible realizar este proceso mediante la aplicación, puesto que se provocaría la renovación de la propia licencia en sí.

En este documento se comentan algunos de las cosas que hay que saber para hacer este proceso sin entrar en mucho detalle:

Las renovaciones de una explotación existente se van almacenando en la tabla `utentes.renovacoes`

Hay que escribir una sentencia de este estilo (no se ha probado):

A continuación se plantea unas posibles sentencias para realizar la actualización. Se puede copiar y pegar a un documento de texto. Editar para poner los valores correctos y luego ejecutar en la base de datos.

```
BEGIN;

-- Primer paso. Anotamos el `gid` y `exp_id` de la explotación que vamos a eliminar. Asumiendo que conocemos el exp_id y que este es por ejemplo: '007/ARAZ/2018/CL' ( y que el gid será `2048` )

SELECT gid, exp_id, exp_name FROM utentes.exploracaoes WHERE exp_id = '007/ARAZ/2018/CL'

INSERT INTO utentes.renovacoes

(exp_id, d_soli, d_ultima_entrega_doc, estado, tipo_lic_sup_old, d_emissao_sup_old, d_validade_sup_old, c_licencia_sup_old, consumo_fact_sup_old, tipo_lic_sup, d_emissao_sup, d_validade_sup, c_licencia_sup, tipo_lic_sub_old, d_emissao_sub_old, d_validade_sub_old, c_licencia_sub_old, consumo_fact_sub_old, tipo_lic_sub, d_emissao_sub, d_validade_sub, c_licencia_sub, exploracao)

VALUES (
    exp_id -- exp_id de la explotación, por ejemplo: '007/ARAZ/2018/CL'
    , d_soli -- en general, será NULL porqué representaría la fecha en que se pidió la renovación. Se podría poner quizas el d_soli de la explotación pero no tiene mucho sentido
    , d_ultima_entrega_doc -- en general será NULL
    , estado -- En general 'Licenciada'. Sería el estado de la licencia en el momento en que se acabo el proceso de renovación
    , tipo_lic_sup_old -- todos estos valores _old se rellenan con los datos de la explotación antigua que se va a eliminar Licença/Concesión/...
    , d_emissao_sup_old
    , d_validade_sup_old
    , c_licencia_sup_old
    , consumo_fact_sup_old
    , tipo_lic_sup -- los que no tienen el _old, serían los nuevos valores que tendría la licencia en el momento en que se acaba el proceso de renovación
    , d_emissao_sup
    , d_validade_sup
    , c_licencia_sup
    , tipo_lic_sub_old
    , d_emissao_sub_old
    , d_validade_sub_old
    , c_licencia_sub_old
    , consumo_fact_sub_old
    , tipo_lic_sub
    , d_emissao_sub
    , d_validade_sub
    , c_licencia_sub
    , exploracao -- gid de la explotación, por ejemplo 2048
);

-- Abrir la web para comprobar que los datos son correctos

-- Comprobar el gid y nombre y número de explotaciones de la utente que posee la explotación a eliminar, por si tuviera sentido eliminar también
SELECT u.gid, u.nome, count(e.*) FROM utentes.utentes u JOIN utentes.exploracaos e ON u.gid = e.utente WHERE u.gid =  ANY('{2048}') GROUP BY u.gid, u.nome;

-- Borrar la explotación antigua
-- DELETE FROM utentes.exploracaos WHERE gid = 2048;
-- Mejor hacerlo desde la web para que elimine también los adjuntos.

-- Si es necesario borrar la utente antigua
```

Se podría hacer una query que automatizara esto si fuera necesario hacerlo muchas veces.

Un tema a mayores sería el de los ficheros adjuntos. Una opción de usuario (se podria hacer de alguna otra forma):

* Se entra en la explotación antes de eliminarla.
* Se bajan los archivos adjuntos
* Se suben a mano a la nueva explotación
* Se anota el gid de la explotación a eliminar y se borra del servidor la carpeta de adjuntos (si es que la explotación se ha borrado directamente en la base de datos)