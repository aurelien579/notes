# Executable file formats

## Portable Executable (PE)

- Format utilisé pour les executables, DLLs, object code sur Windows depuis
NT3.1 (1993)

- Contient des headers et des sections permettant au dynamic linker de mapper
le fichier dans la mémoire

### Import Address Table (IAT)

- Permet à l'application d'appeler des fonctions externes. Le dynamic linker
remplis les slots de l'IAT avec les adresses des fonctions externes. En effet,
l'emplacement de ces fonctions n'est pas connue par l'application initialement.