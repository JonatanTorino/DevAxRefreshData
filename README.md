# DevAxRefreshData
Modelo de D365FO para actualizar configuraciones después de un RefreshData desde PROD

## Installation

### Using Power Shell
Copy and past
```powershell
# Task 1: Clone the repository
$repositoryUrl = "https://github.com/JonatanTorino/DevAxRefreshData"
$localPath = "K:\Axxon\GitHub.JonatanTorino\DevAxRefreshData"

# Clone the repository
git clone $repositoryUrl $localPath | Wait-Process

# Task 2: Create a symbolic link
$targetPath = "K:\Axxon\GitHub.JonatanTorino\DevAxRefreshData\DevAxRefreshData"
$modelName = "DevAxRefreshData"
$linkPath = "K:\AosService\PackagesLocalDirectory\"+$modelName

Write-Host 'Remove existing directory if it exists'
Remove-Item -Path $linkPath -Recurse -Force -ErrorAction SilentlyContinue

Write-Host 'Create a symbolic link'
New-Item -ItemType SymbolicLink -Path $linkPath -Target $targetPath

# Task 3: Compile the model
Write-Host 'Executing the D365 module compile command'
Invoke-D365ModuleFullCompile -Module $modelName

```
