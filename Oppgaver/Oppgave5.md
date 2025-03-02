# Endringer fra Main-Branch

Om en feature tar så lang tid at det rekker å skje endringer i `main` før dine egne endringer er det lurt å jevnlig ta disse inn i din branch.
Dette kan gjøres på to måter: Merge av `main` inn i din feature branch eller å 'rebase' din feature branch på toppen av `main`
Det er praktisk å gjøre dette på main sin `remote tracking branch` som kan navngis med `origin/main`.

For å være sikker på at denne er i synk med det som ligger sentralt, kjører man `git fetch`
Deretter kjører man enten `git merge origin/main` eller `git rebase origin/main`

Om en har følgende historikk:
```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'base' } }%%
gitGraph
    commit id: "A"
    branch feature
    checkout feature
    commit id: "B"
    checkout main
    commit id: "C"
    checkout feature
    commit id: "D"
    checkout main
    commit id: "E"
```
Om en kjører `git merge origin/main`, vil en få:
```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'base' } }%%
gitGraph
    commit id: "A"
    branch feature
    checkout feature
    commit id: "B"
    checkout main
    commit id: "C"
    checkout feature
    commit id: "D"
    checkout main
    commit id: "E"
    checkout feature
    merge main id: "merge main into feature"
```
Om en kjører `git rebase origin/main` vil en få en mer lineær historikk:
```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'base' } }%%
gitGraph
    commit id: "A"
    checkout main
    commit id: "C"
    checkout main
    commit id: "E"
    branch feature
    checkout feature
    commit id: "B'"
    commit id: "D'"
```
En diskusjon som aldri vil bli ferdig er om det ene er bedre enn det andre.
Her er det stor grad av personlige preferanser som spiller inn.




 