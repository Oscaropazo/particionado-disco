# Práctica de Particionado de Discos en Ubuntu 22.04

## Requisitos

- Realizar un particionado **MBR** en un disco de **100 GiB**.
- Realizar un particionado **GPT** en un disco de **50 GiB**.
- Formatear las particiones con **ext4**.
- Crear archivos de texto en cada partición con información sobre su tamaño.
- Montar las particiones en **/media/sdX** y configurarlas en **/etc/fstab**.
- Subir la práctica a un repositorio público en GitHub.

---

## 1. Particionado MBR

### **1.1. Verificar discos disponibles**
```bash
lsblk
```

### **1.2. Crear particiones con fdisk**
```bash
sudo fdisk /dev/sdb
```
**Comandos dentro de fdisk:**
1. **`n`** → Nueva partición
2. **`p`** → Primaria
   - **Partición 1**: 20 GiB
   - **Partición 2**: 10 GiB
   - **Partición 3**: 15 GiB
3. **`n`** → Nueva partición extendida
4. **`n`** → Crear particiones lógicas dentro de la extendida
   - **Partición 5**: 10 GiB
   - **Partición 6**: 15 GiB
5. **`w`** → Guardar cambios y salir

### **1.3. Formatear particiones**
```bash
sudo mkfs.ext4 /dev/sdb1
sudo mkfs.ext4 /dev/sdb2
sudo mkfs.ext4 /dev/sdb3
sudo mkfs.ext4 /dev/sdb5
sudo mkfs.ext4 /dev/sdb6
```

### **1.4. Crear puntos de montaje**
```bash
sudo mkdir -p /media/sdb{1,2,3,5,6}
```

### **1.5. Agregar a /etc/fstab**
Obtener UUIDs:
```bash
blkid
```
Editar `/etc/fstab`:
```bash
sudo nano /etc/fstab
```
Agregar estas líneas (reemplazando los UUID reales):
```plaintext
UUID=xxxxx /media/sdb1 ext4 defaults 0 2
UUID=xxxxx /media/sdb2 ext4 defaults 0 2
UUID=xxxxx /media/sdb3 ext4 defaults 0 2
UUID=xxxxx /media/sdb5 ext4 defaults 0 2
UUID=xxxxx /media/sdb6 ext4 defaults 0 2
```
Montar las particiones:
```bash
sudo mount -a
```

### **1.6. Crear archivos en cada partición**
```bash
echo "Soy la partición 1 MBR y tengo un tamaño de 20GiB" | sudo tee /media/sdb1/info.txt
echo "Soy la partición 2 MBR y tengo un tamaño de 10GiB" | sudo tee /media/sdb2/info.txt
echo "Soy la partición 3 MBR y tengo un tamaño de 15GiB" | sudo tee /media/sdb3/info.txt
echo "Soy la partición 5 MBR y tengo un tamaño de 10GiB" | sudo tee /media/sdb5/info.txt
echo "Soy la partición 6 MBR y tengo un tamaño de 15GiB" | sudo tee /media/sdb6/info.txt
```

---

## 2. Particionado GPT

### **2.1. Crear particiones con gdisk**
```bash
sudo gdisk /dev/sdc
```
**Comandos dentro de gdisk:**
1. **`n`** → Nueva partición
   - **Partición 1**: 20 GiB
   - **Partición 2**: 15 GiB
2. **`w`** → Guardar cambios y salir

### **2.2. Formatear particiones**
```bash
sudo mkfs.ext4 /dev/sdc1
sudo mkfs.ext4 /dev/sdc2
```

### **2.3. Crear puntos de montaje**
```bash
sudo mkdir -p /media/sdc{1,2}
```

### **2.4. Agregar a /etc/fstab**
Obtener UUIDs:
```bash
blkid
```
Editar `/etc/fstab`:
```bash
sudo nano /etc/fstab
```
Agregar estas líneas:
```plaintext
UUID=xxxxx /media/sdc1 ext4 defaults 0 2
UUID=xxxxx /media/sdc2 ext4 defaults 0 2
```
Montar las particiones:
```bash
sudo mount -a
```

### **2.5. Crear archivos en cada partición**
```bash
echo "Soy la partición 1 GPT y tengo un tamaño de 20GiB" | sudo tee /media/sdc1/info.txt
echo "Soy la partición 2 GPT y tengo un tamaño de 15GiB" | sudo tee /media/sdc2/info.txt
```

---

## 3. Subida a GitHub

### **3.1. Subir desde la página web**
1. Ir al repositorio en GitHub.
2. Hacer clic en **"Add file" → "Upload files"**.
3. Seleccionar `particionado.md` y hacer clic en **"Commit changes"**.

### **3.2. Subir usando Git en Windows**
```bash
git clone https://github.com/TuUsuario/particionado-disco.git
cd particionado-disco
mv /ruta/al/archivo/particionado.md .
git add particionado.md
git commit -m "Subida de la práctica de particionado"
git push origin main
```

---

## 4. Entrega de la Práctica

- Asegurarse de que el archivo `particionado.md` esté en GitHub.
- Comentar la URL del repositorio en la plataforma de entrega.

---

¡Práctica completada! 🚀

