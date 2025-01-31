# WarpDB

![warpdb](https://github.com/user-attachments/assets/03326a51-5764-4c71-94df-d0b8e3f4345a)

**WarpDB** es una base de datos de documentos embebida para **Golang**, basada en **PostgreSQL**, con una interfaz similar a **MongoDB**. Diseñada para desarrolladores que necesitan un almacenamiento de documentos eficiente, rápido y sin necesidad de depender de un servidor externo de base de datos.

## 🚀 Características

- **Interfaz tipo MongoDB**: Métodos familiares como `InsertOne()`, `Find()`, `UpdateOne()`, `DeleteOne()`.
- **Soporte para dos modos de uso:**
  - **Modo Remoto**: Usa un servidor PostgreSQL externo como almacenamiento de documentos.
  - **Modo Embebido**: Ejecuta PostgreSQL localmente o en memoria.
- **Almacenamiento en JSONB**: Soporte nativo de PostgreSQL para JSON binario optimizado.
- **Índices GIN**: Consultas rápidas sobre documentos en modo remoto.
- **Alta performance en modo remoto**: Optimizado para minimizar la sobrecarga del intérprete SQL.

⚠️ **Nota:** En el modo embebido, inicialmente **no se soportará replicación ni optimización avanzada como MongoDB**. Se enfocará en proveer una API similar a MongoDB para crear y gestionar bases de datos de documentos.

---

## 📦 Instalación

```sh
 go get github.com/wirvii/warpdb
```

---

## 🛠 Uso Básico

```go
package main

import (
    "fmt"
    "log"
    "github.com/wirvii/warpdb"
)

func main() {
    // Iniciar la base de datos embebida o conectarse a un servidor remoto
    db, err := warpdb.NewDatabase(warpdb.Config{
        Mode: "remote", // Opciones: "remote", "embedded"
        ConnectionString: "postgres://user:password@localhost:5432/warpdb",
    })
    if err != nil {
        log.Fatal(err)
    }

    // Crear una colección
    users := db.Collection("users")

    // Insertar un documento
    doc := warpdb.Document{
        "name": "Juan",
        "age": 30,
    }
    users.InsertOne(doc)

    // Consultar documentos
    results := users.Find(warpdb.Filter{"age": warpdb.GreaterThan(25)})
    for _, r := range results {
        fmt.Println(r)
    }
}
```

---

## ⚡ Optimización y Performance (Solo en Modo Remoto)

Para lograr el máximo rendimiento en **modo remoto**, WarpDB utiliza:
- **Índices GIN** para acelerar consultas sobre JSONB.
- **Modo sincrónico opcional** para garantizar consistencia en entornos distribuidos.
- **Configuración avanzada** para minimizar la latencia de PostgreSQL.

⚠️ **Nota:** Estas optimizaciones **no aplican** al modo embebido por ahora.

---

## 🔄 Modo Embebido

WarpDB permite ejecutar PostgreSQL localmente o en memoria sin necesidad de un servidor externo. En este modo, la base de datos se mantiene en el sistema de archivos local o en memoria según la configuración.

Ejemplo de configuración en modo embebido:

```yaml
mode: "embedded"
storage:
  path: "./pg_data"
  in_memory: true # Opcional, true para ejecución solo en memoria
```

⚠️ **Limitaciones del Modo Embebido:**
- No soporta replicación ni clustering.
- No incluye optimizaciones avanzadas como MongoDB.
- Está diseñado para aplicaciones locales y entornos de desarrollo.

---

## 📜 Licencia

WarpDB es un proyecto **open-source** bajo la licencia **MIT**. ¡Siéntete libre de contribuir!

---

## 🏗️ Roadmap

- [ ] Implementar transacciones en documentos.
- [ ] Soporte para índices secundarios.
- [ ] API REST para consultas externas.
- [ ] Mejoras en optimización para modo embebido.

---

## 🤝 Contribuir

Si quieres ayudar a mejorar WarpDB:
1. Haz un fork del repositorio.
2. Crea una rama con tus cambios: `git checkout -b feature-nueva`.
3. Realiza un commit: `git commit -m "Añadir nueva funcionalidad"`.
4. Envía un Pull Request 🚀.

¡Gracias por apoyar el desarrollo de **WarpDB**! 🎉

