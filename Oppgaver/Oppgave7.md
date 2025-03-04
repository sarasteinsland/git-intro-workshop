# Oppgave 7

I denne oppgaven skal vi se litt på forskjellige måter å rette opp feil. 
Både feil som har oppstått før en har lagt endringene til i `main` branch-en, 
men også hvordan en kan rette på feil etter at koden er delt og kanskje i produksjon.

## Git Reset
Vi har Så vidt vært innom `git reset` tidligere. Da med å bruke SHA til en spesifikk commit for å gå tilbake til denne. 

En ting vi ikke har snakket om så langt er begrepet HEAD. HEAD indikerer enden på den committede historikken i den aktive branch-en. 
Så om en ikke har noen endringer, stage-et eller ikke, så er HEAD der en står akkurat nå.
<br>En kan referere tidligere commits ved å legge til *tilde* og et tall. 
`HEAD~1` er nest siste commit, `HEAD~2` er tredje siste commit osv. 
`HEAD~0` er det samme som `HEAD`.
Dette skal vi bruke litt fremover når vi ser på `git reset`

`git reset` vil fjerne commits. Hva en setter til slutt i kommandoen sier noe om hva en ender opp med som ny `HEAD`.
Tidligere har vi brukt en git SHA. Dette er en absolutt referanse.
`HEAD~n` vil være relativt til hva som er enden av gjeldende branch.

`git reset` kommer med tre modus: *hard*, *soft* og *mixed*.
1) **git reset --soft [commit]**
 - Flytter `HEAD` til [commit].
 - Lar stage-ede filer være igjen.
 - Lar filer på disk, som ikke inngår i ny HEAD, være igjen.

2) **git reset --mixed [commit]**
 - Dette er standard opsjon.
 - Flytter `HEAD` til [commit].
 - Alle stage-ede filer vil bli un-staged.
 - Lar filer på disk, som ikke inngår i ny HEAD, være igjen.

3) **git reset --hard [commit]**
 - Dette er standard opsjon.
 - Flytter `HEAD` til [commit].
 - Alle stage-ede filer vil bli un-staged.
 - Alle filer på disk, som ikke inngår i ny HEAD, vil bli slettet.


Sjekk ut branch-en `oppgave7-1`
```shell
git checkout oppgave7-1
```

### Reset soft
Om du kjører `git --no-pager log --oneline --graph -2` vil du se at siste commit ser ut omtrent som:
```text
* c0961cd (HEAD -> oppgave7-1) lagt til kafka.txt
```
Denne commit-en legger til fila `Oppgave7/kafka.txt
`
Om du kjører
```shell
git reset --soft HEAD~1
git status
```
så vil du få noe som likner
```text
On branch oppgave7-1
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   Oppgave7/kafka.txt
```
Om du kjører `git --no-pager log --oneline --graph -2` vil du se at siste commit du hadde tidligere er borte. 
Så, committ-en er borte, men innholdet i commit-en er der fortsatt og er staget.

For å komme tilbake til start, kjør
```shell
git reset --hard c0961cd
```

###  Reset mixed

Kjør
```shell
git reset --mixed HEAD~1
git status
```
Du vil nå få
```text
On branch oppgave7-1
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Oppgave7/

nothing added to commit but untracked files present (use "git add" to track)
```
Om du nå kjører `git --no-pager log --oneline --graph -2` vil du se at siste commit du hadde tidligere nok en gang er borte.
Innholdet i commit-en er der fortsatt, men denne gangener det ikke staget.

Vi flytter nok en gang tilbake til start:
```shell
git reset --hard c0961cd
```

### Reset hard
Kjør
```shell
git reset --hard HEAD~1
```
og du får noe som likner
```text
HEAD is now at 462c922 Litt rettelser + start oppgave 7
```
```shell
git --no-pager log --oneline --graph -2
```
gir
```text
* 462c922 (HEAD -> oppgave7-1, origin/main, origin/HEAD) Litt rettelser + start oppgave 7
* b2a1b4d Ferdig Oppgave 6
```
Om du kjører
```shell
git status
```
får du nå
```text
On branch oppgave7-1
nothing to commit, working tree clean
```
Som indikerer at alt er borte. Ingen ting fra commit-en vi har fjernet er igjen, staget eller på disk.
> [!CAUTION]
> Når vi kjører `git reset --hard` vil det ikke være noen direkte vei tilbake med mindre en kjenner SHA-en til committ-en som vi slettet.

> [!TIP]
> `git reflog` er nødløsningen for å finne igjen commits du har "mistet"

Kjør
```shell
git reflog
```
og se om du kan finne igjen den slettede commit-en.

## Git Revert
Git revert er en måte å reversere en eller flere commits. 
Om en har en commit, som er pushet til main og kanskje ligger i produksjon, kan en ikke bare fjerne denne commit-en med en rebase eller liknende.

En kan selvfølgelig manuelt lage en ny commit der endringene er fjernet. 
Dette vil være enkelt om det bare er en fil som er lagt til eller en enkelt del av en fil som er endret, men ofte er det vesentlig mer komplekst.

Git revert vil reversere en (eller flere) commits og fjerne de endringen den la til.

Sjekk ut branch-en `oppgave7-2`. Denne inneholder den samme fila som i forrige oppgave. 
Om du åpner fila `Oppgave7/kafka.txt`, så vil du se at noen i en tekst av Kafka har lagt til et avsnitt med `lorem ipsum..`
<br> Etter dette er det en ny commit der et korrekt avsnitt med mere Kafka lagt til. 

Vi kunne selvfølgelig lett bare ha fjernet dette, men la oss heller la Git gjøre jobben.

Om du kjører 
```shell
git revert HEAD~1
```
vil du få opp forslag til Commit-melding
```text
 Revert "Lagt til avsnitt"

 This reverts commit 870ac78852fc81ce8e7f30584d62f1b9e2b9a19a.

 # Please enter the commit message for your changes. Lines starting
 # with '#' will be ignored, and an empty message aborts the commit.
 #
 # On branch oppgave7-2
 # Changes to be committed:
 #	modified:   Oppgaver/Oppgave7/kafka.txt
 #
```

Om du nå ser på fila `Oppgave7/kafka.txt` så vil du se at `Lorem ipsum...` avsnittet er borte.

## Git Cherry-Pick

`git cherry-pick` git deg mulighet til å "importere" en enkelt commit fra en branch inn i en annen.

I branch-en `oppgave7-3` har vi `kafka.txt` slik den blir etter forrige oppgaves `revert`.
Eneste problemet er at det er en lite "typo" i begynnelsen av siste avsnitt: En `G` er borte i starten av linja.

La oss tenke oss at `oppgave7-3` ligger i produksjon med denne feilen. 

Noen har oppdaget dette mens de jobbet med `oppgave7-4`. 
Om du sjekker ut denne branch-en og ser på historikken, vil du se at følgende commit ligger der:
```text
* 3e345b2 Fikset typo i kafka.txt
```

`oppgave7-4` er ikke klar for produksjon ennå, men vi trenger denne fiksen med en gang. 
Vi har heller ikke lyst til å gjøre denne endringen en gang til.

Vi kan gjøre følgende:
```shell
git checkout oppgave7-3
git cherry-pick 3e345b2
```
Om du nå sjekker `kafka.txt` vil du se at `G`-en er på plass.

Om du sjekker historikken, vil du nå få liknende
```text
* c1431c0 (HEAD -> oppgave7-3) Fikset typo i kafka.txt
* 4aad527 (origin/oppgave7-3, oppgave7-2) Revert "Lagt til avsnitt"
* f7c4f9a (origin/oppgave7-2) Lagt til ny slutt
* b923251 Lagt til avsnitt
```

[Oppgave 8](./Oppgave8.md)