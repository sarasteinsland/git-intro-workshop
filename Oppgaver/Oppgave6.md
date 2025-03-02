# Oppgave 6

I denne oppgaven skal vi se litt på konfliktløsing. 
Om vi gjør en operasjon så som `merge`, `pull`, `rebase`, `stash pop` osv når det har blitt gjort endringer på samme sted i en fil både i deres versjon og vår versjon, så vil vi få en konflikt.
Git kan ingenting om kontekst, så som språk, filtyper osv. og kan derfor ikke gjøre et valg med hensyn på om en eller flere endringer skal være med eller droppes.

Det som derfor skjer i denne type operasjoner ved konflikter, er at operasjonen stoppes og en må ta stilling til hvordan konflikten skal løses. 
Deretter gjenopptar en den stoppede operasjonen. Om det ikke er ytterligere problemer, fullføres denne.

Når Git stopper en slik operasjon, blir det gitt ganske gode indtruksjoner om hvordan en kommer seg videre eller avbryter.

## Manuell Konfliktløsning
I branch--en `oppgave6-main` `commit 1` er det lagt til en ny fil: `/Oppgaver/Oppgave 6/lorem.txt` . 
Deretter er det opprettet en ny branch `oppgave6-fix` der det er lagt til ett nytt avsnitt i teksten i commit-en `commit 2`.
I `oppgave6-main` er det så lagt til et annet avsnitt på samme sted i en commit med melding `commit 3`.

Om du sjekker ut de to branch-ene en etter en og ser på fila, vil du se at den ene har lagt til
```text
Vivamus elementum semper nisi. Aenean vulputate eleifend tellus. Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim.
```
mens den andre har fått 
```text
Aliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus. Phasellus viverra nulla ut metus varius laoreet. Quisque rutrum. Aenean imperdiet.
```
Sjekk ut `oppgave6-main` og merge inn `oppgave6-fix` med
```shell
git checkout oppgave6-main
git merge oppgave6-fix
```

Siden det nå er en konflikt, vil operasjonen stoppe med følgende melding
```text
Auto-merging Oppgaver/Oppgave 6/lorem.txt
CONFLICT (content): Merge conflict in Oppgaver/Oppgave 6/lorem.txt
Automatic merge failed; fix conflicts and then commit the result.
```
Om vi kjører `git status` vil den vise dette:
```text
On branch oppgave6-main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   Oppgave 6/lorem.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Om vi ser i fila `Oppgaver/Oppgave 6/lorum.txt` så vil vi litt nede på side se
```text
<<<<<<< HEAD
Vivamus elementum semper nisi. Aenean vulputate eleifend tellus. Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim.
=======
Aliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus. Phasellus viverra nulla ut metus varius laoreet. Quisque rutrum. Aenean imperdiet.
>>>>>>> oppgave6-fix
```
Dette er formatet git bruker for å indikere konflikt. 
Om det er flere konflikter i samme file, vil det være gjentakelser i dette mønsteret.
En enkel måte å finne dette er å søke i fila etter `<<<<<<`. Dette skulle være et mønster som ikke alt for ofte ligger i en fil med vilje.
En kan også se hvor de enkelte delene av konflikten har sitt opphav. 
`HEAD` er enden av gjeldende branch, altså `oppgave6-main`. `oppgave6-fix`er åpenbart den andre branch-en.

Det en deretter må gjøre, er å vurdere hva som egentlig skal ligge i fila. 
I dette tilfellet er det enkelt. 
Begge deler skal med, og vi kan bare slette merkingen til git slik at vi sitter igjen med
```text
In enim justo, rhoncus ut, imperdiet a, venenatis vitae, justo. Nullam dictum felis eu pede mollis pretium. Integer tincidunt. Cras dapibus.

Vivamus elementum semper nisi. Aenean vulputate eleifend tellus. Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim.

Aliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus. Phasellus viverra nulla ut metus varius laoreet. Quisque rutrum. Aenean imperdiet.

Etiam ultricies nisi vel augue. Curabitur ullamcorper ultricies nisi. Nam eget dui. Etiam rhoncus.
```
og lagre.

Så, om det er flere konflikter, løser vi disse, lagrer og følger instruksjonene fra git.
Om vi ser på teksten fra `git status` så sier den 
```text
no changes added to commit (use "git add" and/or "git commit -a")
```
Her kan vi da for eksempel kjøre `git commit -a`. 
Da får vi opp en editor med forslag til commit-melding: 
```text
Merge branch 'oppgave6-fix' into oppgave6-main
```
En kan velge å beholde denne eller skrive en litt mer utfyldende tekst med hvilken konflikt som ble løst.
Om du legger til linjeskift i en commit-melding, så vil all tekst komme med, men bare første linje vil vises i `git log`

## Hjelp koden kompilerer ikke
Konflikten vi løste over var temmelig enkel. 
I kode vil typisk liknende konflikter være to funksjoner lagt til i slutten av en fil. 
Veldig ofte er det mer komplisert. 

> [!IMPORTANT]
> Etter å ha løst en konflikt, er det viktig å kompilere og kjøre tester før en push-er koden remote. 
> Det er mye mindre styr å rydde opp i dette lokalt heller enn å fikse det etter at koden er push-et til main.


 