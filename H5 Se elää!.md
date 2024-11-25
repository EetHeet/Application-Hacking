# H5 Se elää!

## [Tehtävät](https://terokarvinen.com/application-hacking/#h5-se-elaa)

#### Tämä tehtävänanto sisältää pääosin [Lari Iso-Anttilan](https://github.com/lisoant) laatimia tehtäviä.

#### a) Lab1. Tutkiminen mikä on ohjelmassa vialla ja miten se korjataan. [lab1.zip](https://terokarvinen.com/application-hacking/lab1.zip)

#### b) Lab2. Selvitä salasana ja lippu + kirjoita raportti siitä miten aukesi. [lab2.zip](https://terokarvinen.com/application-hacking/lab2.zip)

#### c) Lab3. Kokeile Nora Crackmes harjoituksia tehtävä 3 ja 4 ja loput vapaaehtoisia. Tindall 2023: [NoraCodes / crackmes](https://github.com/NoraCodes/crackmes).

#### d) Lab4. Vapaaehtoinen: [Crackmes.one](https://crackmes.one/) harjoitus. Saatko salasanan selville? lab4.zip Moodlessa.

## Vastaukset

### A.

Tämä ohjelma ottaa kaksi merkkijonoa muuttaa niissä olevia kirjaimia ja tulostaa ne uudestaan

![image](https://github.com/user-attachments/assets/0c67c3de-c38a-476b-a2fd-b37e0a4ec9de)

Mutta, koska **bad_message** on **NULL** ja rivillä 7 koitetaaan viitata siinä olevaan kirjaimeen ja ohjelma kaatuu ja antaa **segmentation fault** error viestin

Ohjelma voidaan korjata niin että tehdään ehto lause, jos message on **NULL** niin ohjelma tulostaa error viestin

![image-1](https://github.com/user-attachments/assets/e786058e-e7c9-495f-8600-577ac181a3aa)

![image-2](https://github.com/user-attachments/assets/d7bb4011-7d01-4f18-ba41-55d493807f2d)

### B.

Huomasin, että kun käyttäjän antama salasana skannataan
niin ohjelma hyppää **mAsdf3a** nimiseen funktioon

![pimage-1-1](https://github.com/user-attachments/assets/f584c9f3-6022-4d82-8041-a4d850899741)

täällä funktiossa verrataan **%r12d** ja **%edx** nimisiä rekistereitä ja jos ne eivät ole samansuuruisia niin tästä funktiosta hypätään takaisin main funktioon

![pimage-2-1](https://github.com/user-attachments/assets/c759eeb5-6c98-4c85-a605-207a28622d13)

![pimage-3-1](https://github.com/user-attachments/assets/2891cfdc-5f1d-487c-b3f8-9c2ae1a8f7eb)

päätin asettaa breakpointin **mAsdf3a** funktioon ja katsoa mitä
nämä pitävät sisällään kokeilin salasanalla **moi**
 
![pimage-5-1](https://github.com/user-attachments/assets/96598a35-3c21-49b7-a249-c789a23f7bb1)

**$edx** oli saman pituinen kun salasana minkä laitoin siihen
ja ylhäällä olevista **strln** funktioista päättelin että tässä verrataan 
annetun salasanan pituutta ja halutun salasanan pituutta ja jos ne ovat samat niin ohjelma jatkaa toimintaa

### (kuva **strln** funktioista)

![image-29](https://github.com/user-attachments/assets/fed7e407-2be9-4dff-bc56-2cb638a68d5c)

Joten kokeilin teoriaa laittamalla salasanaksi **aaaaaaaa**

![pimage-6-1](https://github.com/user-attachments/assets/ff5685ff-a6b2-4bb2-8ea7-97897f329b56)

Eikä funktiosta enää hypätä pois **salasana_pituus = 8**

sitten painoin nextiä niin pitkälle kun antaa mennä ja huomasin kohdan
missä verrataan **%ecx** ja **%edx** rekisteriä ja jos ne ovat eri niin funktio hyppää takaisin **main** funktioon

![pimage-7-1](https://github.com/user-attachments/assets/0ff6a9ee-83a7-4988-a139-e122b6e215a5)

katsotaan mitä rekistereissä on

![pimage-8-1](https://github.com/user-attachments/assets/0309ba9b-657c-439b-a16f-b8288e8426f6)

Minulla ei ollut hajuakaan mitä nämä numerot tarkoittavat joten, kokeilin mitä käy jos muutan annetun salasanan
muutin sen **aaaaaaaa** --> **bbbbbbbb**

ja nyt numero näyttivät tältä

![pimage-9-1](https://github.com/user-attachments/assets/0d188bad-a5c4-4103-8ed0-bab38822a246)

**%ecx** oli noussut yhdellä, mutta **%edx** pysyi samana

koska kirjaimen **a** ascii arvo on 97 ja **b** on 98 päättelin, että
tässä verrataan käyttäjän antaman salasanan kirjaimia haluttuihin kirjaimiin ja numero 100 on **d**
ascii arvona muutin ensimmäisen kirjaimen siksi

eli vahdoin salasanan **bbbbbbbb** --> **dbbbbbbb**

![pimage-10-1](https://github.com/user-attachments/assets/5ac6c20b-3619-441f-afbc-52ab3643b4b2)

nyt numerot olivat samat ja funktio jatkoi toimintaa joten vaihdoin
kirjain kirjaimelta salasanaa kunnes pääsin **dgOMm-x1**

ja sain lipun

![pimage-11-1](https://github.com/user-attachments/assets/e547953e-3ac7-4c6d-80b2-85f699aba21d)

![pimage-12-1](https://github.com/user-attachments/assets/1b6b9a8f-fa1b-492e-8d34-0d7ec799cd1e)

### C.

## crackme03

Ohjelmaa toimii niin, että ensiksi katsotaan onko **%edi** rekisterissä arvo **2**

![image-4](https://github.com/user-attachments/assets/a3abcf11-50d8-48e2-818c-3f91e41a1b3f)

Jos ei ole niin ohjelma hyppää riville **main+113** ohjelma tulostaa **Need exactly one argument** ja loppuu

![image-5](https://github.com/user-attachments/assets/7fbbfdf6-51b0-4978-9b7e-67f43b13cf06)

![image-6](https://github.com/user-attachments/assets/19becd39-9969-44c4-988f-d7d59f84c021)

Mutta, jos ohjelmassa on kaksi argumenttia (ohjelman nimi ja käyttäjän syöte) niin päästään riville **main+45** josaa katsotaan onko rekisterissä **%rax** arvo **6**

![image-7](https://github.com/user-attachments/assets/b786774e-1960-404c-b0b5-724db5b1157e)

Jos näin ei ole niin ohjelma hypää riville **main+82** tulostaa **No %s is not correct** ja loppuu

![image-8](https://github.com/user-attachments/assets/8df842fd-54d9-4015-a7c0-f73f9c4041d9)

![image-9](https://github.com/user-attachments/assets/d9c76823-2a49-4cf6-a95d-36a762d50888)

Eli rekisterin **%rax** arvo on käyttäjän syötteen pituus joten halutun salasanan pituus on 6 merkkiä, jos näin on niin ohjelma hyppää **check_pw** nimiseen funktioon

![image-10](https://github.com/user-attachments/assets/0c659414-8382-4c36-af8b-2922a1103b75)

Laitetaan breakpoint sinne ja käytetään salasanaa **abcdef**

![image-11](https://github.com/user-attachments/assets/87e61b7e-a913-4383-a844-2e587204bae3)

Tässä verrataan **%cl** rekisterissä olevaa arvoa **%rdi,%rax,1** juttuun (en tiedä miksi tätä kutstutaan) ja jos ne eivät ole sama nii ohjelmaa loppuu

**%cl** arvo on 110 

![image-12](https://github.com/user-attachments/assets/c5ef2b6d-c623-4f6e-bf9e-f25e1cc9f7b7)

koska ohjelma loppui salasanalla **abcdef** niin kokeilin mitä tapahtuu kun ensimäisen merkin vaihtaa **n** jonka ascii arvo on 110

Nyt ohjelma ei loppunut ja funktio **check_pw** alkoi alusta, joten katsoin että onko **%cl** arvo muuttunut

![image-13](https://github.com/user-attachments/assets/499109b7-0cb9-418d-821a-e894cb78b215)

se oli nyt 68 joka on ascii arvo kirjaimelle D

jatkoin tätä niin kauan kunnes sain salasanan **nDoEiA** ja se oli oikein

![image-14](https://github.com/user-attachments/assets/c9856378-4d28-48df-be80-fce51fa2bad3)

## crackme03e

Tämä ohjelma toimi tismalleen samalla tavalla, mutta nyt halutun salasanan pituus oli 8

![image-15](https://github.com/user-attachments/assets/ddc5630d-cc79-42bf-8427-d838ff9c8299)

Tässä oikea salasana oli **sfuwxoue**

![image-16](https://github.com/user-attachments/assets/c89abe3d-9cc6-4bad-abc1-58d6b3954b2f)

## crackme04

Tässä tehtävässä pitää pääst kahden cmp veretailun "läpi"

![image-17](https://github.com/user-attachments/assets/c767062a-964c-4126-99aa-8a5247b3a731)

ensimmäisessä katsotaan onko **%edi**:ssä oleva arvo 16

toisessa katsotaan onko **%ecx**:ssä oleva arvo 1762

Ensimmäisen **cmp** lauseen epäilin olevan käyttäjän syötteen pituus, joten kokeilin jos laittaa salasanaki **aaaaaaaaaaaaaaaa** 

![image-18](https://github.com/user-attachments/assets/091ba978-a29c-48a0-bd4a-61822abf8056)

nyt **%edi** arvo on 16 joten se on käyttäjän syötteen pituus

seuraavassa **%ecx**:ssä oleva arvo pitää olla 1762

antamallani salasanalla se oli 

![image-19](https://github.com/user-attachments/assets/ababe23f-b80d-46cb-9831-2b89f7043e08)

En tienny mitä tuo tarkoittaa, joten käynnistin ohjelman uudestaa ja asetin **watchpointin** seuramaan **%ecx

Huomasin että tässä loopissa

![image-20](https://github.com/user-attachments/assets/c5c49229-de74-4f53-89a4-86e115d86f90)

**%ecx** arvo nousi joka kierroksella 97

![image-21](https://github.com/user-attachments/assets/b89d064d-6d72-45e1-903f-631ae646fb15)

koska salasanani oli **aaaaaaaaaaaaaaaa** ja a kirjaimen ascii arvo on 97 tuli rekisterin **%ecx** lopulliseksi arvoksi 1552 = 16 * 97

Tästä päättelin, että salasanan merkeillä ei ole väliä vaa sillä on väliä, että niitä on 16 ja kaikkien kirjaiten ascii arvon summaksi tulee 1762

Kokeilin salasanaksi **nnnnnnnnnnnnnnnp** n kirjaimen ascii arvo on 110 ja 15 * 110 = 1650 ja p kirjaimen ascii arvo on 112 ja 1650 + 112 = 1762

![image-22](https://github.com/user-attachments/assets/0144b562-da4d-48c7-a7c7-ae4883d9ccff)

ja **nnnnnnnnnnnnnnnp** oli oikein

## crackme04e

Tämä ohjelma toimi tismalleen samoin mutta nyt merkkejä pitää olla 32

![image-23](https://github.com/user-attachments/assets/be095867-5669-4ece-8036-9e07f6fbf950)

![image-24](https://github.com/user-attachments/assets/79e27ce1-8c2b-41b9-9631-ce5f079e6f4a)

ja kirjaiten ascii arvojen summaksi pitää tulla 3542

![image-25](https://github.com/user-attachments/assets/361577ab-abb6-4410-9dec-14fa70275506)

![image-26](https://github.com/user-attachments/assets/a6184482-2c37-4b3a-aa3e-ed15dddb25e7)

tähän laitoin salasanksi **nnnnnnnnnnnnnnnnnnnnnnnnnnnnnnyy** eli 30 n kirjainta ja 2 y kirjainta

![image-27](https://github.com/user-attachments/assets/30838599-a59d-417b-bc08-0b9802c74e02)

### D.

Tämä oli hiukan vaikeampi ja minulta loppui aika kesken, joten ratkaisen tämän myöhemmin ja julkaisen sen tänne.

### Lähteet

[ASCII table](https://www.asciitable.com/)
