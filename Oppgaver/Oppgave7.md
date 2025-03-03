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
 - Flytter `HEAD` til _[commit].
 - Alle stage-ede filer vil bli un-staged.
 - Lar filer på disk, som ikke inngår i ny HEAD, være igjen.

3) **git reset --hard [commit]**
 - Dette er standard opsjon.
 - Flytter `HEAD` til [commit].
 - Alle stage-ede filer vil bli un-staged.
 - Alle filer på disk, som ikke inngår i ny HEAD, vil bli slettet.
> [!TIP]
> En rask måte å rydde vekk uønskede, ikke comittede filer i prosjektet, er `git reset --hard HEAD`

> [!CAUTION]
> Dette fjerner alle filer som ikke er med i HEAD
