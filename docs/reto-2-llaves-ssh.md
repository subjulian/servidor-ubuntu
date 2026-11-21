# Reto 2: SSH solo con llaves y sin login de root

Objetivo: autenticarse con un par de llaves en lugar de contraseña, y bloquear el login directo de `root`.

## Tarea 1: generar el par de llaves (en Windows)

```
ssh-keygen -t ed25519
```

- Se generó en el PC con Windows, nunca en la VM: la llave privada no debe estar en el servidor.
- Se protegió con una passphrase larga y aleatoria, guardada en un gestor de contraseñas.
- Resultado en `%USERPROFILE%\.ssh`: `id_ed25519` (privada, no se comparte) e `id_ed25519.pub` (pública, es la que va al servidor).

## Tarea 2: instalar la llave pública en la VM

La línea completa de `id_ed25519.pub` (empieza con `ssh-ed25519`) se pegó en `~/.ssh/authorized_keys` del usuario `julian`, y se ajustaron los permisos:

```
nano ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
ls -l ~/.ssh
```

Resultado: `drwx------` en `~/.ssh` y `-rw-------` en `authorized_keys`. Al conectar, SSH pide la passphrase de la llave en lugar de la contraseña de la cuenta.

## Tarea 3: configurar el servidor SSH

1. Se revisó que `/etc/ssh/sshd_config.d/` estuviera vacía (nada sobrescribía la configuración).
2. Se anotaron los valores efectivos con `sudo sshd -T | grep -Ei 'passwordauthentication|permitrootlogin'`.
3. Respaldo: `sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak`.
4. Edición con `sudo nano /etc/ssh/sshd_config` (se quitó el `#` de cada línea y se cambió el valor).
5. Se validó la sintaxis con `sudo sshd -t` y se comprobaron los valores efectivos antes de reiniciar.
6. Se aplicó el cambio con `sudo systemctl restart ssh`. El estado quedó `active (running)`.

| Directiva | Antes | Después |
|---|---|---|
| `PasswordAuthentication` | `yes` | `no` |
| `PermitRootLogin` | `prohibit-password` | `no` |

`KbdInteractiveAuthentication` ya estaba en `no`, así que no hubo otro camino para usar contraseña.

## Tarea 4: pruebas

Todas desde una ventana nueva de Windows, con una sesión ya conectada abierta como salvavidas.

| Prueba | Comando | Resultado |
|---|---|---|
| Con la llave | `ssh -p 2222 julian@127.0.0.1` | Pide la passphrase y entra. `hostname` responde `Ubuntu` |
| Forzando contraseña | `ssh -o PubkeyAuthentication=no -p 2222 julian@127.0.0.1` | `Permission denied (publickey)` |
| Como root | `ssh -p 2222 root@127.0.0.1` | `Permission denied (publickey)` |

Además, `systemctl is-enabled ssh.socket` responde `enabled`: SSH seguirá disponible tras reiniciar la VM.


---
[← Volver al índice](../README.md)
