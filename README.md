# Webserver
- Guide til opsætning af en webserver

1.

I denne guide bruger jeg DigitalOcean til at opsætte min webserver.
Til at starte med opretter jeg en ny såkaldt "Droplet" og vælger CentOS v. 6.9 32 bit.

Derefter vælger jeg størrelsen af min Droplet, i dette tilfælde vælger jeg bare den mindste og billigeste udgave man kan få.

Når det er gjort, skal man vælge lokationen af hvor sin server bliver hostet, det er en god idé at vælge et land der ligger tæt på (f.eks. Frankfurt eller Amsterdam).

Til sidst giver jeg min server et navn, og trykker "Create".

2.

Nu ligger min server og kører, og for at forbinde til den, bruger jeg et program kaldet "PuTTY".

I PuTTY skal jeg bruge min servers IP adresse, som kan findes på DigitalOcean. Jeg kopiere IP adressen ind, og sørger for at gemme den, som en "Saved Session", så jeg let kan tilgå den igen.

Jeg klikker på min "Saved Session", som jeg lige har lavet, og trykker "Open".

Jeg er nu forbundet til min server, igennem en terminal, og bliver bedt om at logge ind med et brugernavn og kodeord. Brugernavnet og kodeordet bliver sendt til din mail, fra DigitalOcean.

Jeg udfylder brugernavnet og kodeordet, og logger ind. Det er vigtigt at huske, at man ikke kan copy paste på samme måde i terminalen, som vi er vant til på computeren. Når du skal kopiere dit kodeord ind, skal du blot højre klikke i terminalen og dit kodeord vil nu være kopieret ind, selvom man ikke kan se det.

3.

Når man er logget ind skulle følgende tekst i terminalen gerne se således ud:

[root@test ~]#

Vi kan ny begynde at opsætte serveren som vi vil.

4.

Til at starte med, installere vi et program kaldet "Nano", som er et lille simpel text editor program til terminalen.

For at gøre dette, skal vi skrive følgende:

[root@test ~]# yum install nano

Serveren vil nu installere programmet, og når det er færdigt, spørger den om alt er som det skal være, hvorefter vi bare skriver "y" for "Yes" og trykker Enter.

Vi har nu fået installeret Nano på serveren.

5.

Det næste vi skal gøre er at installere MySQL. Det gør vi på præcist samme måde som før, denne gang skriver vi bare følgende i terminalen:

[root@test ~]# yum install mysql-server

Når MySQL er installeret, skal vi huske at starte den, så vi kan arbejde med den. Det gør vi ved at skrive følgende:

[root@test ~]# service mysqld start

Hvis alt går som det skal, skulle der gerne stå:

Starting mysqld: [ OK ]

For at stoppe sql serveren, skrives det samme som før, bare med "stop" i stedet for "start". Du kan også genstarte den, ved at skrive "restart".

Vi skal nu have konfigureret vores MySQL og sørge for at den er sikret. Det gør vi ved at skrive følgende:

[root@test ~]# sudo /usr/bin/mysql_secure_installation

Dette åbner en lille form for installation guide, som vi bare skal følge.

Til at starte med, spørger den om et kodeord, men da vi ikke har opsat noget kodeord endnu, efterlader vi den bare blank, ved at trykke Enter.
Vi bliver så spurgt om vi vil tilføje vores eget kodeord, hvilket vi gerne vil, vi skriver derfor "Y" for "Yes" og trykker Enter.

Når det nye kodeord er udfyldt, spørger den om vi vil fjerne såkaldte anonyme brugere, men da vi arbejder i et test miljø lige nu, er der ingen grund til at fjerne dem, så vi skriver "n" for "No" og trykker Enter.

Resten af punkterne er sådan set ikke så relevante for os, når vi arbejder i et test miljø, så vi skriver bare "n" til resten af punkterne, som installations guiden går igennem.

6.

Når MySQL er installeret, skal vi til at installere Node.js. For at gøre det, starter vi med at installere noget kaldet for "epel-release". Det gør vi ved at skrive følgende:

[root@test ~]# yum install epel-release

Derefter installere vi selve Node.js, ved at skrive:

[root@test ~]# yum install nodejs

Når Node.js er installeret, skal vi installere npm (Node Package Manager), så vi kan arbejde med de forskellige moduler. Det gøres ved at skrive:

[root@test ~]# yum install npm

Vi skal nu installere et modul kaldet for "n", som er et modul der interaktivt kan håndterer din Node.js versioner. Inden vi installere dette, er det vigtigt at vi skrive følgende i terminalen:

[root@test ~]# sudo npm config set strict-ssl false

"sudo" står for "Super User Do". Gør vi ikke dette, kan de skabe problemer senere hen, når vi skal clone vores GitHub repository.

Når det er gjort, kan vi begynde at installere n. Det gør vi ved at skrive følgende:

[root@test ~]# npm install -g n

"-g" står for "global", hvilket vil sige vi installere det globalt på serveren.

Vi skal nu opdatere vores Node.js, dette gør vi ved hjælp af n og skriver følgende i terminalen:

[root@test ~]# n lts

For at tjekke hvilken version af Node.js du kører på din server, kan du skrive følgende:

[root@test ~]# n

Når Node.js er opdateret, kan det være en god idé at genstarte din server. Jeg lukker derfor PuTTY ned, og åbner det igen.

7.

Nu hvor vores Node.js kører som det skal, skal vi have installeret en "Process Manager", kaldet for PM2, til vores applikationer.
Det gør vi ved at skrive følgende:

[root@test ~]# npm install -g pm2

Dette vil vi gerne have til at køre, hver gang vores server startes op, og det gør vi ved at skrive:

[root@test ~]# pm2 startup

8.

Vi skal nu installere Git, så vi kan komme til at arbejde med vores repositories.
Dette gøres ved at skrive følgende:

[root@test ~]# yum install git

Når det er gjort, skal vi konfigurere vores Git, så den kender vores bruger navn og bruger email til vores GitHub.
Det gør vi ved at skrive:

git config --global user.name "Dit navn"
git config --global user.email "din@email.dk"

For at tjekke om det virker, kan vi bruge vores text editor Nano, som vi installeret i starten, til at se om de indtastede oplsyninger er blevet skrevet korrekt.
Det gør vi ved at skrive følgende:

[root@test ~]# nano ~/.gitconfig

Resultatet af det, skulle gerne være, at der kommer til at stå følgende i Nano:

name = Dit navn
email = din@email.dk

9.

For at kunne logge ind på GitHub igennem vores server, skal vi oprette et nøglesæt.
Dette gør vi ved at skrive følgende i terminalen:

[root@test ~]# ssh-keygen -t rsa

Den vil så spørge hvor nøglen skal gemmes, det skulle gerne være i /root/.ssh/id_rsa, den piller vi derfor ikke ved og trykker bare Enter.
Så spørger den om vi ønsker et såkaldt "passphrase", hvilket vi ikke vil i dette tilfælde, så vi trykker bare Enter, og så Enter igen, så vi for et tomt passphrase.

Nu er vores nøgle oprettet, og vi kan tilgå den via. Nano ved at skrive:

[root@test ~]# nano ~/.ssh/id_rsa.pub

Det er dog en dårlig idé at bruge Nano i dette tilfælde, da vi ikke kan få udskrevet hele nøglen. For at udskrive hele nøglen, skrives følgende i stedet:

[root@test ~]# cat ~/.ssh/id_rsa.pub

Hele nøglen vil nu udskrives, og vi kan kopiere den ved bare at markere det hele (du kan ikke bruge Ctrl-C i terminalen).
Det er vigtigt at du får det hele med, både det der står i starten (ssh-rsa) og det der står til sidst (root@ditservernavn).
Det skulle gerne se nogenlunde sådan her ud:

ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAyZG7Ta5vOqm6p+Cg8SGf+Jdzcqk2b4sVZNxqNpWUqY+239MXQ1VVXAPEVYIlIsc6mONz6H0Zsq4epV0pqFTUfNfGQCOClpctKymWNfaZQ+SoAjZwmyuJkAVnQ+say6z6Mo5NI8z45S6kbuY9x54cNRAdR5zPARWcSwopNkcbYAYHSTC2r47CggQeVNCFFV4R9FftC3IkIRh7VaTiCPgHnj4UMaxftUxdFavTjNAvEFBmy/xYOH1BsH1uzzrF/DwcceohtXaVPSc+552mY7GpZw/ldfH0KUtrPYiWuNiBbHl5a7h2QvrJfXbjK/J/tiHDJkNtxVbCFdNV4jZFLUSZmQ== root@test

Vi skal nu have kopiret nøglen til GitHub, det gøres som følgende:

-> Gå ind på GitHub
-> Ind under "Settings"
-> Tryk på "SSH and GPG keys"
-> Vælg "New SSH key"
-> Kopier nøglen ind (husk at give den en titel)
-> Tryk "Add SSH key"

Nøglen er nu kopiret ind, og vi kan dermed så småt begynde at clone og pulle vores repositories.

10.

Vi skal nu oprette en mappe til vores repositories, dette gør vi ved at skrive:

[root@test ~]# mkdir ~/www

"mkdir" står for "Make directory", og mappen kalder vi for "www".

Når det er gjort, vil vi gerne navigere ind i mappen. Dette gør vi ved at skrive følgende:

[root@test ~]# cd ~/www

"cd" står for "Change directory", og så stien på den mappe vi gerne vil ind i.

Når vi er navigeret ind i mappen skulle vores terminal gerne se således ud nu:

[root@test www]#

11.

Inde i vores nye mappe, vil vi nu lave en clone af vores repository. Dette gør vi ved at skrive følgende i terminalen:

[root@test www]# git clone git@github.com:brugernavn/repository

Den spørger så om du er sikker på, at du vil forbinde til GitHub, her skriver du bare "yes" og trykker Enter.

Når det er gjort, skulle du gerne kunne se dit repository i mappen, ved at skrive "ls" som står for "list", og giver dig en lille liste af de filer som er i den konkrete mappe. Det vil sige du skriver:

[root@test www]# ls

Når du har skrevet det, skulle du gerne se navnet på dit repository oppe over kommandolinjen.

Du kan nu prøve at ændre i dit repository, og så lave et pull. For at lave et pull, skal du navigere ind i dit repository, ved hjælp af cd:

[root@test www]# cd ditrepositorynavn

Inde i dit repository kan du evt. lave en ls, for at se hvilke filer der er derinde. Når du har ændret i dit repository, og gerne vil lave et pull skal du skrive følgende:

[root@test webserver]# git pull origin master

Hvis du vil pulle fra andre såkaldte "branchs" end "master", kan du gøre dette ved bare at skrive navnet på dit branch, i stedet for master.
Du kan også nøjes med bare at skrive "git pull".
