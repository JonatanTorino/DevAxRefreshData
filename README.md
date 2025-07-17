# DevAxRefreshData
Modelo de D365FO para actualizar configuraciones después de un RefreshData desde PROD

## Installation

### Using Power Shell
Copy and past
```powershell
# Task 1: Clone the repository
$repositoryUrl = "https://github.com/JonatanTorino/DevAxRefreshData"
$localRepoPath = "K:\Axxon\GitHub.JonatanTorino\DevAxRefreshData"
$modelName = "DevAxRefreshData"
$packagesLocalDirectory = "K:\AosService\PackagesLocalDirectory"

# Clone or pull the repository
git clone $repositoryUrl $localRepoPath | Wait-Process
# En caso que existiera previamente la carpeta, por las dudas ejecuto un pull
Set-Location $localRepoPath
git pull

Write-Host -ForegroundColor Yellow "Deteniendo todos los servicios de D365FO"
Stop-D365Environment

# Task 2: Create a symbolic link
$targetPath = Join-Path $localRepoPath -ChildPath $modelName
$linkPath = Join-Path $packagesLocalDirectory -ChildPath $modelName

Write-Host -ForegroundColor Cyan "Create a symbolic link to $targetPath"
New-Item -ItemType SymbolicLink -Path $linkPath -Target $targetPath

# Task 3: Compile the model
Write-Host -ForegroundColor Green "Executing the D365 module compile command: $modelName"
Invoke-D365ProcessModule -Module $modelName -ExecuteCompile -ExecuteSync

Write-Host -ForegroundColor Yellow "Iniciando el servicio del AOS de D365FO"
Start-D365Environment -Aos
Write-Host -ForegroundColor Yellow "Iniciando el servicio del BATCH de D365FO"
Start-D365Environment -Batch

```
