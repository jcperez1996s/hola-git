Guía de Instalación de Oh My Posh en Windows Terminal
Instalar Windows Terminal y Powershell
1. Instalar Oh My Posh (vía Winget)
Ejecuta en PowerShell o Terminal:
winget install JanDeDobbeleer.OhMyPosh -s winget

2. Verificar instalación
Para confirmar que se instaló correctamente:
oh-my-posh
(Deberías ver la versión y comandos disponibles.)

3. (Opcional) Instalar una fuente compatible
Oh My Posh usa iconos y símbolos que requieren fuentes especiales (como Nerd Fonts). Instala una recomendada con:
oh-my-posh font install
(Sigue las instrucciones para configurar la fuente en tu terminal.)

Configuración Automática en PowerShell
4. Generar comando de inicialización
Ejecuta esto para obtener la línea que carga Oh My Posh al iniciar PowerShell:

oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\jandedobbeleer.omp.json" | Invoke-Expression
(Esto mostrará un código que debes guardar en tu perfil de PowerShell.)

5. Configurar el perfil de PowerShell
Crear/editar el perfil:

New-Item -Path $PROFILE -Type File -Force
notepad $PROFILE
Pega el código generado en el paso 4 (debe verse similar a esto):

(@(& 'C:/Users/tucon/AppData/Local/Programs/oh-my-posh/bin/oh-my-posh.exe' init pwsh --config='C:\Users\tucon\AppData\Local\Programs\oh-my-posh\themes\jandedobbeleer.omp.json' --print) -join "`n") | Invoke-Expression



Install-Module -Name Terminal-Icons -Repository PSGallery

## cONTENIDO TXT
(@(& 'C:/Users/tucon/AppData/Local/Programs/oh-my-posh/bin/oh-my-posh.exe' init pwsh --config='C:\Users\tucon\AppData\Local\Programs\oh-my-posh\themes\agnoster.omp.json' --print) -join "`n") | Invoke-Expression
Import-Module Terminal-Icons
SEt-PSReadLineOption -PredictionViewStyle ListView


## THEME
{
            "background": "#282A36",
            "black": "#282A36",
            "blue": "#57C7FF",
            "brightBlack": "#686868",
            "brightBlue": "#57C7FF",
            "brightCyan": "#9AEDFE",
            "brightGreen": "#5AF78E",
            "brightPurple": "#FF6AC1",
            "brightRed": "#FF5C57",
            "brightWhite": "#EFF0EB",
            "brightYellow": "#F3F99D",
            "cursorColor": "#97979B",
            "cyan": "#9AEDFE",
            "foreground": "#EFF0EB",
            "green": "#5AF78E",
            "name": "Snazzy",
            "purple": "#FF6AC1",
            "red": "#FF5C57",
            "selectionBackground": "#3E404A",
            "white": "#F1F1F0",
            "yellow": "#F3F99D"
        }

## Politica de Privacidad
Get-ExecutionPolicy
## Permitir scripts locales
Set-ExecutionPolicy RemoteSigned
