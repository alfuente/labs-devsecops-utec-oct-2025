# Guía de Laboratorio: Configuración de Entorno DevSecOps con GitLab

## 📋 Objetivo
Configurar tu entorno de desarrollo y crear tu propio repositorio del proyecto Juice Shop para las prácticas del curso DevSecOps.

## Índice
1. [Crear Cuenta en GitLab](#1-crear-cuenta-en-gitlab)
2. [Instalar Visual Studio Code](#2-instalar-visual-studio-code)
3. [Configurar Llaves SSH](#3-configurar-llaves-ssh)
4. [Clonar Repositorio del Curso](#4-clonar-repositorio-del-curso)
5. [Crear tu Proyecto Privado](#5-crear-tu-proyecto-privado)
6. [Extraer y Copiar el Código de Juice Shop](#6-extraer-y-copiar-el-código-de-juice-shop)
7. [Realizar tu Primer Commit](#7-realizar-tu-primer-commit)

---

## 1. Crear Cuenta en GitLab

### Pasos para Registro

1. **Acceder al sitio web**
   - Navega a [https://gitlab.com](https://gitlab.com)
   - Haz clic en el botón **"Register"** o **"Registrarse"**

2. **Completar el formulario de registro**
   - **Nombre completo**: Ingresa tu nombre
   - **Nombre de usuario**: Elige un nombre de usuario único
   - **Correo electrónico**: Proporciona un email válido
   - **Contraseña**: Crea una contraseña segura (mínimo 8 caracteres)

3. **Verificar el correo electrónico**
   - Revisa tu bandeja de entrada
   - Haz clic en el enlace de verificación enviado por GitLab
   - Confirma tu cuenta

4. **Configuración inicial**
   - Selecciona tu rol (Developer, DevOps Engineer, etc.)
   - Indica el uso que le darás a GitLab
   - Completa el onboarding básico

---

## 2. Instalar Visual Studio Code

### Windows

1. **Descargar el instalador**
   - Visita [https://code.visualstudio.com](https://code.visualstudio.com)
   - Haz clic en **"Download for Windows"**
   - Descarga el archivo `VSCodeUserSetup-x64-{version}.exe`

2. **Ejecutar el instalador**
   - Haz doble clic en el archivo descargado
   - Acepta el acuerdo de licencia
   - Selecciona la ubicación de instalación

3. **Opciones de instalación recomendadas**
   - ✅ Agregar "Abrir con Code" al menú contextual
   - ✅ Agregar a PATH
   - ✅ Registrar Code como editor para tipos de archivo compatibles
   - ✅ Crear icono en el escritorio (opcional)

4. **Finalizar instalación**
   - Haz clic en **"Install"**
   - Espera a que complete
   - Haz clic en **"Finish"**

### macOS

1. **Descargar el instalador**
   - Visita [https://code.visualstudio.com](https://code.visualstudio.com)
   - Haz clic en **"Download for Mac"**
   - Descarga el archivo `.zip`

2. **Instalar la aplicación**
   - Abre el archivo `.zip` descargado
   - Arrastra **Visual Studio Code.app** a la carpeta **Applications**

3. **Abrir Visual Studio Code**
   - Ve a la carpeta Applications
   - Haz doble clic en Visual Studio Code
   - Si aparece advertencia de seguridad, haz clic derecho > Abrir

4. **Agregar al PATH (opcional)**
   ```bash
   # Abre VSCode, presiona Cmd+Shift+P y busca "Shell Command: Install 'code' command in PATH"
   ```

### Linux (Ubuntu/Debian)

#### Método 1: Usando .deb

1. **Descargar el paquete .deb**
   ```bash
   cd ~/Downloads
   wget https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64 -O vscode.deb
   ```

2. **Instalar el paquete**
   ```bash
   sudo apt install ./vscode.deb
   ```

3. **Actualizar repositorios**
   ```bash
   sudo apt update
   sudo apt upgrade
   ```

#### Método 2: Usando repositorio oficial

1. **Importar la clave GPG de Microsoft**
   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
   sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
   ```

2. **Agregar el repositorio**
   ```bash
   sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
   ```

3. **Instalar VSCode**
   ```bash
   sudo apt update
   sudo apt install code
   ```

### Linux (Fedora/RHEL/CentOS)

1. **Importar la clave GPG**
   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```

2. **Agregar el repositorio**
   ```bash
   sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
   ```

3. **Instalar VSCode**
   ```bash
   # Para dnf (Fedora 22+)
   sudo dnf check-update
   sudo dnf install code

   # Para yum
   sudo yum check-update
   sudo yum install code
   ```

---

## 3. Configurar Llaves SSH

> **Nota**: Es recomendable configurar las llaves SSH antes de clonar repositorios para facilitar el trabajo futuro.

### Generar Llaves SSH

#### En Windows (PowerShell)

1. **Abrir PowerShell**

2. **Generar la llave SSH**
   ```powershell
   # Crear directorio .ssh si no existe
   mkdir $HOME\.ssh -Force
   
   # Generar llave SSH
   ssh-keygen -t ed25519 -C "tu-email@ejemplo.com"
   
   # Si tu sistema no soporta ed25519, usa RSA
   ssh-keygen -t rsa -b 4096 -C "tu-email@ejemplo.com"
   ```

3. **Durante la generación**
   ```
   Enter file in which to save the key (C:\Users\TuUsuario\.ssh\id_ed25519): [Presiona Enter]
   Enter passphrase (empty for no passphrase): [Opcional: Ingresa contraseña]
   Enter same passphrase again: [Repite la contraseña]
   ```

4. **Copiar la llave pública**
   ```powershell
   # Ver y copiar la llave pública
   Get-Content $HOME\.ssh\id_ed25519.pub
   
   # O copiarla al portapapeles directamente
   Get-Content $HOME\.ssh\id_ed25519.pub | Set-Clipboard
   ```

#### En macOS/Linux (Bash)

1. **Abrir Terminal**

2. **Generar la llave SSH**
   ```bash
   # Generar llave SSH
   ssh-keygen -t ed25519 -C "tu-email@ejemplo.com"
   
   # Si tu sistema no soporta ed25519, usa RSA
   ssh-keygen -t rsa -b 4096 -C "tu-email@ejemplo.com"
   ```

3. **Durante la generación**
   ```
   Enter file in which to save the key (/home/usuario/.ssh/id_ed25519): [Presiona Enter]
   Enter passphrase (empty for no passphrase): [Opcional: Ingresa contraseña]
   Enter same passphrase again: [Repite la contraseña]
   ```

4. **Iniciar el agente SSH**
   ```bash
   # Iniciar el agente SSH en segundo plano
   eval "$(ssh-agent -s)"
   
   # Agregar tu llave SSH al agente
   ssh-add ~/.ssh/id_ed25519
   ```

5. **Copiar la llave pública**
   ```bash
   # Ver la llave pública
   cat ~/.ssh/id_ed25519.pub
   
   # En macOS, copiar al portapapeles
   pbcopy < ~/.ssh/id_ed25519.pub
   
   # En Linux (con xclip)
   xclip -selection clipboard < ~/.ssh/id_ed25519.pub
   
   # O instalar xclip primero
   sudo apt install xclip  # Ubuntu/Debian
   ```

### Agregar Llave SSH a GitLab

1. **Acceder a configuración de SSH**
   - En GitLab, haz clic en tu avatar (esquina superior derecha)
   - Selecciona **"Preferences"** o **"Preferencias"**
   - En el menú lateral, haz clic en **"SSH Keys"**

2. **Agregar nueva llave**
   - En el campo **"Key"**: Pega la llave pública completa que copiaste
   - En el campo **"Title"**: Dale un nombre descriptivo (ej: "Mi Laptop Windows", "MacBook Pro")
   - **Expiration date**: (Opcional) Establece una fecha de expiración
   - **Usage type**: Deja "Authentication & Signing" seleccionado

3. **Guardar la llave**
   - Haz clic en **"Add key"**
   - La llave aparecerá en tu lista de llaves SSH

### Verificar Conexión SSH

#### Windows (PowerShell)
```powershell
ssh -T git@gitlab.com
```

#### macOS/Linux (Bash)
```bash
ssh -T git@gitlab.com
```

**Respuesta esperada:**
```
Welcome to GitLab, @tu-usuario!
```

Si es la primera vez, aparecerá:
```
The authenticity of host 'gitlab.com' can't be established.
Are you sure you want to continue connecting (yes/no)? yes
```

---

## 4. Clonar Repositorio del Curso

En esta sección clonaremos el repositorio que contiene todos los materiales del curso DevSecOps.

### Prerequisitos

Asegúrate de tener Git instalado:

**Windows:**
- Descarga Git desde [https://git-scm.com/download/win](https://git-scm.com/download/win)
- Durante la instalación, selecciona "Git from the command line and also from 3rd-party software"

**macOS:**
```bash
# Verificar si Git ya está instalado
git --version

# Si no está instalado, usar Homebrew
brew install git
```

**Linux:**
```bash
# Ubuntu/Debian
sudo apt install git

# Fedora/RHEL
sudo dnf install git
```

### Clonar el Repositorio del Curso

#### Usando PowerShell (Windows)

1. **Abrir PowerShell**
   - Presiona `Win + X` y selecciona "Windows PowerShell"
   - O busca "PowerShell" en el menú Inicio

2. **Crear y navegar al directorio de trabajo**
   ```powershell
   # Crear carpeta para los proyectos del curso
   mkdir C:\DevSecOps-UTEC
   cd C:\DevSecOps-UTEC
   ```

3. **Configurar Git (primera vez)**
   ```powershell
   # Configurar nombre y email (usa tu email de GitLab)
   git config --global user.name "Tu Nombre"
   git config --global user.email "tu-email@ejemplo.com"
   
   # Verificar configuración
   git config --list
   ```

4. **Clonar el repositorio del curso**
   ```powershell
   # Clonar usando HTTPS (se pedirá autenticación)
   git clone https://gitlab.com/alejandro2025/labs-devsecops-utec-2025.git
   
   # O usando SSH (si ya configuraste las llaves)
   git clone git@gitlab.com:alejandro2025/labs-devsecops-utec-2025.git
   ```

5. **Verificar la clonación**
   ```powershell
   # Entrar al repositorio
   cd labs-devsecops-utec-2025
   
   # Listar contenido
   dir
   
   # Verificar que existe la carpeta Code
   dir Code
   
   # Verificar que existe el archivo juice-shop-devsecops.zip
   dir Code\juice-shop-devsecops.zip
   ```

#### Usando Terminal (macOS/Linux)

1. **Abrir Terminal**
   - **macOS**: Presiona `Cmd + Space`, escribe "Terminal"
   - **Linux**: Presiona `Ctrl + Alt + T`

2. **Crear y navegar al directorio de trabajo**
   ```bash
   # Crear carpeta para los proyectos del curso
   mkdir ~/DevSecOps-UTEC
   cd ~/DevSecOps-UTEC
   ```

3. **Configurar Git (primera vez)**
   ```bash
   # Configurar nombre y email (usa tu email de GitLab)
   git config --global user.name "Tu Nombre"
   git config --global user.email "tu-email@ejemplo.com"
   
   # Verificar configuración
   git config --list
   ```

4. **Clonar el repositorio del curso**
   ```bash
   # Clonar usando HTTPS (se pedirá autenticación)
   git clone https://gitlab.com/alejandro2025/labs-devsecops-utec-2025.git
   
   # O usando SSH (si ya configuraste las llaves)
   git clone git@gitlab.com:alejandro2025/labs-devsecops-utec-2025.git
   ```

5. **Verificar la clonación**
   ```bash
   # Entrar al repositorio
   cd labs-devsecops-utec-2025
   
   # Listar contenido
   ls -la
   
   # Verificar que existe la carpeta Code
   ls -l Code
   
   # Verificar que existe el archivo juice-shop-devsecops.zip
   ls -lh Code/juice-shop-devsecops.zip
   ```

### Estructura Esperada

Después de clonar, deberías ver una estructura similar a:

```
labs-devsecops-utec-2025/
├── Code/
│   ├── juice-shop-devsecops.zip  ← Este es el archivo que necesitamos
│   └── ... (otros archivos del curso)
├── README.md
└── ... (otros directorios y archivos)
```

---

## 5. Crear tu Proyecto Privado

Ahora crearás tu propio repositorio privado donde trabajarás con el código de Juice Shop durante el curso.

### Pasos para Crear el Proyecto

1. **Iniciar sesión en GitLab**
   - Accede a [https://gitlab.com](https://gitlab.com)
   - Ingresa con tus credenciales

2. **Crear nuevo proyecto**
   - Haz clic en el botón **"New project"** o el ícono **"+"** en la barra superior
   - Selecciona **"Create blank project"**

3. **Configurar el proyecto**
   - **Project name**: `juice-shop-devsecops`
   - **Project slug**: `juice-shop-devsecops` (se genera automáticamente)
   - **Project description**: `Proyecto de laboratorio DevSecOps - Juice Shop`
   - **Visibility Level**: Selecciona **"Private"** 🔒
   - **Initialize repository with a README**: ☐ **DESMARCA** esta opción (lo haremos manualmente)
   - **Enable Static Application Security Testing (SAST)**: ☐ Desmarcado (lo configuraremos después)

4. **Crear el proyecto**
   - Haz clic en **"Create project"**
   - Serás redirigido a la página del proyecto vacío

5. **Copiar la URL del repositorio**
   - En la página del proyecto verás instrucciones para Git Command line
   - Busca la sección con las URLs de clonación
   - Copia la **URL SSH**: `git@gitlab.com:tu-usuario/juice-shop-devsecops.git`
   - O la **URL HTTPS**: `https://gitlab.com/tu-usuario/juice-shop-devsecops.git`

### Clonar tu Nuevo Repositorio Vacío

#### PowerShell (Windows)

```powershell
# Navegar a tu directorio de trabajo
cd C:\DevSecOps-UTEC

# Clonar tu nuevo repositorio (usando SSH)
git clone git@gitlab.com:tu-usuario/juice-shop-devsecops.git

# O usando HTTPS
git clone https://gitlab.com/tu-usuario/juice-shop-devsecops.git

# Entrar al nuevo repositorio
cd juice-shop-devsecops

# Verificar que está vacío
git status
```

#### Terminal (macOS/Linux)

```bash
# Navegar a tu directorio de trabajo
cd ~/DevSecOps-UTEC

# Clonar tu nuevo repositorio (usando SSH)
git clone git@gitlab.com:tu-usuario/juice-shop-devsecops.git

# O usando HTTPS
git clone https://gitlab.com/tu-usuario/juice-shop-devsecops.git

# Entrar al nuevo repositorio
cd juice-shop-devsecops

# Verificar que está vacío
git status
```

Ahora deberías tener dos directorios en `C:\DevSecOps-UTEC` (Windows) o `~/DevSecOps-UTEC` (macOS/Linux):
- `labs-devsecops-utec-2025/` - Repositorio del curso (solo lectura)
- `juice-shop-devsecops/` - Tu repositorio personal (aquí trabajarás)

---

## 6. Extraer y Copiar el Código de Juice Shop

En esta sección extraeremos el archivo ZIP y copiaremos el contenido a tu nuevo repositorio.

### Extraer el Archivo ZIP

#### Windows

**Opción 1: Usando la Interfaz Gráfica**

1. **Navegar al archivo**
   - Abre el Explorador de Archivos
   - Ve a `C:\DevSecOps-UTEC\labs-devsecops-utec-2025\Code`
   - Busca el archivo `juice-shop-devsecops.zip`

2. **Extraer el contenido**
   - Haz clic derecho en `juice-shop-devsecops.zip`
   - Selecciona **"Extraer todo..."**
   - Cambia la ruta de destino a: `C:\DevSecOps-UTEC\temp-juice-shop`
   - Haz clic en **"Extraer"**

**Opción 2: Usando PowerShell**

```powershell
# Navegar al directorio del curso
cd C:\DevSecOps-UTEC\labs-devsecops-utec-2025\Code

# Crear directorio temporal para extraer
mkdir C:\DevSecOps-UTEC\temp-juice-shop

# Extraer el archivo ZIP
Expand-Archive -Path .\juice-shop-devsecops.zip -DestinationPath C:\DevSecOps-UTEC\temp-juice-shop -Force

# Verificar el contenido extraído
dir C:\DevSecOps-UTEC\temp-juice-shop
```

#### macOS

**Opción 1: Usando Finder**

1. **Navegar al archivo**
   - Abre Finder
   - Ve a `~/DevSecOps-UTEC/labs-devsecops-utec-2025/Code`
   - Busca el archivo `juice-shop-devsecops.zip`

2. **Extraer el contenido**
   - Haz doble clic en `juice-shop-devsecops.zip`
   - Esto creará una carpeta con el contenido extraído

**Opción 2: Usando Terminal**

```bash
# Navegar al directorio del curso
cd ~/DevSecOps-UTEC/labs-devsecops-utec-2025/Code

# Crear directorio temporal para extraer
mkdir -p ~/DevSecOps-UTEC/temp-juice-shop

# Extraer el archivo ZIP
unzip juice-shop-devsecops.zip -d ~/DevSecOps-UTEC/temp-juice-shop

# Verificar el contenido extraído
ls -la ~/DevSecOps-UTEC/temp-juice-shop
```

#### Linux

```bash
# Navegar al directorio del curso
cd ~/DevSecOps-UTEC/labs-devsecops-utec-2025/Code

# Crear directorio temporal para extraer
mkdir -p ~/DevSecOps-UTEC/temp-juice-shop

# Extraer el archivo ZIP
unzip juice-shop-devsecops.zip -d ~/DevSecOps-UTEC/temp-juice-shop

# Verificar el contenido extraído
ls -la ~/DevSecOps-UTEC/temp-juice-shop
```

### Copiar el Contenido a tu Repositorio

#### Windows - Usando PowerShell

```powershell
# IMPORTANTE: Asegúrate de estar en el directorio correcto
cd C:\DevSecOps-UTEC\juice-shop-devsecops

# Método 1: Copiar todo el contenido usando Copy-Item
# Nota: Ajusta la ruta según cómo se haya extraído el ZIP
Copy-Item -Path "C:\DevSecOps-UTEC\temp-juice-shop\*" -Destination "C:\DevSecOps-UTEC\juice-shop-devsecops\" -Recurse -Force

# Si el ZIP creó una subcarpeta, ajusta la ruta:
# Copy-Item -Path "C:\DevSecOps-UTEC\temp-juice-shop\juice-shop-devsecops\*" -Destination "C:\DevSecOps-UTEC\juice-shop-devsecops\" -Recurse -Force

# Método 2: Copiar usando Robocopy (más robusto)
robocopy "C:\DevSecOps-UTEC\temp-juice-shop" "C:\DevSecOps-UTEC\juice-shop-devsecops" /E /NFL /NDL

# Verificar que los archivos se copiaron
dir

# Verificar algunos archivos clave
dir package.json
dir Dockerfile
```

#### Windows - Usando Explorador de Archivos

1. **Abrir dos ventanas del Explorador**
   - Ventana 1: `C:\DevSecOps-UTEC\temp-juice-shop`
   - Ventana 2: `C:\DevSecOps-UTEC\juice-shop-devsecops`

2. **Seleccionar todo el contenido**
   - En la ventana del `temp-juice-shop`, presiona `Ctrl + A`
   - Esto selecciona todos los archivos y carpetas

3. **Copiar y pegar**
   - Presiona `Ctrl + C` para copiar
   - Ve a la ventana de `juice-shop-devsecops`
   - Presiona `Ctrl + V` para pegar

4. **Confirmar**
   - Si pregunta sobre reemplazar archivos, confirma
   - Espera a que termine la copia

#### macOS - Usando Terminal

```bash
# IMPORTANTE: Asegúrate de estar en el directorio correcto
cd ~/DevSecOps-UTEC/juice-shop-devsecops

# Método 1: Copiar usando cp (simple)
# Nota: Ajusta la ruta según cómo se haya extraído el ZIP
cp -R ~/DevSecOps-UTEC/temp-juice-shop/* ~/DevSecOps-UTEC/juice-shop-devsecops/

# Si el ZIP creó una subcarpeta, ajusta la ruta:
# cp -R ~/DevSecOps-UTEC/temp-juice-shop/juice-shop-devsecops/* ~/DevSecOps-UTEC/juice-shop-devsecops/

# Método 2: Copiar usando rsync (recomendado, más robusto)
rsync -av ~/DevSecOps-UTEC/temp-juice-shop/ ~/DevSecOps-UTEC/juice-shop-devsecops/

# Copiar también archivos ocultos (importante para .gitignore, etc.)
cp -R ~/DevSecOps-UTEC/temp-juice-shop/.* ~/DevSecOps-UTEC/juice-shop-devsecops/ 2>/dev/null

# Verificar que los archivos se copiaron
ls -la

# Verificar algunos archivos clave
ls -l package.json
ls -l Dockerfile
```

#### macOS - Usando Finder

1. **Abrir dos ventanas de Finder**
   - Ventana 1: `~/DevSecOps-UTEC/temp-juice-shop`
   - Ventana 2: `~/DevSecOps-UTEC/juice-shop-devsecops`

2. **Seleccionar todo el contenido**
   - En la ventana del `temp-juice-shop`, presiona `Cmd + A`
   - Esto selecciona todos los archivos y carpetas

3. **Copiar y pegar**
   - Presiona `Cmd + C` para copiar
   - Ve a la ventana de `juice-shop-devsecops`
   - Presiona `Cmd + V` para pegar

4. **Ver archivos ocultos** (opcional pero recomendado)
   - Presiona `Cmd + Shift + .` para mostrar archivos ocultos
   - Esto te permitirá ver archivos como `.gitignore`

#### Linux - Usando Terminal

```bash
# IMPORTANTE: Asegúrate de estar en el directorio correcto
cd ~/DevSecOps-UTEC/juice-shop-devsecops

# Método 1: Copiar usando cp
# Nota: Ajusta la ruta según cómo se haya extraído el ZIP
cp -R ~/DevSecOps-UTEC/temp-juice-shop/* ~/DevSecOps-UTEC/juice-shop-devsecops/

# Si el ZIP creó una subcarpeta, ajusta la ruta:
# cp -R ~/DevSecOps-UTEC/temp-juice-shop/juice-shop-devsecops/* ~/DevSecOps-UTEC/juice-shop-devsecops/

# Método 2: Copiar usando rsync (recomendado)
rsync -av ~/DevSecOps-UTEC/temp-juice-shop/ ~/DevSecOps-UTEC/juice-shop-devsecops/

# Copiar también archivos ocultos
shopt -s dotglob
cp -R ~/DevSecOps-UTEC/temp-juice-shop/* ~/DevSecOps-UTEC/juice-shop-devsecops/
shopt -u dotglob

# Verificar que los archivos se copiaron
ls -la

# Verificar algunos archivos clave
ls -l package.json
ls -l Dockerfile
```

### Limpiar Directorio Temporal (Opcional)

Una vez que hayas verificado que todo se copió correctamente:

**Windows:**
```powershell
Remove-Item -Path C:\DevSecOps-UTEC\temp-juice-shop -Recurse -Force
```

**macOS/Linux:**
```bash
rm -rf ~/DevSecOps-UTEC/temp-juice-shop
```

---

## 7. Realizar tu Primer Commit

Ahora que tienes el código de Juice Shop en tu repositorio, es momento de hacer el primer commit y subirlo a GitLab.

### Verificar el Estado del Repositorio

#### PowerShell (Windows)

```powershell
# Asegúrate de estar en tu repositorio
cd C:\DevSecOps-UTEC\juice-shop-devsecops

# Ver el estado de Git
git status
```

#### Terminal (macOS/Linux)

```bash
# Asegúrate de estar en tu repositorio
cd ~/DevSecOps-UTEC/juice-shop-devsecops

# Ver el estado de Git
git status
```

**Salida esperada:**
```
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .dockerignore
        .gitignore
        Dockerfile
        README.md
        package.json
        ... (muchos más archivos)

nothing added to commit but untracked files present (use "git add" to track)
```

### Agregar Archivos al Staging Area

```bash
# Agregar TODOS los archivos al staging
git add .

# Verificar que se agregaron
git status
```

**Salida esperada después de `git add .`:**
```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .dockerignore
        new file:   .gitignore
        new file:   Dockerfile
        new file:   README.md
        new file:   package.json
        ... (lista de todos los archivos)
```

### Crear el Primer Commit

```bash
# Crear el commit con un mensaje descriptivo
git commit -m "Initial commit: Proyecto Juice Shop para laboratorio DevSecOps"

# Verificar el commit
git log --oneline
```

**Salida esperada:**
```
abc1234 (HEAD -> main) Initial commit: Proyecto Juice Shop para laboratorio DevSecOps
```

### Subir los Cambios a GitLab (Push)

#### Si tu rama principal es "main"

```bash
# Subir los cambios al repositorio remoto
git push -u origin main
```

#### Si tu rama principal es "master"

```bash
# Subir los cambios al repositorio remoto
git push -u origin master
```

#### Si no estás seguro de la rama

```bash
# Ver tu rama actual
git branch

# Ver las ramas remotas
git branch -r

# Subir a la rama correspondiente
git push -u origin $(git branch --show-current)
```

**Salida esperada durante el push:**
```
Enumerating objects: 156, done.
Counting objects: 100% (156/156), done.
Delta compression using up to 8 threads
Compressing objects: 100% (134/134), done.
Writing objects: 100% (156/156), 2.45 MiB | 3.67 MiB/s, done.
Total 156 (delta 12), reused 0 (delta 0), pack-reused 0
remote: 
remote: To create a merge request for main, visit:
remote:   https://gitlab.com/tu-usuario/juice-shop-devsecops/-/merge_requests/new?merge_request%5Bsource_branch%5D=main
remote: 
To gitlab.com:tu-usuario/juice-shop-devsecops.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

### Verificar en GitLab

1. **Abrir tu navegador**
   - Ve a `https://gitlab.com/tu-usuario/juice-shop-devsecops`

2. **Verificar el contenido**
   - Deberías ver todos los archivos del proyecto
   - El commit inicial debería aparecer en el historial
   - La fecha y hora del commit deberían coincidir

3. **Verificar archivos importantes**
   - ✅ README.md
   - ✅ package.json
   - ✅ Dockerfile
   - ✅ .gitignore
   - ✅ Carpetas del proyecto (src, config, etc.)

---

## 8. Abrir el Proyecto en Visual Studio Code

### Desde la Terminal/PowerShell

**Windows:**
```powershell
cd C:\DevSecOps-UTEC\juice-shop-devsecops
code .
```

**macOS/Linux:**
```bash
cd ~/DevSecOps-UTEC/juice-shop-devsecops
code .
```

### Desde VSCode Directamente

1. **Abrir VSCode**
2. **File** → **Open Folder** (o `Ctrl+K Ctrl+O` en Windows/Linux, `Cmd+K Cmd+O` en macOS)
3. **Navegar** a tu carpeta `juice-shop-devsecops`
4. **Seleccionar** la carpeta y hacer clic en **"Seleccionar carpeta"**

### Extensiones Recomendadas para el Curso

Una vez abierto el proyecto, instala estas extensiones:

1. **GitLens** - Git supercharged
   ```
   Presiona Ctrl+Shift+X → Busca "GitLens" → Install
   ```

2. **Docker** - Gestión de contenedores
   ```
   Busca "Docker" (de Microsoft) → Install
   ```

3. **ESLint** - Linting de JavaScript
   ```
   Busca "ESLint" → Install
   ```

4. **GitLab Workflow** - Integración con GitLab
   ```
   Busca "GitLab Workflow" → Install
   ```

5. **YAML** - Soporte para archivos YAML (para CI/CD)
   ```
   Busca "YAML" (de Red Hat) → Install
   ```

---

## Resumen de Estructura de Directorios

Al finalizar esta guía, deberías tener la siguiente estructura:

**Windows:**
```
C:\DevSecOps-UTEC\
├── labs-devsecops-utec-2025\       ← Repositorio del curso (solo lectura)
│   ├── Code\
│   │   └── juice-shop-devsecops.zip
│   └── ...
└── juice-shop-devsecops\            ← Tu repositorio personal (trabajo activo)
    ├── .git\                        ← Control de versiones
    ├── .gitignore
    ├── Dockerfile
    ├── README.md
    ├── package.json
    └── ... (todos los archivos del proyecto)
```

**macOS/Linux:**
```
~/DevSecOps-UTEC/
├── labs-devsecops-utec-2025/       ← Repositorio del curso (solo lectura)
│   ├── Code/
│   │   └── juice-shop-devsecops.zip
│   └── ...
└── juice-shop-devsecops/            ← Tu repositorio personal (trabajo activo)
    ├── .git/                        ← Control de versiones
    ├── .gitignore
    ├── Dockerfile
    ├── README.md
    ├── package.json
    └── ... (todos los archivos del proyecto)
```

---

## Comandos Git Útiles para el Curso

### Comandos de Uso Diario

```bash
# Ver estado del repositorio (usa esto frecuentemente)
git status

# Ver cambios en archivos
git diff

# Ver historial de commits
git log --oneline
git log --graph --oneline --all

# Ver información del remoto
git remote -v
```

### Agregar y Commitear Cambios

```bash
# Agregar un archivo específico
git add archivo.txt

# Agregar múltiples archivos
git add archivo1.txt archivo2.txt

# Agregar todos los archivos modificados
git add .

# Agregar todos los archivos de una carpeta
git add carpeta/

# Ver qué está en staging
git status

# Hacer commit
git commit -m "Descripción clara de los cambios"

# Agregar y commitear en un solo comando (solo archivos tracked)
git commit -am "Mensaje del commit"
```

### Sincronizar con GitLab

```bash
# Subir cambios al repositorio remoto
git push

# Bajar cambios del repositorio remoto
git pull

# Ver diferencias con el remoto
git fetch
git diff origin/main
```

### Trabajar con Ramas (para el curso)

```bash
# Ver ramas locales
git branch

# Ver todas las ramas (locales y remotas)
git branch -a

# Crear nueva rama para una característica
git branch feature/nueva-funcionalidad

# Cambiar a una rama
git checkout feature/nueva-funcionalidad

# Crear y cambiar a nueva rama en un solo comando
git checkout -b feature/mi-feature

# Volver a la rama principal
git checkout main

# Eliminar rama local
git branch -d feature/mi-feature

# Fusionar una rama a main
git checkout main
git merge feature/mi-feature
```

### Deshacer Cambios

```bash
# Descartar cambios en un archivo (antes de add)
git checkout -- archivo.txt

# Quitar archivo del staging (después de add, antes de commit)
git reset HEAD archivo.txt

# Ver el último commit
git show

# Revertir el último commit (crea un nuevo commit)
git revert HEAD

# CUIDADO: Resetear al último commit (pierde cambios)
git reset --hard HEAD
```

---

## Troubleshooting Común

### Problema 1: "Permission denied (publickey)"

Este error ocurre cuando Git no puede autenticarse con GitLab usando SSH.

**Solución:**

```bash
# Verificar que la llave SSH está en el agente
ssh-add -l

# Si no está, agregarla
ssh-add ~/.ssh/id_ed25519

# En Windows PowerShell, si ssh-add no funciona:
# Usar Git Bash en su lugar, o configurar el servicio OpenSSH Authentication Agent

# Verificar conexión con GitLab
ssh -T git@gitlab.com
```

**Alternativa: Usar HTTPS en lugar de SSH**

```bash
# Cambiar la URL del remoto a HTTPS
git remote set-url origin https://gitlab.com/tu-usuario/juice-shop-devsecops.git

# Verificar el cambio
git remote -v
```

### Problema 2: "fatal: not a git repository"

Este error aparece cuando intentas usar comandos Git fuera de un repositorio.

**Solución:**

```bash
# Asegúrate de estar en el directorio correcto
cd /ruta/al/repositorio
pwd  # (o 'cd' en Windows para ver la ruta actual)

# Verificar si es un repositorio Git
ls -la .git  # En Windows: dir .git

# Si necesitas inicializar un nuevo repositorio
git init
```

### Problema 3: Conflicto al hacer push

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'gitlab.com:usuario/proyecto.git'
```

**Solución:**

```bash
# Primero bajar los cambios remotos
git pull origin main

# Si hay conflictos, Git te lo indicará
# Edita los archivos con conflictos
# Busca las marcas: <<<<<<<, =======, >>>>>>>

# Después de resolver conflictos:
git add .
git commit -m "Resolver conflictos de merge"
git push origin main
```

### Problema 4: "The file will have its original line endings"

Este es un warning sobre finales de línea (Windows usa CRLF, Linux/Mac usa LF).

**Solución:**

```bash
# Configurar Git para manejar finales de línea automáticamente
# En Windows:
git config --global core.autocrlf true

# En macOS/Linux:
git config --global core.autocrlf input
```

### Problema 5: Olvidaste configurar user.name y user.email

```
*** Please tell me who you are.
```

**Solución:**

```bash
# Configuración global (para todos los repositorios)
git config --global user.name "Tu Nombre"
git config --global user.email "tu-email@ejemplo.com"

# Configuración local (solo para el repositorio actual)
git config user.name "Tu Nombre"
git config user.email "tu-email@ejemplo.com"

# Verificar la configuración
git config --list
```

### Problema 6: No puedes extraer el archivo ZIP

**En Windows:**

```powershell
# Si Expand-Archive falla, intenta:
Add-Type -AssemblyName System.IO.Compression.FileSystem
[System.IO.Compression.ZipFile]::ExtractToDirectory(
    "C:\DevSecOps-UTEC\labs-devsecops-utec-2025\Code\juice-shop-devsecops.zip",
    "C:\DevSecOps-UTEC\temp-juice-shop"
)
```

**En macOS/Linux:**

```bash
# Si unzip no está instalado:
# Ubuntu/Debian:
sudo apt install unzip

# macOS (con Homebrew):
brew install unzip

# Fedora/RHEL:
sudo dnf install unzip
```

### Problema 7: "fatal: refusing to merge unrelated histories"

Este error puede ocurrir si el repositorio remoto tiene contenido que no está relacionado con tu repositorio local.

**Solución:**

```bash
# Permitir merge de historiales no relacionados
git pull origin main --allow-unrelated-histories

# Resolver cualquier conflicto si existe
# Luego hacer commit y push
git push origin main
```

---

## Mejores Prácticas para el Curso

### 1. Commits Frecuentes y Descriptivos

✅ **Bueno:**
```bash
git commit -m "Agregar endpoint de autenticación con JWT"
git commit -m "Corregir validación de entrada en formulario de login"
git commit -m "Actualizar dependencias de seguridad en package.json"
```

❌ **Malo:**
```bash
git commit -m "cambios"
git commit -m "fix"
git commit -m "wip"
```

### 2. Estructura de Mensajes de Commit

Sigue la convención de commits convencionales:

```bash
# Formato: <tipo>: <descripción>

# Tipos comunes:
feat:     # Nueva característica
fix:      # Corrección de bug
docs:     # Cambios en documentación
style:    # Cambios de formato (no afectan el código)
refactor: # Refactorización de código
test:     # Agregar o modificar tests
chore:    # Tareas de mantenimiento
security: # Cambios relacionados con seguridad

# Ejemplos:
git commit -m "feat: Agregar autenticación de dos factores"
git commit -m "fix: Corregir vulnerabilidad SQL injection"
git commit -m "docs: Actualizar README con instrucciones de deployment"
git commit -m "security: Actualizar dependencias con vulnerabilidades conocidas"
```

### 3. Usar .gitignore Apropiadamente

Asegúrate de que tu `.gitignore` incluya:

```gitignore
# Dependencias
node_modules/
package-lock.json

# Archivos de ambiente
.env
.env.local
.env.*.local

# Logs
logs/
*.log
npm-debug.log*

# Archivos del sistema operativo
.DS_Store
Thumbs.db
desktop.ini

# IDEs
.vscode/
.idea/
*.swp
*.swo

# Build outputs
dist/
build/
```

### 4. Sincronización Regular

```bash
# Al inicio de cada sesión de trabajo:
git pull origin main

# Durante el trabajo:
git add .
git commit -m "Descripción de cambios"

# Al final de cada sesión:
git push origin main
```

### 5. Revisar Antes de Commitear

```bash
# Ver qué archivos han cambiado
git status

# Ver los cambios específicos
git diff

# Ver cambios en staging
git diff --staged

# Luego commitear
git add .
git commit -m "Mensaje descriptivo"
```

---

## Extensiones Recomendadas para VSCode

### Esenciales para el Curso

1. **GitLens** - Git supercharged
   - Visualiza blame, navegación de código, comparaciones
   - `Ctrl+Shift+X` → Busca "GitLens" → Install

2. **GitLab Workflow** - Integración con GitLab
   - Ve merge requests, pipelines, issues desde VSCode
   - Busca "GitLab Workflow" → Install

3. **Docker** - Gestión de contenedores
   - Construye, ejecuta y gestiona contenedores
   - Busca "Docker" (de Microsoft) → Install

4. **ESLint** - Linting de JavaScript/Node.js
   - Identifica problemas en el código
   - Busca "ESLint" → Install

5. **Prettier** - Formateador de código
   - Mantén un estilo consistente
   - Busca "Prettier - Code formatter" → Install

### Útiles para DevSecOps

6. **YAML** - Soporte para archivos YAML
   - Para archivos de CI/CD `.gitlab-ci.yml`
   - Busca "YAML" (de Red Hat) → Install

7. **SonarLint** - Análisis estático de código
   - Detecta bugs y vulnerabilidades
   - Busca "SonarLint" → Install

8. **Thunder Client** - Cliente REST API
   - Para probar APIs
   - Busca "Thunder Client" → Install

### Para Mejorar Productividad

9. **Path Intellisense** - Autocompletado de rutas
   - Busca "Path Intellisense" → Install

10. **Auto Rename Tag** - Renombrar tags HTML/XML
    - Busca "Auto Rename Tag" → Install

---

## Recursos Adicionales

### Documentación Oficial

- **GitLab Docs**: [https://docs.gitlab.com](https://docs.gitlab.com)
- **Git Docs**: [https://git-scm.com/doc](https://git-scm.com/doc)
- **VSCode Docs**: [https://code.visualstudio.com/docs](https://code.visualstudio.com/docs)
- **GitLab SSH Keys**: [https://docs.gitlab.com/ee/user/ssh.html](https://docs.gitlab.com/ee/user/ssh.html)

### Tutoriales y Guías

- **Git Tutorial Interactivo**: [https://learngitbranching.js.org/](https://learngitbranching.js.org/)
- **Pro Git Book**: [https://git-scm.com/book/es/v2](https://git-scm.com/book/es/v2)
- **GitLab CI/CD**: [https://docs.gitlab.com/ee/ci/](https://docs.gitlab.com/ee/ci/)

### Comandos Git - Cheat Sheet

- **GitHub Git Cheat Sheet**: [https://education.github.com/git-cheat-sheet-education.pdf](https://education.github.com/git-cheat-sheet-education.pdf)
- **GitLab Git Cheat Sheet**: [https://about.gitlab.com/images/press/git-cheat-sheet.pdf](https://about.gitlab.com/images/press/git-cheat-sheet.pdf)

---

## Conclusión

¡Felicidades! Has completado exitosamente la configuración de tu entorno de laboratorio para el curso DevSecOps:

### ✅ Checklist de Completitud

- ✅ Cuenta en GitLab creada y verificada
- ✅ Visual Studio Code instalado y configurado
- ✅ Llaves SSH generadas y agregadas a GitLab
- ✅ Repositorio del curso clonado (`labs-devsecops-utec-2025`)
- ✅ Proyecto personal privado creado (`juice-shop-devsecops`)
- ✅ Código de Juice Shop extraído y copiado
- ✅ Primer commit realizado y subido a GitLab
- ✅ Proyecto abierto en Visual Studio Code

### 🎯 Próximos Pasos

1. **Familiarízate con el proyecto**
   - Explora la estructura del código
   - Lee el README.md del proyecto
   - Revisa las dependencias en package.json

2. **Mantén tu repositorio actualizado**
   - Haz commits frecuentes de tu trabajo
   - Sincroniza regularmente con GitLab
   - Usa mensajes de commit descriptivos

3. **Participa en el curso**
   - Sigue las instrucciones del instructor
   - Revisa regularmente el repositorio del curso para actualizaciones
   - Practica los conceptos de DevSecOps en tu proyecto

### 📝 Notas Importantes

- **No modifiques** el repositorio `labs-devsecops-utec-2025` (solo lectura)
- **Trabaja siempre** en tu repositorio personal `juice-shop-devsecops`
- **Haz backup** regular haciendo push a GitLab
- **Consulta esta guía** cuando necesites recordar comandos

### 🆘 ¿Necesitas Ayuda?

Si encuentras problemas:
1. Revisa la sección de [Troubleshooting](#troubleshooting-común)
2. Consulta la [documentación oficial](#recursos-adicionales)
3. Pregunta a tus compañeros o al instructor
4. Revisa los foros de GitLab y Stack Overflow

---

**¡Éxito en tu curso de DevSecOps!** 🚀🔒

---

*Última actualización: Noviembre 2025*  
*Versión: 2.0 - Laboratorio UTEC DevSecOps*  
*Autor: Alejandro Fuentes*