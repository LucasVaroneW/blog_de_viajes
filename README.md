## Proyecto Blog de viajes 🚀
El blog de viajes es un proyecto desarrollado con Node.Js y Express.Js con la finalidad que se allí se comparta la experiencia de cada usuario en sus viajes. Este blog esta desarrollado con Mysql. Además que desarrolle su propia API Rest para que otros usuarios puedan interactuar con el sitio desde otros sistemas.

## Para correr la aplicación

* Descargar o clonar el repositorio.
* Instalar node.js.
* hacer un npm install.
* Para correr la app hacer un node app.js.
* Para la base de datos usaremos Mysql.

## Conceptos aprendidos
* Paquetes de node.js
* Express.js
* Router
* Desarrollo de API Rest
* Desarrollo WEB con HTML, CSS, Javascript
* Git
* Github
* MySQL
* CRUD con MySQL
* Middlewares
* EJS
* Posts
* Login
* Registro de usuarios

# Editar y Eliminar

## Editar

Para editar debes crear un formulario donde precargues los valores


```HTML
<form action="/admin/procesar_editar" method="POST">
  <input type="hidden" name="id" value="<%=publicacion.id%>">
  <fieldset>
    <legend>Datos de publicación</legend>
    <p>
      <label for="titulo">Título</label>
      <br>
      <input type="text" id="titulo" name="titulo" required="true" value="<%=publicacion.titulo%>" />
    </p>
    <p>
      <label for="resumen">Resumen</label>
      <br>
      <textarea id="resumen" name="resumen"><%=publicacion.resumen%></textarea>
    </p>
    <p>
      <label for="contenido">Contenido</label>
      <br>
      <textarea id="contenido" name="contenido"><%=publicacion.contenido%></textarea>
    </p>

    <p>
      <input class="primary" type="submit" value="Continuar" />
    </p>
  </fieldset>
</form>
```

Luego el procesar del editar

```Javascript
router.post('/admin/procesar_editar', (peticion, respuesta) => {
  pool.getConnection((err, connection) => {
    const consulta = `
      UPDATE publicaciones
      SET
      titulo = ${connection.escape(peticion.body.titulo)},
      resumen = ${connection.escape(peticion.body.resumen)},
      contenido = ${connection.escape(peticion.body.contenido)}
      WHERE
      id = ${connection.escape(peticion.body.id)}
      AND
      autor_id = ${connection.escape(peticion.session.usuario.id)}
    `
    connection.query(consulta, (error, filas, campos) => {
      if (filas && filas.changedRows > 0){
        peticion.flash('mensaje', 'Publicación editada')
      }
      else{
        peticion.flash('mensaje', 'Publicación no editada')
      }
      respuesta.redirect("/admin/index")
    })
    connection.release()
  })
})
```


## Eliminar

```Javascript
router.get('/admin/procesar_eliminar/:id', (peticion, respuesta) => {
  pool.getConnection((err, connection) => {
    const consulta = `
      DELETE
      FROM
      publicaciones
      WHERE
      id = ${connection.escape(peticion.params.id)}
      AND
      autor_id = ${connection.escape(peticion.session.usuario.id)}
    `
    connection.query(consulta, (error, filas, campos) => {
      if (filas && filas.affectedRows > 0){
        peticion.flash('mensaje', 'Publicación eliminada')
      }
      else{
        peticion.flash('mensaje', 'Publicación no eliminada')
      }
      respuesta.redirect("/admin/index")
    })
    connection.release()
  })
})
```

## Autores
**Lucas Varone** - [LucasVaroneW](https://github.com/LucasVaroneW)
