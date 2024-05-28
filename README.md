# Traductions de mods de Fallout 2 - RPU.

## Présentation

Utilisation de Chat GPT avec bonne relecture et reformulation.

## Liens vers les mods

RPU : https://github.com/BGforgeNet/Fallout2_Restoration_Project

EcCo Gameplay Overhaul : https://github.com/phobos2077/fo2_ecco/tree/master

Talking Heads Addon : https://www.nexusmods.com/fallout2/mods/45

Talking Heads Actually Talk (THAT) : https://www.nexusmods.com/fallout2/mods/67

Companion Expansion : https://www.nexusmods.com/fallout2/mods/70

Inventory Filter : https://github.com/rotators/InventoryFilter

fo2tweaks : https://github.com/BGforgeNet/FO2tweaks 


## Codes utiles :
### Recherche des lignes commencants par { mais ne finissant pas par }
```
for File in *.msg
do 
    [[ $(sed -rn "/^\{[0-9]+}\{[^}]*}\{[^}]+$/p" "$File") ]] && echo "Fichier : $File - $(sed -rn "/^\{[0-9]+}\{[^}]*}\{[^}]+$/p" $File)"
done
```

### Recherche des lignes ne commencants pas par {
```
for File in *.msg
do 
    while read Line
    do 
        [[ ${#Line} > 2 && ${Line} != *#* && ${Line:0:1} != "{" ]] && echo "$File : $Line"
    done < "$File"
done
```

### Suppression des majuscules dans les noms (nécéssaire à RPU sous Linux)
```
while read Fichier
do 
    mv "$Fichier" "${Fichier,,}"
done < <(find . -name "*[[:upper:]]*")
```

### Reprise des codes THAT depuis les fichiers US dans les fichiers FR
Si les fichiers US sont dans le dossier courant et les FR dans le dossier "a"
```
for file in *.msg
do
    while read US
    do
        Code="${US/#{}"
        Code="${Code%%\}*}"
        sed -i "/^{${Code}}{}/ s/{${Code}}{}/${US}/" "a/$file"
    done < <(sed -n "/^{[0-9]\+}{.\+}{.*/ s/\({[0-9]\+}{.\+}\){.*/\1/p" "$file")
done
```

