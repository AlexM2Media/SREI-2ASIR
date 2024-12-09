# Actividad #6 - Expresiones regulares

## Ejercicios

### 1. Directorios en /www/ cuyo nombre consista en tres dígitos

Expresión regular: `^/www/[0-9]{3}$`

### 2. Ficheros: *.gif, *.jpeg, *.jpg, *.png

Expresión regular: `.+\.(gif|jpe?g|png)$`

### 3. Escribe una directiva para redireccionar todos los GIF a ficheros JPEG en otro servidor

```apache
RedirectMatch "(.*)\.gif$" "$1.jpg"
```

### 4. Números enteros y decimales

Expresión regular: `\d*\.?\d+`

### 5. Números de teléfono en el formato Americano: 123-123-1234

Expresión regular: `\b\d{3}-?\d{3}-?\d{4}\b`

### 6. Palabras

Expresión regular: `[a-zA-Z]+`

### 7. Códigos hexadecimales de color de 24 o 32 bits

Expresión regular: `(#|0x)?(?:[0-9A-F]{2}){3,4}`

### 8. Palabras de 4 letras

Expresión regular: `\b\w{4}\b`

### 9. Número entero sin signo

Expresión regular: `\b\d+\b`

### 10. Número entero con signo

Expresión regular: `[-+]?\d+`

### 11. Números reales

Expresión regular: `[-+]?([0-9]*\.[0-9]+|[0-9]+)`

### 12. Número reales con exponente

Expresión regular: `[-+]?[0-9]*\.?[0-9]+([eE][-+]?[0-9]+)?`

### 13. Email

Expresión regular: `\b[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}\b`

### 14. Números del 0 a 255

Expresión regular: `^([1]?[0-9][0-9]?|2[0-4][0-9]|25[0-5])$`

---
**Autor:** Alejandro Mateo - [@AlexM2Media](https://github.com/AlexM2Media)  
**Repositorio:** [SREI-2ASIR](https://github.com/AlexM2Media/SREI-2ASIR)  
**Web/Portfolio:** [alexm2.media](https://alexm2.media)
