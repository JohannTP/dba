# dba
SELECT A.Nombre APLICACION, R.Nombre ROL, R.Id RolId, M.Nombre MODULO, M.Id ,SM.Descripcion SUBMODULOS, SM.ModuloId, RSM.RolId FROM UsuariosAplicacionesRoles UAR
JOIN Aplicaciones A
ON UAR.AplicacionId = A.Id
--JOIN RolesAplicaciones RA ON A.Id = RA.AplicacionId
JOIN Roles R
ON UAR.RolId = R.Id
JOIN RolesModulos RM
ON R.Id = RM.RolId
JOIN Modulos M ON RM.ModuloId = M.Id AND A.Id = M.AplicacionId
LEFT JOIN RolesSubModulos RSM ON R.Id = RSM.RolId AND M.Id = (SELECT ModuloId FROM SubModulos WHERE Id = RSM.SubModuloId) 
LEFT JOIN SubModulos SM ON RSM.SubModuloId = SM.Id AND M.Id = SM.ModuloId
WHERE UAR.UsuarioId = 1

https://bootswatch.com/united/
https://positiwise.com/blog/clean-architecture-net-core
https://www.youtube.com/watch?v=u1HWG1YMFKA
https://learn.microsoft.com/es-es/sql/t-sql/lesson-1-creating-database-objects?view=sql-server-ver16



/*DROP TABLE RolesSubModulos
DROP TABLE SubModulos
DROP TABLE Aplicaciones
DROP TABLE UsuariosAplicacionesRoles
DROP TABLE UsuariosRoles
DROP TABLE RolesModulos
DROP TABLE Modulos
DROP TABLE Roles
DROP TABLE UsuariosAplicaciones
DROP TABLE Permisos
DROP TABLE Usuarios*/

CREATE TABLE Usuarios(
	Id INT PRIMARY KEY IDENTITY,
	Codigo VARCHAR(50) NOT NULL,
	Nombre VARCHAR(50) NOT NULL,
	ApellidoPaterno VARCHAR(50) NOT NULL,
	ApellidoMaterno VARCHAR(50) NOT NULL,
	Email VARCHAR(50) NOT NULL,
	Estado BIT NOT NULL,
	CodigoSede INT,
	FechaCreacion DATETIME,
	FechaActualizacion DATETIME,
	FechaBaja DATETIME,
	IdUsuarioCreador INT,
	IdUsuarioActualiza INT
);

CREATE TABLE Aplicaciones(
	Id INT PRIMARY KEY IDENTITY,
	Nombre VARCHAR(50) NOT NULL,
	Estado BIT NOT NULL,
	FechaCreacion DATETIME,
	FechaActualizacion DATETIME,
	IdUsuarioCreador INT,
	IdUsuarioActualiza INT
);

CREATE TABLE Roles(
	Id INT PRIMARY KEY IDENTITY,
	Nombre VARCHAR(50) NOT NULL,
	Estado BIT NOT NULL,
	FechaCreacion DATETIME,
	FechaActualizacion DATETIME,
	IdUsuarioCreador INT,
	IdUsuarioActualiza INT
);

CREATE TABLE Modulos(
	Id INT PRIMARY KEY IDENTITY,
	Nombre VARCHAR(50) NOT NULL,
	Ruta VARCHAR(50) NOT NULL,
	AplicacionId INT NOT NULL,
	Estado BIT NOT NULL,
	FechaCreacion DATETIME,
	FechaActualizacion DATETIME,
	IdUsuarioCreador INT,
	IdUsuarioActualiza INT
	FOREIGN KEY (AplicacionId) REFERENCES Aplicaciones(Id)
);


CREATE TABLE SubModulos(
	Id INT PRIMARY KEY IDENTITY,
	Descripcion VARCHAR(50) NOT NULL,
	Ruta VARCHAR(50) NOT NULL,
	ModuloId INT NOT NULL,
	Estado BIT NOT NULL,
	FechaCreacion DATETIME,
	FechaActualizacion DATETIME,
	IdUsuarioCreador INT,
	IdUsuarioActualiza INT
	FOREIGN KEY (ModuloId) REFERENCES Modulos(Id)
);

CREATE TABLE UsuariosAplicacionesRoles(
	Id INT PRIMARY KEY IDENTITY,
	UsuarioId INT NOT NULL,
	AplicacionId INT NOT NULL,
	RolId INT NOT NULL,
	FechaCreacion DATETIME,
	FechaActualizacion DATETIME,
	IdUsuarioCreador INT,
	IdUsuarioActualiza INT
	FOREIGN KEY (UsuarioId) REFERENCES Usuarios(Id),
	FOREIGN KEY (AplicacionId) REFERENCES Aplicaciones(Id),
	FOREIGN KEY (RolId) REFERENCES Roles(Id)
);


CREATE TABLE RolesModulos(
	Id INT PRIMARY KEY IDENTITY,
	RolId INT NOT NULL,
	ModuloId INT NOT NULL,
	FechaCreacion DATETIME,
	FechaActualizacion DATETIME,
	IdUsuarioCreador INT,
	IdUsuarioActualiza INT
	FOREIGN KEY (RolId) REFERENCES Roles(Id),
	FOREIGN KEY (RolId) REFERENCES Modulos(Id)
);

CREATE TABLE RolesSubModulos(
	Id INT PRIMARY KEY IDENTITY,
	RolId INT NOT NULL,
	SubModuloId INT NOT NULL,
	FechaRegistro DATETIME,
	FechaActualizacion DATETIME,
	IdUsuarioCreador INT,
	IdUsuarioActualiza INT
	FOREIGN KEY (RolId) REFERENCES Roles(Id),
	FOREIGN KEY (SubModuloId) REFERENCES SubModulos(Id)
);

CREATE TABLE RolesAplicaciones(
	Id INT PRIMARY KEY IDENTITY,
	RolId INT NOT NULL,
	AplicacionId INT NOT NULL,
	FechaCreacion DATETIME,
	FechaActualizacion DATETIME,
	IdUsuarioCreador INT,
	IdUsuarioActualiza INT
	FOREIGN KEY (RolId) REFERENCES Roles(Id),
	FOREIGN KEY (AplicacionId) REFERENCES Aplicaciones(Id)
);



--uspUsuariosObtener()

SELECT A.Nombre, M.Nombre, SM.Descripcion FROM Aplicaciones A
LEFT JOIN Modulos M
ON A.Id = M.AplicacionId
LEFT JOIN SubModulos SM
ON M.Id = SM.ModuloId
WHERE A.Id = 1
















SELECT * FROM SubModulos
delete from Modulos
DELETE FROM SubModulos
DBCC CHECKIDENT ('SubModulos', RESEED, 0);

---------------------
SELECT M.Nombre FROM Modulos M
JOIN RolesModulos RM
ON M.Id = RM.ModuloId
WHERE RM.RolId = 1

SELECT SM.Descripcion FROM SubModulos SM
JOIN RolesSubModulos RSM
ON SM.Id = RSM.SubModuloId
WHERE RSM.RolId = 1

SELECT A.Nombre APLICACION, R.Nombre ROL, M.Nombre MODULO, SM.Descripcion SUBMODULO
FROM UsuariosAplicacionesRoles UAR
LEFT JOIN Aplicaciones A
ON UAR.AplicacionId = A.Id
LEFT JOIN Roles R
ON UAR.RolId = R.Id
LEFT JOIN RolesModulos RM
ON R.Id = RM.RolId
LEFT JOIN Modulos M
ON RM.ModuloId = M.Id AND A.Id = M.AplicacionId AND R.Id = RM.Id
LEFT JOIN RolesSubModulos RSM
ON R.Id = RSM.RolId 
LEFT JOIN SubModulos SM
ON RSM.SubModuloId = SM.Id AND M.Id = SM.ModuloId
WHERE UAR.UsuarioId = 1
------------------
--MODULOSP POR APLICACION
SELECT A.Nombre, M.Nombre FROM Aplicaciones A
JOIN Modulos M
ON A.Id = M.AplicacionId

-- MODULOS POR ROL
SELECT M.Nombre, RM.Id  FROM Modulos M
JOIN RolesModulos RM
ON M.Id = RM.ModuloId
WHERE RM.RolId = 1 





--APLICACION ROL Y MODULO
SELECT A.Nombre APLICACION, R.Nombre ROL, M.Nombre FROM UsuariosAplicacionesRoles UAR
LEFT JOIN Aplicaciones A
ON UAR.AplicacionId = A.Id
LEFT JOIN Roles R
ON UAR.RolId = R.Id
JOIN RolesModulos RM
ON R.Id = RM.RolId
JOIN Modulos M ON RM.ModuloId = M.Id
WHERE UAR.UsuarioId = 1

-- APLICACION ROL MODULO Y SUBMODULO

SELECT A.Nombre APLICACION, R.Nombre ROL, R.Id RolId, M.Nombre MODULO, M.Id ,SM.Descripcion SUBMODULOS, SM.ModuloId, RSM.RolId 
FROM UsuariosAplicacionesRoles UAR
JOIN Aplicaciones A
ON UAR.AplicacionId = A.Id
--JOIN RolesAplicaciones RA ON A.Id = RA.AplicacionId
JOIN Roles R
ON UAR.RolId = R.Id
JOIN RolesModulos RM
ON R.Id = RM.RolId
JOIN Modulos M ON RM.ModuloId = M.Id AND A.Id = M.AplicacionId
LEFT JOIN RolesSubModulos RSM ON R.Id = RSM.RolId AND M.Id = (SELECT ModuloId FROM SubModulos WHERE Id = RSM.SubModuloId) 
LEFT JOIN SubModulos SM ON RSM.SubModuloId = SM.Id AND M.Id = SM.ModuloId
WHERE UAR.UsuarioId = 1 AND A.Id = 1

select * from SubModulos

select * from RolesSubModulos

-- ****************************
GO
ALTER PROCEDURE uspUsuariosSelect
	@pEmail VARCHAR(50),
	@pAplicacionId INT
AS
BEGIN
	SELECT U.Id, U.Email 
	FROM Usuarios U
	JOIN UsuariosAplicacionesRoles UAR ON U.Id = UAR.UsuarioId
	JOIN Aplicaciones A ON UAR.AplicacionId = A.Id
	WHERE U.Email = @pEmail AND A.Id = @pAplicacionId
END 

GO
CREATE PROCEDURE uspAplicacionesSelect
	@pEmail VARCHAR(50),
	@pAplicacionId INT
AS
BEGIN
	SELECT A.Id, A.Nombre 
	FROM Aplicaciones A
	JOIN UsuariosAplicacionesRoles UAR ON A.Id = UAR.AplicacionId
	JOIN Usuarios U ON UAR.UsuarioId = U.Id
	WHERE U.Email = @pEmail AND A.Id = @pAplicacionId

END 

ALTER PROCEDURE uspRolesSelect
	@pUsuarioId INT,
	@pAplicacionId INT
AS
BEGIN
	SELECT R.Id, R.Nombre FROM UsuariosAplicacionesRoles UAR
	JOIN Roles R ON UAR.RolId = R.Id
	WHERE UAR.Id = @pUsuarioId AND UAR.AplicacionId = @pAplicacionId
END



SELECT M.Id, M.Nombre, M.Ruta FROM UsuariosAplicacionesRoles UAR
JOIN Roles R 
ON UAR.RolId = R.Id
JOIN RolesModulos RM
ON R.Id = RM.RolId
JOIN Modulos M 
ON RM.ModuloId = M.Id
WHERE UAR.AplicacionId = 2 AND R.Id = 1 AND UAR.UsuarioId = 1


SELECT R.Id, R.Nombre FROM UsuariosAplicacionesRoles UAR
JOIN Roles R 
ON UAR.RolId = R.Id WHERE R.Id = 1 AND UAR.Id = 1


ALTER PROCEDURE uspModulosSelect
	@pUsuarioId INT,
	@pRolId INT,
	@pAplicacionId INT
AS
BEGIN
	SELECT DISTINCT M.Id, M.Nombre, M.Ruta FROM UsuariosAplicacionesRoles UAR
	JOIN Roles R ON UAR.RolId = R.Id
	JOIN RolesModulos RM ON R.Id = RM.RolId
	JOIN Modulos M ON RM.ModuloId = M.Id AND UAR.AplicacionId = M.AplicacionId
	WHERE UAR.AplicacionId = @pAplicacionId AND UAR.RolId = @pRolId AND UAR.UsuarioId = @pUsuarioId
END





EXEC uspUsuariosSelect @pEmail = 'jose.flores.@touring.pe', @pAplicacion = 1
EXEC uspAplicacionesSelect @pEmail = 'jose.flores.@touring.pe', @pAplicacionId = 1
EXEC uspRolesSelect @pUsuarioId = 5, @pAplicacionId = 1;














-----APLICACIONES ROLES MODULOS Y SUB MODULOS SIN LA TABLA ROLESAPLICACION
SELECT A.Nombre APLICACION, R.Nombre ROL, M.Nombre MODULO, SM.Descripcion SUBMODULOS FROM UsuariosAplicacionesRoles UAR
LEFT JOIN Aplicaciones A
ON UAR.AplicacionId = A.Id
LEFT JOIN Roles R
ON UAR.RolId = R.Id
JOIN RolesModulos RM
ON R.Id = RM.RolId
JOIN Modulos M ON RM.ModuloId = M.Id AND A.Id = M.AplicacionId
LEFT JOIN SubModulos SM
ON M.Id = SM.ModuloId
WHERE UAR.UsuarioId = 1












SELECT A.Nombre APLICACION, R.Nombre ROL, M.Nombre MODULO, M.Nombre SUBMODULO FROM UsuariosAplicacionesRoles UAR
LEFT JOIN Aplicaciones A
ON UAR.AplicacionId = A.Id
LEFT JOIN Roles R
ON UAR.RolId = R.Id
LEFT JOIN RolesModulos RM
ON R.Id = RM.RolId
LEFT JOIN Modulos M ON RM.ModuloId = M.Id
LEFT JOIN RolesSubModulos RSM ON R.Id = RSM.RolId
LEFT JOIN SubModulos SM ON RSM.SubModuloId = SM.Id AND SM.ModuloId = M.Id
WHERE UAR.UsuarioId = 1
