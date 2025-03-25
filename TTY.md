# 🖥️ Tratamiento TTY

El tratamiento TTY es fundamental para ejecutar comandos en un terminal remoto y manipular el entorno de la terminal. A continuación, se presentan los métodos para configurar TTY en sistemas Linux y Windows.

---

## 🐧 **Linux**

### 🔹 **Opción 1: Usando `script`**

```bash
script /dev/null -c bash
ctrl+z
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
stty rows 52 columns 180
```

1. Inicia un `bash` en modo "raw" para evitar que la terminal interfiera con las entradas.
2. Usa `stty` para configurar el tamaño de la terminal.
3. Configura las variables `TERM` y `SHELL` para asegurar la correcta ejecución del shell.

---

### 🔹 **Opción 2: Usando `pty` en Python**

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=screen-256color
[Ctrl + z]
echo $TERM
stty raw -echo
fg
[INTRO]
```

1. Usa Python para iniciar un shell interactivo.
2. Cambia el tipo de terminal a `screen-256color` para garantizar que el terminal se comporte adecuadamente.

---

### 🔹 **Comandos adicionales**

```bash
stty raw -echo; fg; ls; export SHELL=/bin/bash; export TERM=screen; stty rows 38 columns 116; reset;
```

- Configura el terminal para asegurar que esté en modo interactivo y preparado para la ejecución de comandos.

---

```bash
script -q /dev/null -c bash
/usr/bin/script -qc /bin/bash /dev/null
```

- Usa el comando `script` para registrar la sesión de shell y mejorar la persistencia del entorno.

---

## 💻 **Windows**

Para trabajar con TTY en Windows, puedes utilizar una herramienta muy útil llamada **ConPtyShell**.

👉 [ConPtyShell en GitHub](https://github.com/antonioCoco/ConPtyShell)

Con esta herramienta podrás gestionar una sesión interactiva en Windows, similar a las opciones mencionadas para Linux.

---
