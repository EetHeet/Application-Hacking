# H2 Break & unbreak

## [Tehtävät](https://terokarvinen.com/application-hacking/#h2-break--unbreak)

#### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
- OWASP: OWASP Top 10: [A01 Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)
- PortSwigger: [Access control vulnerabilities and privilege escalation](https://portswigger.net/web-security/access-control)
- Karvinen 2006: [Raportin kirjoittaminen](https://terokarvinen.com/2006/raportin-kirjoittaminen-4/)
- Vapaaehtoinen: PortSwigger 2020: [What is SQL injection? - Web Security Academy](https://www.youtube.com/watch?v=wX6tszfgYp4) (noin 10 min video)

#### a) Murtaudu 010-staff-only. Ks. Karvinen 2024: [Hack'n Fix](https://terokarvinen.com/hack-n-fix/)

#### b) Korjaa 010-staff-only haavoittuvuus lähdekoodista. Osoita testillä, että ratkaisusi toimii.

#### c) Ratkaise dirfuzt-1 artikkelista Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf.](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/) Tämä auttaa 020-your-eyes-only ratkaisemisessa.

#### d) Murtaudu 020-your-eyes-only. Ks. Karvinen 2024: [Hack'n Fix](https://terokarvinen.com/hack-n-fix/)

#### e) Korjaa 020-your-eyes-only haavoittuvuus. Osoita testillä, että ratkaisusi toimii.

#### f) Vapaaehtoinen. Johdantotehtävä, joka auttaa 010-staff-only ratkaisemisessa. Ratkaise Portswigger Academyn ["Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data".](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)

#### g) Vapaaehtoinen. Johdantotehtävä, joka auttaa 010-staff-only ratkaisemisessa. Ratkaise Portswigger Academyn ["Lab: SQL injection vulnerability allowing login bypass"](https://portswigger.net/web-security/sql-injection/lab-login-bypass)

## Vastaukset

### X.

### A.

Tehtävässä on kerrottu että pin koodilla 123 saadaan oma salasana

![image](https://github.com/user-attachments/assets/c2081ac8-1db0-479a-a418-a33e8a1353d6)

Tehtävässä on myös kerrottu että ohjelma tekee tälläisen SQL kyselyn

`SELECT password FROM pins WHERE pin='123'`

Joten jos ohjelmassa ei ole mitään tarkistusta voimme kokeilla saada sen tulostaa kaikki salasanat jos laitamme **pin** kohtaan `' OR 1=1 --`

![image-1](https://github.com/user-attachments/assets/9af3f706-cbc6-4481-a874-244fa0b4f819)

Mutta html dokumentti estää muita kuin numeroita tässä kentässä, joten "korjataan" se muokkaamalla html dokumenttia

Muunnetaan tätä lausetta

![image-2](https://github.com/user-attachments/assets/1710da03-8c9c-4bd5-a90f-074bd4a775d6)

Poistetaan `type="number"`

![image-3](https://github.com/user-attachments/assets/f674c444-5a36-48b7-b937-2df24df1670d)

Nyt saamme eri salasanan, joten voimme olettaa, että tämä toimii

![image-4](https://github.com/user-attachments/assets/1bd18603-c90f-426c-91b0-3b9e4fe025d1)

Mutta koska tämä ei tulosta kaikkia salasanoja voidaan olettaa että ohjelmassa on lause joka tekee sen että se tulostaa vain ensimmäisen olion, joten jos haluamme nähdä muita salasanoja käytetään `LIMIT` parametria

Koska tiedämme että ensimmäinen salasana tietokanssa on **foo** kokeillaan toista salasanaa lausekkeella `' OR 1=1 LIMIT 2,1 --`

![image-5](https://github.com/user-attachments/assets/da59a979-db5b-49ef-a31c-b67a00591dc6)

Ja tällä saimme admin salasanan

### B.

Aikaisemmasta tehtävästä voimme päätellä, että ohjelma ei suorita minkäänlaista tarkistusta tai sanitointia annettuun pinniin ja koodia lukemalla voimme varmistaa tämän

![image-6](https://github.com/user-attachments/assets/fae438da-ac72-4836-95cd-cbac90e21c9a)

Koodista huomataan että **pin** otetaan suoraan html lomakkeesta ja syötetään se **SQL** lausekkeeseen

Ensimäisenä voimme tehdä kohdan joka varmistaa sen, että anettu **pin** on numero ja antaa käyttäjälle virhe ilmoituksen jos näin ei ole

![image-7](https://github.com/user-attachments/assets/3030e159-f391-4ff4-8130-1bc248a25b1f)

Nyt lisäsin kohdan, joka tarkistaa onko annettu **pin** numero ja jos ei ole antaa ilmoituksen käyttäjälle **Give number**

![image-9](https://github.com/user-attachments/assets/6702c0db-0ae0-44d1-8e42-09eefc122549)

Tämä ei kuitenkaan välttämättä anna kokonaan suojaa **SQL injektiota** vastaan niin lisätään muutama kohta, jotka sanitoivat käyttäjän antaman syötteen

![image-8](https://github.com/user-attachments/assets/f0d945aa-2eb6-4ebb-9f91-a062b778b125)

Muutoksissa käyttäjän antama pin lisätään erilliseen muuttujaan, joka sanitoidaan ennenkuin se syötetään SQL:llään esim lauseke `' OR 1=1 LIMIT 1,2 -- ` niin nyt ohjelma tekisin lausekkeen näyttämään tältä `'' OR 1=1 LIMIT 1,2 --` jolloin ensimmäinen heittomerkki luetaan tekstinä eikä **SQL** komentona

### C.

Olen tehnyt tämän tehtävän aikaisemmin sen löytää täältä **https://github.com/EetHeet/Tunkeutumistestaus/blob/main/h3%20Fuff%20faster.md, kohta "b) Salainen, mutta ei multa. Ratkaise dirfuzt-1"**

### D.

Tehtävissä oltiin aikaisemmin annettu vinkki että **ffuf** ohjelmasta on tässä kohtaa hyötyä joten alotin **fuzzaamalla** palvelimen

![image-10](https://github.com/user-attachments/assets/43e35dba-dccb-4f4f-a94b-6b85afcb7b76)

Tällä löysin piilotetun sivun **admin-console**, joten kokeilin mitä käy jos koittaa päästä sinne

![image-11](https://github.com/user-attachments/assets/de430019-664d-465d-9423-ef22030c5a7a)

Vastaani sain kirjautumis ruudun

Seuraavaksi tein käyttäjän 

![image-12](https://github.com/user-attachments/assets/97fa8066-a328-4851-a063-4d7aab1bd5fd)

![image-13](https://github.com/user-attachments/assets/3910af1c-5970-4c99-b8cc-83b08e4dcd34)

Nyt kokeilin mitä käy kun koittaa päästä **Admin dashboard**

![image-14](https://github.com/user-attachments/assets/50325ff6-aab3-4176-8fed-704131f98542)

Siitä tulee vain virheilmoitus, että minulla ei ole lupaa päästä sinne, mutta **Admin dashboard** ei ole sama kuin aikaisemmin löydetty **admin-console**, joten kokeilin mitä käy kun koittaa päästä sinne

![image-15](https://github.com/user-attachments/assets/ac642d15-ddcc-4292-a47b-585960c63089)

Tämä toimi

### E.

Aikaisemmasta tehtävästä voi päättellä, että ohjelma ei kunnolla kato kellä on oikeus päästä **admin-console** sivulle, joten se pitää korjata

Tässä kohtaan olin hiukan jumissa, joten katsoin [vinkkejä](https://terokarvinen.com/hack-n-fix/#tips) ja siellä oli että **"View permissions are checked in views.py"**, joten päätin katsoa sinne 

![image-16](https://github.com/user-attachments/assets/e2da3573-a8f9-49c0-9aaa-4c1b96e49625)

Tässä huomasin, että **AdminSHowAllView** kohdasta puuttuu `self.request.user.is_staff`, joten lisäsin sen siihen

![image-17](https://github.com/user-attachments/assets/11c13e06-5350-437f-be16-0f62c0a8ad59)

Ja kokeilin päästä **admin-console sivulle uudestaan

![image-18](https://github.com/user-attachments/assets/ad2b9b51-a825-4d1c-867f-14699fb78f82)

Ja nyt ohjelma estää minua pääsemästä sinne niin kuin kuulukin

Aikaisemmin funktio palautti **True** arvon jos käyttäjä oli vain kirjautunut sisään mutta nyt jotta sen palauttaa **True** arvon käyttäjän pitää olla kirjautuneena sisään ja sen pitää olla myös staff

### F.

![image-19](https://github.com/user-attachments/assets/fe6520c8-cc6e-4987-8483-b93963964fed)

Tehtävässä on kerrottu että ohjelma hakee tälläisellä lausekkeella dataa `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

Eli ohjelma hakee tuotteet annetusta kategoriasta ja `released = 1` tarkoittaa, että ne on julkaistu, joten jos syötämme **category** kohtaan ` ' OR 1=1 -- ` niin ohjelma skippaa kokonaan **released** kohdan ja tulostaa kaikki tuotteet kaikista kategorioista oli ne julkastu tai ei

![image-20](https://github.com/user-attachments/assets/01bfb6fe-79f5-43f5-9c27-f03bfc1e35a9)

### G.

![image-21](https://github.com/user-attachments/assets/9ca04d0f-2f71-4981-9208-b21dd63a4dc9)

Tässä tehtävässä on kerrottu että kirjautumis funktio sisältää **SQL injection** haavoittuvuuden, joten kokeilin laittaa salasana kohtaan `' OR 1=1 -- `

![image-22](https://github.com/user-attachments/assets/01d47267-7e6b-43bd-b1ce-1a560504af51)

Ja se toimi

Tätä en osaa oikeen selittää järkevästi tarkemmin

### Lähteet
