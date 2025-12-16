Day 4: Script Execution Permissions

Cambiar los permisos de ejecución de un archivo.

```bash
ls -l

-rwxr-xr-- 1 user user 1234 Jun 1 12:00 myscript.sh
```


-   rwx   r-x   r--  </br>
│    │     │     │   </br>
│    │     │     └── otros (others) </br>
│    │     └──────── grupo (group) </br>
│    └────────────── dueño (user) </br>
└────────────────── tipo de archivo </br>

Sintaxis
chmod [quién][operador][permisos] archivo

Quién
| Carácter | Significado      |
| -------- | ---------------- |
| `u`      | dueño (user)    |
| `g`      | grupo (group)   |
| `o`      | otros (others)  |
| `a`      | todos (all)     |

Operador
| Carácter | Significado        |
| -------- | ------------------ |
| `+`      | agregar permiso    |
| `-`      | quitar permiso     |
| `=`      | establecer permiso |

Tipo de archivo
| Carácter | Significado    |
| -------- | -------------- |
| `-`      | archivo normal |
| `d`      | directorio     |
| `l`      | link simbólico |
| `c`      | dispositivo    |
| `b`      | bloque         |

Permisos
| Letra | Permiso | Archivo      | Directorio       |
| ----- | ------- | ------------ | ---------------- |
| `r`   | read    | leer archivo | listar contenido |
| `w`   | write   | modificar    | crear/borrar     |
| `x`   | execute | ejecutar     | entrar (`cd`)    |

Ej:
```bash
chmod u=rwx,g=rx,o=rx archivo
```