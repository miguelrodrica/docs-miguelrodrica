---
sidebar_position: 1
---

# SQL - Subiendo Nivel

## Nivel 1 - Fundamentos (Exploración básica)

1. Listar todos los usuarios.

```
SELECT * from users;
```

2. Mostrar solo first_name, last_name, email.

```
SELECT first_name, last_name, email FROM users;
```

3. Filtrar usuarios cuyo role sea 'admin'.

```
SELECT * FROM users WHERE role = 'admin';
```

4. Filtrar usuarios con document_type = 'CC'.

```
SELECT * FROM users WHERE document_type = 'CC';
```

5. Mostrar usuarios mayores de 18 años (calcular edad desde birth_date).

```
SELECT * FROM users WHERE TIMESTAMPDIFF(year, birth_date, CURDATE()) >= 18;
```

6. Mostrar usuarios cuyo ingreso sea mayor a 5,000,000.

```
SELECT * FROM users WHERE monthly_income > 5000000;
```

7. Mostrar usuarios cuyo nombre empiece por "A".

```
SELECT * FROM users WHERE first_name LIKE 'a%';
```

8. Mostrar usuarios que no tengan company.

```
SELECT * FROM users WHERE company is NULL;
```

## Nivel 2 - Combinación de condiciones

9. Usuarios mayores de 25 años que sean 'employee'.

```
SELECT * FROM users WHERE TIMESTAMPDIFF(year, birth_date, CURDATE()) >= 25 AND role = 'employee';
```

10. Usuarios con 'CC' que estén activos.

```
SELECT * FROM users WHERE document_type = 'CC' AND is_active = 1;
```

11. Usuarios mayores de edad sin empleo.

```
SELECT * FROM users WHERE TIMESTAMPDIFF(year, birth_date, CURDATE()) >= 18 AND company is NULL;
```

12. Usuarios con empleo y con ingresos mayores a 3,000,000.

```
SELECT * FROM users WHERE company IS NOT NULL AND monthly_income > 3000000;
```

13. Usuarios casados con al menos 1 hijo.

```
SELECT * FROM users WHERE marital_status = 'casado' AND children_count >= 1;
```

14. Usuarios entre 30 y 40 años.

```
SELECT * FROM users WHERE TIMESTAMPDIFF(year, birth_date, CURDATE()) BETWEEN 30 AND 40;
```

15. Usuarios 'admin' verificados mayores de 25 años.

```
SELECT * FROM users
WHERE role = 'admin'
AND TIMESTAMPDIFF(year, birth_date, CURDATE()) > 25
AND is_verified = 1;
```

## Nivel 3 - Introducción a análisis (Agregaciones)

16. Contar usuarios por role.

```
SELECT role, COUNT(*) FROM users GROUP BY role;
```

17. Contar usuarios por document_type.

```
SELECT document_type, COUNT(*) FROM users GROUP BY document_type;
```

18. Contar cuántos usuarios están desempleados.

```
SELECT COUNT(*) FROM users WHERE company is NULL;
```

19. Calcular el promedio general de ingresos.

```
SELECT AVG(monthly_income) FROM users;
```

20. Calcular el promedio de ingresos por role.

```
SELECT role, AVG(monthly_income) FROM users GROUP BY role;
```

## Nivel 4 - Pensamiento analítico

21. Mostrar profesiones con más de 10 personas.

```
SELECT profession, COUNT(*)
FROM users
WHERE profession IS NOT NULL
GROUP BY profession
HAVING COUNT(*) > 10;
```

22. Mostrar la ciudad con más usuarios.

```
SELECT city , COUNT(*) AS num_city
FROM users
GROUP BY city
ORDER BY num_city DESC
LIMIT 1
```

23. Comparar cantidad de menores vs mayores de edad.

```
SELECT
  CASE
    WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) < 18 THEN 'Menores'
    ELSE 'Mayores de edad'
  END AS categoria,
  COUNT(*) AS total
FROM users
GROUP BY categoria;
```

24. Promedio de ingresos por ciudad ordenado de mayor a menor.

```
SELECT city, AVG(monthly_income) as promedio
FROM users
GROUP BY city
ORDER BY promedio desc;
```

25. Mostrar las 5 personas con mayor ingreso.

```
SELECT first_name , MAX(monthly_income) as mayor_ingreso
FROM users
GROUP BY first_name
ORDER BY mayor_ingreso desc
LIMIT 5;
```

## NIVEL 5 - Nivel Ingeniero

26. Clasificar usuarios como:

"Menor"
"Adulto"
"Adulto mayor"

```
SELECT
  CASE
    WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) < 18 THEN 'Menores'
    WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) BETWEEN 18 and 50 THEN 'Adulto'
    ELSE 'Adulto mayor'
  END AS categoria,
  COUNT(*) AS total
FROM users
GROUP BY categoria;
```

27. Mostrar cuántos usuarios hay en cada clasificación anterior.

```
SELECT
  CASE
    WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) < 18 THEN 'Menores'
    WHEN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) BETWEEN 18 and 50 THEN 'Adulto'
    ELSE 'Adulto mayor'
  END AS categoria,
  COUNT(*) AS total
FROM users
GROUP BY categoria;
```

28. Ranking de ingresos por ciudad.

```
SELECT
  city,
  SUM(monthly_income ) AS total_ingresos,
  RANK() OVER (ORDER BY SUM(monthly_income) DESC) AS ranking
FROM users
GROUP BY city;
```

29. Profesión con mayor ingreso promedio.

```
SELECT profession, AVG(monthly_income) AS promedio_ingreso
FROM users
GROUP BY profession
ORDER BY promedio_ingreso DESC
LIMIT 1;
```

30. Mostrar usuarios cuyo ingreso esté por encima del promedio general.

```
SELECT *
FROM users
WHERE monthly_income > (
    SELECT AVG(monthly_income)
    FROM users
);
```