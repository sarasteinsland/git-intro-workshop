# Oppgave 1
I denne oppgaven vil vi klone repoet lokalt.
For å kunne 'pushe' endringer til et remote repo vil det kreve at alle enten setter opp et remote repo i sin egen github-konto, eller lager et lokalt, filbasert repo.
En kan også hoppe over dette, men en vil da ikke kunne gjennomføre deloppgaver der et remote repo er påkrevet.
## Klone Workshop-Repoet
For å kunne jobbe med workshop-en må en klone workshop-repoet fra GitHub.
I en kommandolinje, flytt deg inn i en folder der du har prosjekter. Du kan også bare klone det i hjemme-folderen din.
```shell
git clone https://github.com/RasmanTuta/git-intro-workshop.git
```
Det blir da opprettet en ny folder med samme navnet som repoet: `git-intro-workshop`
Om du ønsker å ha et annet navn på repoet, så legger du bare ønsket navn på slutten av kommandoen:
```shell
git clone https://github.com/RasmanTuta/git-intro-workshop.git git-intro-123
```
Om en har [satt opp en github-konto med SSH-nøkkel](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) kan en også klone repoet med SSH:
```shell
git clone git@github.com:RasmanTuta/git-intro-workshop.git
```

## Bytte Remote Repository
For at ikke alle som jobber med workshop-en skal gå i beina på hverandre, må vi bytte `remote` på den lokale klonen.
Dette kan vi gjøre på et par forskjellige måter.
### 1. Opprette et nytt repo i egen GitHub-konto
I en nettleser, gå til hjemmesiden din på github: [https://github.com/din-bruker-her](https://github.com/<din bruker her>)
Oppe i høyre hjørnet, trykk på `+` og velg `New Repository`
Gi det nye repoet et navn og trykk `Create repository`

Vi skal nå følge beskrivelsen under `…or push an existing repository from the command line` med en liten endring:
```shell
git remote remove origin
git remote add origin https://github.com/<Din-Bruker-Her>/<ditt-repo-navn-her>.git
git branch -M main
git push -u origin main
git branch --unset-upstream
git push --set-upstream origin --all
```

### 2. Opprette et nytt fil-basert lokalt repo

```shell
mkdir -p repo.git
git init --bare ./repo.git
git remote add origin ./repo.git
git branch -M main
git push -u origin main
git branch --unset-upstream
git push --set-upstream origin --all
```

## Sjekke om ny remote har blitt korrekt satt
Om du kjører 
```shell
git remote -v
```
så skal resultatet vise to linjer med referanse til det `origin` ble satt til
```text
origin  git@github.com:RasmanTuta/git-intro-workshop.git (fetch)
origin  git@github.com:RasmanTuta/git-intro-workshop.git (push)
```

Du kan også kjøre 
```shell
git --no-pager branch -a
```
for å sjekke at alle brancher er kommet med.