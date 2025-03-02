# Oppgave 3
I denne oppgaven skal vi se på `git log`. 
Git log er kommandoen for å se på historikken i et git repo. 
Det er en mengde forskjellige måter å gjøre dette på som gir forskjellige resultater.
Om en bare kjører `git log`, får en en pager med hver commit listet opp med SHA, forfatter, dato og commit-melding:
```text
commit 7c0bdb5ffc6caeca5e6fb02d9a5b7af6737ffbd4
Author: Kristian Berg <rasmantutagmail.com>
Date:   Sat Mar 1 11:20:39 2025 +0100

    fikset repo.git, i hope
```
Alle log-kommandoene kjører i en pager. Det betyr at det som skrives fra en kommando kommer i et nytt "vindu" der en og en side vises ved å trykke mellomrom. 
For å gå tilbake til kommandolinjen må en trykke 'Q'. 

Git har en  `--no-pager` switch, som må komme mellom git og kommando, for å unngå denne. 
For eksempel `git --no-pager log --oneline`. Denne kommandoen gir en linje med info per commit.
I et repo som er av en viss alder, kan det fort være flere hundre commits. 
Disse vil uten pager fly forbi og forsvinne opp i terminalvinduet. 
Om en vet hvor mange commits man masimalt er ute etter, kan man legge på f.eks `-n 7` eller bare `-7`
Dette begrenser utskriften til de siste syv commits.

Kjør `git --no-pager log --oneline -7` og se på utskriften. Den skal se ut som noe i retning av
```text
4e387c0 (HEAD -> main, origin/main, origin/HEAD) Lagt til gitconfig
96f731a (origin/Oppgave1, Oppgave1) fikset Oppgave 1 fremgangsmåte
9c8031c Fikset fremgangsmåter og lagt til fork-opsjon
392eaef Lagt til lokalt repo i ignore
1e725bf Ny strategi lokalt repo
7c0bdb5 fikset repo.git, i hope
f833580 litt endringer i beskrivelse oppgave 1
```
En kan se her at formatet er `SHA(branch)Commit-melding`.

Om en putter på `--graph` på en log-kommando, så vil utskriften få en simple graf som bruker streker å stjerner for å indikere grener i historikken.
Om historikken er lineær, vil ikke dette bli spesielt spennende, men om en har merge-et inn grener vil det kanskje ha mer nytte.
Om du sjekker ut `Oppgave3` med `git checkout Oppgave3` og kjører `git --no-pager log --graph --oneline -6`, så vil du få noe som ser ut ca. slik:
```text
* 44abeff (HEAD -> Oppgave3, origin/Oppgave3) Og enda en
* 64c1e36 (tag: 3.1) En commit til
*   7952497 Merge branch 'Oppgave3-2' into Oppgave3
|\  
| * fe5022a (origin/Oppgave3-2, Oppgave3-2) kommit gjort i annen branch
* | a3dbf59 (tag: 3.0) Første commit i Oppgave 3 branch
|/  
* a1ceba7 Start på Oppgave 3
```
Her kan en se at en commit er gjort i en annen branch for så å ha blitt merge-et inn med en merge commit.
Du kommer tilbake til hoved-branch-en med `git checkout main`

Om du vil se historikken mellom to punkter i historien, kan dette gjøres ved å `<commit 1>...<commit 2>` som argument til log. 
Dette vil liste opp commits fra `<commit 1>` eksklusiv og til `<commit 2>`inklusiv. 
En kan her bruke noe som entydig identifiserer en commit. 
Dette kan være en commit SHA eller et tag-navn. For eksempel, om en bruker `tags` for å markere releaser, vil `git --no-pager log --oneline 3.0...3.1` gi historikken mellom tag for release`3.0` og tag for release `3.1`.

Om du kjører dette, skal du få noe som likner:
```text
64c1e36 (tag: 3.1) En commit til
7952497 Merge branch 'Oppgave3-2' into Oppgave3
fe5022a (Oppgave3-2) kommit gjort i annen branch
```

Mer informasjon om git log finner du på [https://git-scm.com/docs/git-log](https://git-scm.com/docs/git-log)

[Oppgave 4](./Oppgave4.md)