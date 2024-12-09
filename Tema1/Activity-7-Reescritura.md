# Actividad #7 - Reescritura

Lee la documentación sobre mapeo de contenidos en Apache:
- http://httpd.apache.org/docs/current/urlmapping.html
- http://httpd.apache.org/docs/current/mod/mod_rewrite.html

Algunos ejemplos de redirecciones útiles:
https://www.elarraydejota.com/ejemplos-de-redirecciones-utiles-y-comunes-con-mod_rewrite-para-apache

Lleva a cabo los ejercicios propuestos en el siguiente enlace:
http://www.josedomingo.org/pledin/2011/10/ejemplos-del-modulo-rewrite-en-apache-2-2/

Nota: Si no utilizas ficheros .htaccess, incluye las directivas de reescritura dentro de la directiva "directory" correspondiente, tal como se muestra a continuación:

```apache
<Directory /var/www/html>
    Options FollowSymLinks
    RewriteEngine On
    RewriteBase /
    RewriteRule ^([a-z]+)/([0-9]+)/([0-9]+)$ operacion.php?op=$1&op1=$2&op2=$3
</Directory>
```

El fichero operacion.php:

```php
<?php
if (isset($_GET['op']) && isset($_GET['op1']) && isset($_GET['op2'])) {
    $op = $_GET["op"];
    $op1 = $_GET["op1"];
    $op2 = $_GET["op2"];
    $r = 0;
    if ($op == "suma") $r = $op1 + $op2;
    else if ($op == "resta") $r = $op1 - $op2;
    else if ($op == "multiplica") $r = $op1 * $op2;
    else if ($op == "divide") $r = $op1 / $op2;
    echo "op = " . $op . "<br>";
    echo "op1 = " . $op1 . " ";
    echo "op2 = " . $op2 . " ";
    echo "Resultado = " . $r;
}
?>
```

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)