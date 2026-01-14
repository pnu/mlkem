# Määrittelydokumentti

Harjoitustyö: Algoritmit

## Aihe

Työn aiheena on ML-KEM algoritmin toteutus standardin
[Module-Lattice-Based Key-Encapsulation Mechanism Standard (FIPS 203)](https://csrc.nist.gov/pubs/fips/203/final) määrittelemän mukaisesti Pythonilla.

Pystyn vertaisarvioimaan myös TypeScript toteutuksia.

## Algoritmi

ML-KEM on kvanttiturvalliseksi suunniteltu avaintenvaihtomekanismi
jonka avulla yksityisen ja siitä johdetun julkisen avaimen haltijat
voivat luoda uuden keskenään jaetun salaisuuden (256-bittisen
satunnaisluvun). Tätä salaisuutta voidaan puolestaan käyttää symmetrisen
salausalgoritmin (kuten AES-256) avaimena varsinaisen salattavan
sisällön siirtämiseksi.

Mekanismi koostuu useista algoritmeistä, jotka on kuvattu standardissa.
Tässä työssä totetutetaan Python kirjasto joka toteuttaa tarvittavat
algoritmit, niihin oikeellisuuteen liittyvät testit, sekä minimaalinen
komentorivi-ohjelma jolla tehdään end-to-end yhteensopivuustesti jonkin
olemassaolevan yleisen toteutuksen kanssa. Lisäksi analysoidaan
algoritmien aika- ja tilavaativuutta.

## Harjoitustyön ydin

Kaikki varsinaisten ML-KEM algoritmien ulkopuolelle kuuluva toteutetaan
valmiilla kirjastoilla mikäli mahdollista. Tavoitteena on esimerkiksi
matriisien käsittely NumPy kirjatolla, mikäli se pitää koodin helposti
ymmärrettävänä.

Työn tavoitteena on ML-KEM algoritmien opiskelu ja dokumentoivan koodin
tuottaminen sen tueksi.
Suorituskyvyn optimointi (pl. NTT, joka on osa standardia) luettavuuden
kustannuksella tai esim. side-channel hyökkäysten torjuminen
suoritusjärjestystä, muistin käsittelyä tms. järjestelmätason
olosuhteita muuttamalla eivät kuulu tähän työhön. Ajan salliessa
näitä voidaan kuitenkin pohtia dokumentaatiossa.

## ML-KEM laskennallinen perusta

ML-KEM oikea toiminta ja turvallisuus perustuu "Decisional-Module-Learning
With Errors (D-MLWE)" ongelman oletettuun vaikeuteen, jonka matemaattisesta
taustasta puolestaan löytyy nimessä esiintyvät hilat (module-lattice).

Algoritmissa yksityinen avain ("decapsulation key") on $k$-ulotteinen
vektori jonka komponentit ovat $n$-asteisia polynomeja jotka valitaan
standardiin kuuluvien parametrien määrittelemällä tavalla satunnaisesti
polynomirenkaasta $R_q = \mathbb{Z}_q[X]/(X^n + 1)$. Polynomit kuvataan
sen kertoimista muodostettuina vektoreina joiden kerto- ja yhteenlaskut
tehdään $\mod q$.

Julkinen avain ("encapsulation key") muodostetaan paljastamalla
yksityisen vektorin ja satunnaisen $k × k$ matriisin tulo, johon
on lisätty vielä erillinen kohina-vektori. Laskennallinen
haaste (Learning With Errors) liittyy siihen, että tästä joukosta
alkuperäisen yksityisen avaimen oppiminen tai löytäminen on laskennallisesti
erittäin vaikeaa. Näin muodostetulla julkisella avaimella on kuitenkin
mahdollista tehdä tiettyjä laskutoimituksia, jotka vain alkuperäisen
yksityisen avaimen haltija voi palauttaa.

ML-KEM standardin keskeiset kiinteät parametrit ovat $q = 28 ⋅ 13 + 1 = 3329$
ja $n = 256$. Näiden valintaan liittyy käytännön syyt (tarvittavan avaimen
pituus on 256 bittiä) sekä alkuluvun $q$ ominaisuudet NTT-muunnoksessa.
Standardissa määritellyt eri salaustasot ML_KEM_512, ML_KEM_768 ja
ML_KEM_1024 poikkeavat ulottuvuuden $k$ ja satunnaisvalinnassa
käytetettävän jakauman osalta.

Harjoitustyössä ei syvennytä tätä tarkemmin algoritmin matemaattisen
perustaan tai sen ominaisuuksien todistamiseen, koska tämä on kattavasti
dokumentoitu ja tutkittu tieteellisissä julkaisuissa. 
