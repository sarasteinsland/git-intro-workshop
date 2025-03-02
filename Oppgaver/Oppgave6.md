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
 