# PowerShell

## Introduction

## Scripts

```PowerShell
###############################################################################
# @file: micheline.ps1                                                        #
# @author: LEBLOND Benjamin, SOUCHET Justin, CHEN Patrick                     #
# @date: 2018-11-21                                                           #
# @description : Liste les fichiers .docx, .html et le fichier secret.txt     #
# 	du dossier TRAVAIL ; ferme Word si il est ouvert ; test la connection a   #
# 	quelques sites internet.                                                  #
###############################################################################

$files = Get-ChildItem .\TRAVAIL -File -Recurse | Where-Object {
	(
		($_.Extension -eq ".docx" -or $_.Extension -eq ".html") -and
		$_.CreationTime -ge "01/01/2018"
	) -or
	$_.Name -eq "secret.txt"
}

# Affichage des fichiers sous le format d'un "ls"
# $files

# Affichage des fichiers avec uniquement les champs choisis
$files | Select-Object Name,CreationTime,DirectoryName,Extension,LastAccessTime,LastWriteTime,Length,FullName

# Envoi des fichier dans une archive
$files | Compress-Archive -CompressionLevel Fastest -DestinationPath files.zip -Force

# Récuperation du processus de Word
$wordProcess = Get-Process | Where-Object {$_.ProcessName -eq "winword"}

# Si le process existe, on le ferme
if ($wordProcess) {
	$tmp = $wordProcess.CloseMainWindow();
	$wordProcess.close();
}

$sites = @("google.com", "portal.office.com", "microsoft.com", "pcastuces.com", "aidermicheline.com")

foreach ($site in $sites) {
	Write-host "Test de connection au site $site : $(if (Test-Connection $site -quiet -BufferSize 1 -Count 1) {'OK'} else {'KO'})"
}

```

