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
