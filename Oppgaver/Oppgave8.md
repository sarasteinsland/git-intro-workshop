# Oppgave 8

I denne oppgaven skal vi se på ett av de kraftigste verktøyene git har å komme med: **Interactive Rebase**

Interactive rebase lar en gjøre en rekke endringer på commits i historikken.
 - Endre rekkefølge på commits
 - Fjerne commits
 - Slå sammen commits (squash)
 - Endre commit-melding (reword)
 - Stoppe for å gjøre endringer (edit)
 - +++

Om en kjører for eksempel `git rebase -i HEAD~5`, vil en få åpnet en editor med følgende tekst
```text
pick <SHA commit 1> commit melding 1
pick <SHA commit 2> commit melding 2
pick <SHA commit 3> commit melding 3
pick <SHA commit 4> commit melding 4
pick <SHA commit 5> commit melding 5

# Rebase 46ccbe2..12e6b78 onto 46ccbe2 (5 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
#         create a merge commit using the original merge commit's
#         message (or the oneline, if no original merge commit was
#         specified); use -c <commit> to reword the commit message
# u, update-ref <ref> = track a placeholder for the <ref> to be updated
#                       to this position in the new commits. The <ref> is
#                       updated at the end of the rebase
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#

```

De fem øverste linjene er kommandoene som en gir for hvordan rebase skal gjennomføres.
Om en bare lukker editoren, vil det fortsette som om dette var en vanlig rebase.

Nedenfor er det forklaring på hvilke `rebase`-kommandoer en kan bruke. 
For å bruke disse, bytter en ut `pick` i begynnelsen av en linje med ønsket kommando. 
For eksempel `s` eller `squash` for å slå sammen to commits.

Som med en vanlig rebase så vil operasjonen stoppe hver gang det oppdages en konflikt, for at denne kan løses før en fortsetter.

Vi skal nå teste ut noen av konseptene her. Sjekk ut `oppgave8` og list ut historikken
```shell
git checkout oppgave8
❯ git --no-pager log --oneline --graph -7
```
Du skal nå få
```text
* a9cfd03 (HEAD -> oppgave8, origin/oppgave8) Slettet avsnitt i lingues.txt
* 90560f1 Lagt til kafka.txt
* 9a7f2ed poc - skal ikke push-es
* 7409bff Lagt til lingues.txt
* 2023f91 Lagt til cicero.txt
* 29c07c4 Lagt til lorem.txt
* 16e1169 (origin/main, origin/HEAD, main) Ferdig oppgave 7
```
Planen er som følger:
1) Vi skal bytte rekkefølge på commits som legger til `lorem.txt` og `cicero.txt`.
2) I `kafka.txt` har noen snublet i tastaturet og lagt til en rekke `@@@@@`. Vi stopper underveis og fikser dette.
3) Siste commit er bare en endring i `lingues.txt`. Denne kan vi flytte opp og `squash-e` med commit der denne fila ble lagt til.
4) Commit med melding `poc - skal ikke push-es` har sneket seg inn, og skal slettes. Dette kan gjøres ved  å fjerne linja til denne, eller merke den med `drop`.

Kjør
```shell
git rebase -i HEAD~6
```

Du får da opp fila:
```text
pick 29c07c4 Lagt til lorem.txt
pick 2023f91 Lagt til cicero.txt
pick 7409bff Lagt til lingues.txt
pick 9a7f2ed poc - skal ikke push-es
pick 90560f1 Lagt til kafka.txt
pick 9077412 Slettet avsnitt i lingues.txt

# Rebase 16e1169..9077412 onto 16e1169 (6 commands)
...
```

Følg instruksjonene i lista over, og endre fila for å utføre riktige endringer. Når du er ferdig, lagre og steng editor.
Underveis vil `rebase` stoppe flere ganger og åpne ny editor. 
Det skal være nok informasjon i teksten hver gang til at du skal kunne utføre det som er nødvendig.

Etter at du er ferdig, kjør
```shell
git --no-pager log --oneline --graph -5
```
Du skal da få noe liknende
```text
* b2fd41b (HEAD -> oppgave8) Lagt til kafka.txt
* df8b2c2 Lagt til lingues.txt
* a84b31f Lagt til lorem.txt
* b986288 Lagt til cicero.txt
* 16e1169 (origin/main, origin/HEAD, main) Ferdig oppgave 7
```
Husk at om du ikke får det til kan du alltid gå tilbake til utgangspunktet med en `git reset hard`

