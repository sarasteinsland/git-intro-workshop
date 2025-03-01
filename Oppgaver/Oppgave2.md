# Oppgave 2
I denne oppgaven skal vi se på `.gitignore` og  `.gitconfig`
## .gitignore
`.gitignore` er en fil som beskriver filer og foldere som ikke skal eller ikke skal regnes med inn i commits. 
Typisk legger en denne fila i rot-folderen til prosjektet. Reglene en legger inn i denne fila vil lgjelde for hele prosjektet.

Om en ønsker å overstyre de globale reglene, kan en ny `.gitignore`-fil legges i en underkatalog.
Her kan en opprette nye regler for filer som skal ignoreres, eller oppheve regler som er laget på et mer generelt nivå.

Det en typisk legger i `gitignore` er 
 - Filer og foldere som ikke er kode, men et resultat av en byggeprosess i det enkelte språk.
 - Filer som blir generert av utviklingsverktøy (IDE), men som ikke er nødvendig for å åpne prosjektet i utviklingsverktøyet.

 For IntelliJ IDEA vil dette for eksempel være
```text
.idea/
*.iml
```
`.idea/` er folderen med dette navnet, og `*.iml` er alle filer som har navn som ender på `.iml`

En god ressurs for `.gitignore` er [gitignore.io](https://www.toptal.com/developers/gitignore/) 
Her kan en velge en eller flere ressurser og generere `.gitignore` basert på disse. 
Den genererer en ganske stor fil, men en vil være ganske sikker på at det meste er dekket.

Følg lenken over, generer en fil og sjekk ut innholdet i fila som er generert. 
En vanlig mye brukt og vanlig kombinasjon kan være `java`, `Maven`, `IntelliJ` og `Visual Studio Code`. 
Søk opp disse en og en, og generer en fil. Kikk gjennom file og se på de forskjellige reglene. 
Når du finner regler du lurer på, kan du sjekke [dokumentasjonen her](https://git-scm.com/docs/gitignore).

## Filtrere filer med .gitignore
Generer en fil fra kommandolinja for eksempel med følgende kommando
```shell
echo lorem ipsum... > lorem.txt
```
Om du nå kjører `git status` vil du få noe slikt som:
```text
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        lorem.txt

nothing added to commit but untracked files present (use "git add" to track)
```
Om du åpner `.gitignore` og legger til enten `lorem.txt` eller `*.txt` og kjører `git status` igjen, vil du få følgende:
```text
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .gitignore

no changes added to commit (use "git add" and/or "git commit -a")
```

Om du genererer en ny fil i en folder:
```shell
mkdir killroy
echo Killroy was here>killroy/killroy.txt
```

Om du nå kjører `git st` vil du få:
```text
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .gitignore

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        killroy/

no changes added to commit (use "git add" and/or "git commit -a")
```

Om du nå legger folderen til i `.gitignore` med en linje med `killroy/` og deretter kjører `git status` igjen, vil du se at vi er tilbake til å bare ha `.gitignore`som endret fil.

Endringen i `.gitignore` kan du 'committe' med 
``` shell
git add .gitignore
git commit -m "oppdatert .gitignore"
```
For å  legge endringene opp på remote repo, kan du nå kjøre
```shell
git push origin
```

## gitconfig
Git lagrer konfigurasjon du har satt i konfig-filer. 
 - Din globale konfigurasjon, som gjelder for alle repoene dine, ligger i din hjemmekatalog i fila`.gitconfig`. 
   For å endre denne må kommandoer ha `--global` 
 - Konfigurasjon for et enkelt repository ligger i fila `.git/gitconfig`. Denne endres med `--local` som også er standard.
   Så om du gjør en `git set ...` uten noen 'switch', så er det den lokale som endres.

Den globale konfigurasjonen din kan listes ut med `git config list --global`.
En enkelt i konfigurasjonen, for eksempel brukernavn, kan listes ut med `git config --get user.name`.
Om du sammenlikner dette med hva om ligger i `.gitconfig`, så ser du at fra kommandolinja så bruker vi ´[section].key` for å navngi konfigurasjon.

Du kan også editere hele konfigurasjonen i standard teksteditor ved å kjøre `git config edit --global`

### Alias
En praktisk 'feature' i konfigurasjonen er `alias`. 
Et alias er en ny, som regel veldig kort, git kommando som du kan definere selv i stedet for mye brukte, lengre kommandoer.

Om du kjører `git config set --global alias.st "status"` vil du få en ny git kommando som du kan kjøre som `git st` som har samme effekt som `git status`.

Du kan også endre denne ved å kjøre `git config edit --global` og legge til 
```text
[alias]
    st = status
```
