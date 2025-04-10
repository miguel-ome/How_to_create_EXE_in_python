PASSO A PASSO – Criar EXE com INSTALADOR para sistema Tkinter + Excel

🌐 REQUISITOS:
- Python 3.11 instalado
- Pip atualizado
- PyInstaller instalado: pip install pyinstaller
- Inno Setup instalado: https://jrsoftware.org/isdl.php

📁 ESTRUTURA DE ARQUIVOS:
Seu projeto deve conter:
- main.py           ← Código principal do sistema
- clientes.xlsx     ← Arquivo do "banco de dados"
- icon.ico (opcional) ← Ícone do app
- install.iss       ← Script do instalador (criado no Inno Setup)

🚀 ETAPA 1 – CRIAR O EXECUTÁVEL
Abra o terminal na pasta do projeto e execute:

pyinstaller --noconfirm --onefile --windowed   --add-data "clientes.xlsx;."   --icon=acordo.ico   main.py

📝 Detalhes:
- `--noconfirm`: sobrescreve sem perguntar
- `--onefile`: gera um único arquivo .exe
- `--windowed`: oculta console (ideal para GUI)
- `--add-data`: inclui arquivos extras no executável (Excel, imagens)
  OBS: no Windows use `;` para separar caminho origem/destino

📂 Após a execução:
- O .exe final estará em: `dist/main.exe`

🔧 ETAPA 2 – CRIAR O INSTALADOR (Opcional)
Abra o Inno Setup > Criar novo script > Use Assistente:

1. Nome da app: Sistema de Clientes
2. Versão: 1.0 | Empresa: Seu Nome
3. Executável: selecione `main.exe` dentro da pasta `dist`
4. Pasta de instalação: {pf}\SistemaClientes
5. Adicione o arquivo `clientes.xlsx` (deve estar na mesma pasta do .exe)
6. Ícone: selecione o .ico, se tiver
7. Marque “Criar atalho na Área de Trabalho”
8. Finalize e compile

Script para o empacotador:

; Script do Inno Setup para empacotar o sistema de clientes

[Setup]
AppName=Sistema de Clientes
AppVersion=1.0
DefaultDirName={pf}\SistemaClientes
DefaultGroupName=Sistema de Clientes
OutputDir=Output
OutputBaseFilename=instalador_sistema_clientes
Compression=lzma
SolidCompression=yes
ArchitecturesInstallIn64BitMode=x64
DisableWelcomePage=no

[Files]
Source: "dist\main.exe"; DestDir: "{app}"; Flags: ignoreversion
Source: "clientes.xlsx"; DestDir: "{app}"; Flags: ignoreversion

[Icons]
Name: "{commondesktop}\Sistema de Clientes"; Filename: "{app}\main.exe"

[Run]
Filename: "{app}\main.exe"; Description: "Executar Sistema de Clientes"; Flags: nowait postinstall skipifsilent


✅ Saída final:
- Um instalador `.exe` que instalará tudo no PC do usuário

📦 DICA EXTRA:
Se quiser empacotar tudo (inclusive `clientes.xlsx`) para ser acessado no mesmo local do `.exe`, no `main.py` use este caminho:

```python
import os, sys

def recurso_path(rel_path):
    if hasattr(sys, '_MEIPASS'):
        return os.path.join(sys._MEIPASS, rel_path)
    return os.path.join(os.path.abspath("."), rel_path)

excel_path = recurso_path("clientes.xlsx")
