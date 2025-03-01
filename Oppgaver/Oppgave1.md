# Oppgave 1
I denne oppgaven vil vi klone repoet lokalt.
For å kunne 'pushe' endringer til et remote repo vil det kreve at alle enten setter opp et remote repo i sin egen github-konto, eller lager et lokalt, filbasert repo.

## Opprette en kopi av Remote Repository
For at ikke alle som jobber med workshop-en skal gå i beina på hverandre, må på en eller annen måte opprette en kopi av det orginale repoet.
Dette kan vi gjøre på et par forskjellige måter.
### 1. Bruke lokalt fil-basert lokalt repo
Vi kan sette opp et fil-basert lokalt repo og flytte koden inn i dette.
Ved å sette `origin`til å peke på dette, vil det fungere som et remote repo selv om det befinner seg lokalt på disken din.
```shell
mkdir repo.git
git init --bare repo.git
git clone --mirror https://github.com/RasmanTuta/git-intro-workshop.git mirror
cd mirror
git remote set-url origin ../repo.git
git push --mirror origin
cd ..
rm -rf mirror
git clone repo.git git-intro-workshop
cd git-intro-workshop
```

### 2. Opprette et nytt repo i egen GitHub-konto
I en nettleser, gå til hjemmesiden din på github: [https://github.com/din-bruker-her](https://github.com/<din bruker her>)
Oppe i høyre hjørnet, trykk på `+` og velg `New Repository`
Gi det nye repoet et navn og trykk `Create repository`

Normalt når en oppretter ett nytt repo på GitHub for å jobbe med et repo opprettet lokalt, Vile en følge oppskriften under `…or push an existing repository from the command line`.
Vi må følge en litt annen tilnærming som ligner litt mere punkt 1 over:
```shell
git clone --mirror https://github.com/RasmanTuta/git-intro-workshop.git mirror
cd mirror
git remote set-url origin https://github.com/<Din-Bruker-Her>/<ditt-repo-navn-her>.git
git push --mirror origin
cd ..
rm -rf mirror
git clone https://github.com/<Din-Bruker-Her>/<ditt-repo-navn-her>.git git-intro-workshop
cd git-intro-workshop
```

### 3. Bruke `fork` fra GitHub
GitHub har en `fork` funksjon som lar deg lage en kopi av et repo inn i din GitHub-bruker:

 - Gå til [https://github.com/RasmanTuta/git-intro-workshop](https://github.com/RasmanTuta/git-intro-workshop)
 - Oppe til høyre vil du se teksten `Fork` med en pil ned. Trykk pilen og velg `Create a new fork`
 - Følg instruksjonene som følger.
 - Deretter kloner du din nye fork lokalt:
```shell
git clone https://github.com/<Din-Bruker-Her>/<ditt-fork-navn-her>.git git-intro-workshop
cd git-intro-workshop
```

## Sjekke om ny remote har blitt korrekt satt
Om du kjører 
```shell
git remote -v
```
så skal resultatet vise to linjer med referanse til det `origin` ble satt til
```text
origin  ./repo.git (fetch)
origin  ./repo.git (push)
```

Du kan også kjøre 
```shell
git --no-pager branch -a
```
for å sjekke at alle brancher er kommet med.


[Oppgave 2](./Oppgave2.md)