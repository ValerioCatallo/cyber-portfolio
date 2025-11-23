# CRASH — cups.service (status=203/EXEC) → ENOENT

## 🔍 Contesto

È stata copiata la unit originale da `/usr/lib/systemd/system/cups.service` in `/etc/systemd/system/cups.service`, modificando:

ExecStart=/usr/sbin/cupsd_fake


Il binario `cupsd_fake` non esiste → `status=203/EXEC` + ENOENT.

## 🔥 Errore

- errno: **ENOENT**
- meaning: file non trovato
- generato da: **systemd (PID 1)** durante l'apertura del binario

## 🧪 Riproduzione minima

1. Copia la unit in `/etc/systemd/system`
2. Modifica ExecStart con un binario inesistente
3. `systemctl daemon-reload && systemctl restart cups`

## 📎 Traccia essenziale

```text
openat(AT_FDCWD, "/usr/sbin/cupsd_fake", ...) = -1 ENOENT


🛠 Fix rapido

Ripristinare un percorso valido in ExecStart.

🏷 Note

    Le override in /etc/systemd/system sostituiscono completamente la unit originale

    203/EXEC appare quando l’eseguibile non può essere aperto

    L’errore è di systemd, non del servizio

