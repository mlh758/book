# Configuración

Nu tiene un número pequeño, pero creciente, de variables internas que puedes establecer cambios en su aspecto y funcionamiento. A continuación una lista de las variables actuales, sus tipos, y una descripción de cómo se utilizan:

| Variable        | Tipo           | Descripción  |
| ------------- | ------------- | ----- |
| path | tabla de cadenas | PATH para usar en búsqueda de binarios |
| env | fila | variables de entorno que serán pasadas a comandos externos |
| ctrlc_exit | booleano | salir o no de Nu después de presionar ctrl-c varias veces |
| table_mode | "light" o otro | habilitar tables livianas o normales |
| edit_mode | "vi" o "emacs" | cambia edición de línea a modo "vi" o "emacs" |

## Uso

### Configuración de variables

Para establecer una de estas variables, puedes usar `config --set`. Por ejemplo:

```
> config --set [edit_mode "vi"]
```

### Estableciendo una variable desde la tubería

Hay una manera adicional de establecer una variable, y es usar el contenido de la tubería como el valor deseado para la variable. Para esto usa la bandera `--set-into`:

```
> echo "bar" | config --set_into foo
```

Esto es de utilidad cuando se trabaja con las variables `env` y `path`.

### Listado de todas las variables.

Ejecutando el comando `config` sin argumentos mostrará una tabla de las preferencias de configuración actuales:

```
> config
━━━━━━━━━━━┯━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━
 edit_mode │ env            │ path             │ table_mode 
───────────┼────────────────┼──────────────────┼────────────
 emacs     │ [table: 1 row] │ [table: 10 rows] │ normal 
━━━━━━━━━━━┷━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━
```

Nota: si por el momento no has establecido variables de configuración, puede estar vacía.

### Obteniendo una variable

Usando la bandera `--get`, puedes conseguir el valor de una variable:

```
> config --get edit_mode
```

### Eliminando una variable

Para eliminar una variable de la configuración, usa la bandera `--remove`:

```
> config --remove edit_mode
```

### Borrar toda la configuración

Si deseas borrar toda la configuración y empezar de cero, puedes usar la bandera `--clear`. Por supuesto, tenga precaución con esto ya que una vez ejecutado el archivo de configuración también se eliminará.

```
> config --clear
```

### Encontrar dónde se almacena el archivo de configuración

El archivo de configuración se carga desde una ubicación predeterminada. Para encontrar esta ubicación en el sistema, puedes solicitarla usando la bandera `--path`:

```
config --path
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 <value> 
───────────────────────────────────────
 /home/nusheller/.config/nu/config.toml 
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Cargando la configuración desde un archivo

Es posible que desees cargar la configuración desde una ubicación distinta a la predeterminada. Para hacerlo, usa la bandera `--load`:

```
> config --load myconfiguration.toml
```

## Configurando Nu como shell de inicio de sesión

Para usar Nu como shell de inicio de sesión, necesitarás configurar las variables `path` y `env`. Con estos, obtendrás suficiente soporte para ejecutar comandos externos como shell de inicio de sesión.

Antes de cambiarlo, ejecuta Nu dentro de otra shell, como Bash. Luego, obtén el entorno y PATH desde esa shell con los siguientes comandos:

```
> config --set [path $nu:path]
> config --set [env $nu:env]
```

`$nu:path` y `$nu:env` son variables especiales que están prestablecidas a las variables PATH y entorno actuales respectivamente. Una vez que las estableces a la configuración, estarán disponibles cuando uses Nu como shell de inicio de sesión.

A continuación, en algunas distribuciones también deberás asegurarte de que Nu esté en la lista en `/etc/shells`:

```
❯ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/dash
/bin/bash
/bin/rbash
/usr/bin/screen
/usr/bin/fish
/home/jonathan/.cargo/bin/nu
```

Con esto, deberías de poder hacer `chsh` y establecer Nu como la shell de inicio de sesión. Luego de cerrar sesión, en el próximo inicio de sesión deberías de recibir un brillante mensaje de Nu.

## Configuración del prompt

Actualmente, la configuración del prompt es manejada instalando Nu con el soporte prompt proporcionado con [starship](https://github.com/starship/starship).

```
nushell on 📙 master [$] is 📦 v0.5.1 via 🦀 v1.40.0-nightly 
❯ 
```
Starship es un prompt divertido, colorido y sorprendentemente poderoso. Para configurarlo, sigue los pasos en su [manual de configuración](https://starship.rs/config/).
