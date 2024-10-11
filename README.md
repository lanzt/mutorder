# mutorder
Rule Border Mutation Generator for John The Ripper

Descripción rápida, despues la arreglo :P

Con esta herramienta evitaremos armar a mano una regla, en la que añadimos ya sea al inicio, al final o ambos, numeros y caracteres especiales.

![image](https://raw.githubusercontent.com/lanzt/blog/main/assets/images/THM/crackthehashlevel2/thmCrackthehashlevel2-google-hashcat-border-rules.png)

El funcionamiento es muy simple, el script busca un archivo llamado: `rules-syntax.txt`, en él agregariamos la forma en la que quisieramos que quedara la contraseña visualmente, el script buscara `0` para saber que quieres añadir numeros y `!` para saber que quieres añadir caracteres especiales, por ejemplo:

```bash
➧ cat rules-syntax.txt
password00
password!0
!password0
```

Lo que daria como resultado el formato de la regla:

```bash
➧ python3 rule-border-mutation-generator.py
# -----> password00
$[0-9] $[0-9]
# -----> password!0
$[!@#$%^&*()+=.?] $[0-9]
# -----> !password0
^[!@#$%^&*()+=.?] $[0-9]
```

Lo unico que quedaria es tomar ese output y agregarlo al archivo de reglas que usemos (`/etc/john/jonh.conf` o `/usr/share/john/john-local.conf`):

```bash
➧ cat /usr/share/john/john-local.conf
[...]
[List.Rules:Borders]
# -----> password00
$[0-9] $[0-9]
# -----> password!0
$[!@#$%^&*()+=.?] $[0-9]
# -----> !password0
^[!@#$%^&*()+=.?] $[0-9]
```

Y listos, la usariamos así:

```bash
➧ cat test.txt
carlitos
```

```bash
john --wordlist=test.txt --rules=Borders --stdout
carlitos67
carlitos#9
#carlitos3
```

> Aunque al final para evitar esto es mejor usar mascaras (mucho más claro de entender), pero me gustó el script y quise compartirlo :*
