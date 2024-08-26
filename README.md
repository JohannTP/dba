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

@{
	foreach (var opcion in opcionesNovelUno)
	{
	    var subOpciones = opcionesNovelDos.Where(o => o.IdOpcionPadre == opcion.Id).ToList();
	
	    if (subOpciones.Any())
	    {
		<li class="nav-item dropdown">
		    <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown" href="#" role="button" aria-haspopup="true" aria-expanded="false">@opcion.Nombre</a>
		    <div class="dropdown-menu">
			@foreach (var subOpcion in subOpciones)
			{
			    <a class="dropdown-item" href="@subOpcion.Ruta">@subOpcion.Nombre</a>
			}
		    </div>
		</li>
	
	    }
	    else
	    {
		<li class="nav-item">
		    <a class="nav-link" href="@opcion.Ruta">@opcion.Nombre</a>
		</li>
	
	    }
	
	}
}






-------------------------------------------------------
APP_STAR()

//protected void Application_BeginRequest(object sender, EventArgs e)
        //{
        //    ValidarPermisos(sender, e);
        //}

        protected void ValidarPermisos(object sender, EventArgs e)
        {

            if (HttpContext.Current.Session == null || HttpContext.Current.Session["OpcionesNivelUno"] == null)
            {
                // Redirige a la página de no autorizado si la sesión no está disponible
                HttpContext.Current.Response.Redirect("~/Login/NoAutorizado");
                return;
            }


            //obtener url solicitada
            var requestUrl = HttpContext.Current.Request.Url.AbsolutePath;


            //excluir ciertas rutas de la validacion
            if (requestUrl.Equals("/Login", StringComparison.OrdinalIgnoreCase) ||
                requestUrl.Equals("/Login/IniciarSesion", StringComparison.OrdinalIgnoreCase) ||
                requestUrl.Equals("/", StringComparison.OrdinalIgnoreCase) ||
                requestUrl.Equals("/Login/Index", StringComparison.OrdinalIgnoreCase) ||
                requestUrl.Equals("/Login/NoAutorizado", StringComparison.OrdinalIgnoreCase) ||
                requestUrl.StartsWith("/Content", StringComparison.OrdinalIgnoreCase) ||
                requestUrl.StartsWith("/Scripts", StringComparison.OrdinalIgnoreCase))
            {
                return;
            }

            HttpContext context = HttpContext.Current;
            var opcionesNivelUno = context.Session["OpcionesNivelUno"] as List<OpcionesDTO>;
            var opcionesNivelDos = context.Session["OpcionesNivelDos"] as List<OpcionesDTO>;
            var opcionesNivelTres = context.Session["OpcionesNivelTres"] as List<OpcionesDTO>;

            //combinar la lista de permisos del usuario
            var opciones = opcionesNivelUno.Concat(opcionesNivelDos).Concat(opcionesNivelTres).ToList();

            bool accesoPermitido = opciones.Any(o => requestUrl.StartsWith(o.Ruta, StringComparison.OrdinalIgnoreCase));

            if (!accesoPermitido)
            {
                HttpContext.Current.Response.Redirect("~/Login/NoAutorizado");
            }
        }









