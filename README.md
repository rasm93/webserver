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


