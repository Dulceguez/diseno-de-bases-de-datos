![alt text](image.png)

1.  SELECT DISTINCT p.apellido, p.nombre, p.mail
    FROM Profesor p 
    INNER JOIN Curso c ON (c.idProfesor = p.idProfesor)
    WHERE c.idArea = ‘Datos’

    UNION

    SELECT DISTINCT e.apellido, e.nombre, e.mail
    FROM Estudiante e 
    INNER JOIN Inscripción i ON (e.idEstudiante=i.idEstudiante)
    INNER JOIN Curso c ON (i.idCurso=c.idCurso)
    WHERE c.idArea = ‘Datos’
        AND resultado > 3 
        AND i.año >= 2023


2.  SELECT e.nombre, e.apellido, e.mail
    FROM Estudiante e
    INNER JOIN Inscripción i ON (e.idEstudiante=i.idEstudiante)
    INNER JOIN Curso c ON (i.idCurso = c.idCurso)
    WHERE (i.semestre = 1)
        AND (c.idArea = ‘Datos’)

    INTERSECT

    SELECT e.nombre, e.apellido, e.mail
    FROM Estudiante e
    INNER JOIN Inscripción i ON (e.idEstudiante=i.idEstudiante)
    INNER JOIN Curso c ON (i.idCurso = c.idCurso)
    WHERE (i.semestre = 1)
        AND (i.año = 2026)
        AND (c.idArea = ‘Ing. de Software’)


3.  SELECT e.nombre, e.apellido, e.mail
    FROM Estudiante e
    INNER JOIN Inscripción i ON (e.idEstudiante=i.idEstudiante)
    INNER JOIN Curso c ON (i.idCurso = c.idCurso)
    WHERE (i.semestre = 1)
        AND (i.año = 2026)
        AND (c.idArea = ‘Sistemas Operativos’ 
        OR c.idArea = 'Redes')

    EXCEPT

    SELECT e.nombre, e.apellido, e.mail
    FROM Estudiante e
    INNER JOIN Inscripción i ON (e.idEstudiante=i.idEstudiante)
    INNER JOIN Curso c ON (i.idCurso = c.idCurso)
    WHERE (i.semestre = 1)
        AND (i.año = 2026)
        AND (c.idArea = ‘Programación’)


4.  SELECT e.nombre, e.apellido, e.email
    FROM Estudiante e
    INNER JOIN Inscripción i 
    ON (e.idEstudiante = i.idEstudiante)
    INNER JOIN Curso c ON (i.idCurso = c.idCurso)
    WHERE (c.idArea = ‘Programación’)
        AND (i.resultado > 3)

    GROUP BY e.idEstudiante, e.nombre, e.apellido, e.email HAVING COUNT(DISTINCT c.idCurso) = ( 
    SELECT COUNT(*) 
    FROM Curso 
    WHERE idArea = 'Programación' 
    );