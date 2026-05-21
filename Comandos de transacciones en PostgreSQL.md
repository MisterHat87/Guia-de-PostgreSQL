# Comandos de transacciones en PostgreSQL
Notas de clase

---
# 1. Parámetro AUTOCOMMIT 
El parámetro `AUTOCOMMIT` viene siempre activado por defecto en el Postgre
Esto significa que cada sentencia SQL se confirma automáticamente después de ejecutarse.

Para ver su estado se utiliza el siguiente comando:

## Para ver su estado: 

```sql
\echo :AUTOCOMMIT
```


¿Cómo lo apago y prendo?

Apagarlo:
```sql
\set AUTOCOMMIT off 
```

Prenderlo:
```sql
\set AUTOCOMMIT on
```

---



# Integridad de datos


## SAVEPOINT;
Es como Hacer un checKpoint, o un punto de guardado de una transacción.
Es opcional, pero es útil cuando se desea regresar solo a una parte específica de la transacción y no cancelar todo desde el inicio
Esta es su forma de digitarlo: 
```sql
SAVEPOINT punto1;
```



> Donde poner el sabe point, Según el profe "Digamos que en una transacción que tenga de 5 a 10 líneas" esto ya que al hacer un rollback es un regreso muy abrupto
Hay que tener el criterio de saber en que momento hay que usar el save point y el roll back

## ROLLBACK;
Cargar el checkpoint 
Cancela los cambios  no confirmados, se encarga de gestionar errores, es como el ctrl z
```sql
ROLLBACK;
```
---

# Comando para Realizar una Transaccion


## BEGIN;
El comando `BEGIN` se utiliza para iniciar una transacción manualmente en PostgreSQL.
Básicamente, lo que hace es abrir una instacncia, un espacio controlado donde se pueden ejecutar varias sentencias SQL antes de decidir si los cambios se guardan o se cancelan.
A lo que me refiero es que si se hacen cambios dentro del `BEGIN` se quedan dentro de este, hasta que se haga un `COMMIT`, osea, no afectan a la base de datos;
```sql
BEGIN;
```
---
si yo cometi un error al escribirlo y quiere borrar lo que se estaba haciendo dentro de ese en este BEGIN; use "/d" y esto lo cerrara de manera abrupta
 
---

## COMMIT;
Esto es lo que cierra todo los cambios hechos de forma manual por el usuario dentro del `BEGIN`. 
Para ilustrarlo de otra manera tengo esta cita sin referencias apa 7 que saque de internet de Alguien(s.f) "En programación, un commit se refiere a la acción de confirmar los cambios realizados en un repositorio de código", en unas palabras más mundanas
"El botón de enviar", este en la base de datos hace cambios permanentes e irreversibles
```sql
COMMIT;
```
---

EJEMPLO, del trabajo hecho en clase
---
```sql
BEGIN; 
INSERT INTO cuentas (nombre, saldo) VALUES ('Pedro', 300); 
SAVEPOINT punto1; 
UPDATE cuentas SET saldo = saldo - 50 WHERE nombre = 'Pedro'; -- Error 
UPDATE cuentas SET saldo = saldo + 50 WHERE nombre = 'MariaX'; 
ROLLBACK TO punto1; 
COMMIT;


```
 
