# H4 Kääntöpaikka

## [Tehtävät](https://terokarvinen.com/application-hacking/#h4-kaantopaikka)

#### x) Lue/katso/kuuntele ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
- Hammond 2022: [Ghidra for Reverse Engineering (PicoCTF 2022 #42 'bbbloat')](https://www.youtube.com/watch?v=oTD_ki86c9I) (Video, noin 20 min)
- Vapaaehtoinen: € Eagle and Nancy 2020: [The Ghidra Book: 2. Reversing And Disassembly Tools](https://learning.oreilly.com/library/view/the-ghidra-book/9781098125684/xhtml/ch02.xhtml#ch02lev29) (Usein suositeltu kirja Ghidrasta)

#### a) Asenna Ghidra.

#### b) rever-C. Käänteismallinna packd-binääri C-kielelle Ghidralla. Etsi pääohjelma. Anna muuttujielle kuvaavat nimet. Selitä ohjelman toiminta. Ratkaise tehtävä binääristä, ilman alkuperäistä lähdekoodia. [ezbin-challenges.zip](https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip)

#### c) Jos väärinpäin. Muokkaa passtr-ohjelman binääriä (ilman alkuperäistä lähdekoodia) niin, että se hyväksyy kaikki salasanat paitsi oikean. Osoita testein, että ohjelma toimii. [ezbin-challenges.zip](https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip)

#### d) Nora CrackMe: Käännä binääreiksi Tindall 2023: [NoraCodes / crackmes](https://github.com/NoraCodes/crackmes). Lue [README.md](https://github.com/NoraCodes/crackmes/blob/master/README.md): älä katso lähdekoodeja, ellet tarvitse niitä apupyöriksi. Näissä tehtävissä binäärejä käänteismallinnetaan. Binäärejä ei muokata, koska muutenhan jokaisen tehtävän ratkaisu olisi vaihtaa palautusarvoksi "return 0".

#### e) Nora crackme01. Ratkaise binääri.

#### f) Nora crackme01e. Ratkaise binääri.

#### g) Nora crackme02. Nimeä pääohjelman muuttujat käänteismallinnetusta binääristä ja selitä ohjelman toiminta. Ratkaise binääri.

#### h) Vapaaehtoinen: Ja sen yli. Crackme01 on useampia ratkaisuja. Montako löydät? Miksi?

#### i) Vapaaehtoinen: Pyytämättäkin. Crackme02 on kaksi ratkaisua. Löydätkö molemmat?

#### j) Vapaaehtoinen, hieman haastavampi: A ray. Nora crackme02e. Ratkaise binääri.

## Vastaukset

### X.

- Hammond tarkastelee binääriä `file`, `strings`, `strace`, `objdump` ja `ltrace` komenoilla, mutta ei löydä oikeen mitään
- Hammond asentaa Ghidran ja alkaa tutkia binääriä sillä
    
    - Hän löytää halutun numeron aika nopeasti mutta se on ilmaistu hexadecimaalina **0x86187**
    - Hän muuttaa sen pythonilla decimaaliluvuksi **549255**


### A.

Asennetaan **ghidra** kaliin komennolla `sudo apt-get install ghidra`

![image](https://github.com/user-attachments/assets/96b9a78f-ee44-4b95-b5de-ff508a26d7a8)

### B.

**packd** tiedosto on pakattu joten ennekun sitä voi tutkia **ghidralla** niin puretaan se komennolla `upx -d packd`

![image-1](https://github.com/user-attachments/assets/bc16d247-211e-49a3-9439-ea6d3eb888e4)

Kun ohjelma on purettu ja analysoitu ghidralla saamme tälläisen C käännöksen

![image-2](https://github.com/user-attachments/assets/fe802ae6-48d2-479e-b680-3b59d8b1a9fb)

Ohjelma toimii siten että käyttäjältä kysytään salasana jonka jälkeen **__iso00_scanf** skannaa sen ja tallentaa sen taulukkoon **local_28**

Tämän jälkeen käyttäjän antamaa salasanaa verrataan **piilos-AnAnAs** ja jos ne ovat samat niin **iVar1** muuttujaan tallennetaan arvo **0** jos ne ovat eri niin **iVar1** muutujaan tallenetaan, jokin **integer** arvo

Tämän jälkeen **if** lausekkeella katsotaan onko **iVar1 = 0** ja jos on niin käyttäjälle palautetaan **lippu** jos **iVar1 != 0** niin käyttäjälle palautetaan **Sorry, no bonus.**

![image-3](https://github.com/user-attachments/assets/f799fb2a-7b24-4a10-8793-d5b312a6d083)

Tässä muutin **iVar1** nimeksi **checker** koska sitä arvoa käytetään "chekkaamaan" onko salasana oikein muutin **local_28** nimeksi **password** koska sinne tallennetaan käyttäjän salasana

Muutin vielä **DAT_0010201d** nimeksi **%19s** koska löysin **assembly** koodista tämän kohdan

![image-5](https://github.com/user-attachments/assets/fe06d439-ab1a-4a46-b73b-ddf8806e5146)

### C.

**passtr** ohjelma toimii tismalleen samalla tavalla kuin **packd** ohjelma

![image-6](https://github.com/user-attachments/assets/bbee898f-5ee9-434f-8d25-1f2a85d76584)

Joten jos haluamme tehdä niin että lippu palautetaan kaikilla muilla salasanoilla kuin **sala-hakkeri-321** niin **"Yes! That\'s the password. FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}"** ja **"Sorry, no bonus."** pitäisi vaihtaa paikkaa

Kun katsomme **assembly** koodia sieltä löytyy tämmöinen kohta

![image-7](https://github.com/user-attachments/assets/42e794cb-6f5b-45d3-9865-cd6706d440c1)

**JNZ** ilmeisesti tarkoittaa "jump if not zero" ([lähde](https://stackoverflow.com/questions/14841169/jnz-cmp-assembly-instructions)) ja vihreästä nuolesta näkee, että jos se ei ole nolla niin se hyppää **"Sorry, no bonus."** kohtaan

Tästä voisi päätellä jos ottaa **N** kirjaimen pois niin ohjelma menisi tähän kohtaan jos se oisi nolla "jump if zero"

![image-8](https://github.com/user-attachments/assets/f0e96cdb-4da4-49aa-9ee1-e05421f3cb01)

Ja kuten C käännöksestä näkee niin nyt jos **iVar1** palauttaa **0** niin ohjelma tulostaa **"Sorry, no bonus."** ja muussa tapauksessa ohjelma tulostaa lipun

Testataan ohjelmaa vielä

Exportataan ohjelma

![image-9](https://github.com/user-attachments/assets/1c46a156-75b8-409f-a784-1cfd6ea0d40a)

Ja tehdään ohjelmasta executable komennolla `chmod +x passtr`

![image-10](https://github.com/user-attachments/assets/86cd655e-476d-4e16-b83c-ae7af1b36b9f)

![image-11](https://github.com/user-attachments/assets/b2e3d77c-59b2-4772-a240-1075f088aa65)

Ja testeistä näkyy että vain **sala-hakkeri-321** vastauksella ohjelma palautta **"Sorry, no bonus."** ja kaikilla muilla saadaan lippu 

### D.

Ladataan **crackmes** repositorio kaliin komennolla `git clone https://github.com/NoraCodes/crackmes.git`

![image-36](https://github.com/user-attachments/assets/3ad0a1dd-e98d-4a3d-a565-d186fafa1eac)

### E.

Kun katsotaan **ghidralla** ohjelmaa

![image-13](https://github.com/user-attachments/assets/0f15c719-044a-4a50-b7cd-4a3e40617039)

Nähdään että ohjelma vertaa käyttäjän syötettä tekstiin **password1**

Joten biinärin voi ratkaista syöttämällä tämän tekstiin

![image-14](https://github.com/user-attachments/assets/4d1637fa-4714-4aa6-af58-04156c111d3c)

### F.

seuraava ohjelma toimii tismalleen samalla tavalla kuin edellinen

![image-15](https://github.com/user-attachments/assets/caff5aac-b096-43ad-9eb0-165559971a26)

pitää vaan muistaa käyttää escape merkkiä `\!` koska **!** ei muuten toimii halutusti

![image-16](https://github.com/user-attachments/assets/0c99636b-33df-4b34-941c-9f2d20508c2c)

### G. JA I.

Tässä on alkuperäinen kuva ohjelmasta **ghidran** kääntämänä

![image-17](https://github.com/user-attachments/assets/0d6c3bd0-31aa-4e29-b163-557c3a1fac7b)

Päätin uudelleen nimetä **param_1** --> **argc** ja **param_2** --> **argv** koska **param_1**
laskee kuinka monta argumenttia ohjelmalle annetaan ja **param_2** tallentaa argumentit [Lähde](https://www.tutorialspoint.com/cprogramming/c_command_line_arguments.htm)

Sitten nimesin **uVar1** --> **return_value** koska siihen tallenetaan ohjelman palautus arvo

Ja nimesin vielä **local_c** --> **i** koska sitä käytetään indeksi arvona

![image-18](https://github.com/user-attachments/assets/761410c6-3d80-4518-95b2-7e6a7c60c24b)

Ohjelma toimii siten, että ensimmäinen **if** lauseke katsoo onko argumentteja kaksi ja jos on ohjelma jatkaa suorittamista, muuten ohjelma
palauttaa **Need exactly one argument.** ensimmäinen argumentti on aina ohjelman nimi, joten ohjelmaan voi laittaa vain yhden oman argumentin

![image-19](https://github.com/user-attachments/assets/8ecf57f4-d5a5-4f81-b2f1-44ed7224444c)

Sitten alkaa **for** looppi

Ensimmäisenä loopissaa **i** alustetaan nollalla

![image-20](https://github.com/user-attachments/assets/68e5b819-202d-4165-a49f-fffcc3251c37)

Sitten katsotaan, että merkkijonossa **password1** kohdassa **i** oleva arvo on erisuuri kuin **\0** tällä varmistetaan ettei merkkijono ole 
loppunut esim. jos **i = 9** niin arvo olisi silloin **\0** koska merkkijonossa ei ole niin montaa kirjainta

![image-21](https://github.com/user-attachments/assets/02613809-32c0-47f5-a0be-1249b93a452a)

Seuraavaksi **for** loopissa tarkistetaan, että annetussa argumentissa kohdassa **i** oleva arvo on erisuuri kuin **\0**

![image-22](https://github.com/user-attachments/assets/c1965765-8b64-4f25-a7ed-5813479a5555)

Tässä on jo ensimmäinen kohta miten binäärin voi ratkaista koska jos käyttäjä antaa argumentiksi tyhjän rivin niin ohjelma hyppää
samantien pois **for** loopista ja palauttaa arvon **0**

![image-23](https://github.com/user-attachments/assets/5eb6fab1-8f0d-4da8-9f84-a43a9c2a6d0f)

Mutta jos **for** looppi jatkuu niin **i** arvo nousee yhdellä joka kerta

![image-24](https://github.com/user-attachments/assets/eaee1944-260c-4628-aa7e-25888ea84cad)

Seuraavassa **if** lausekkeessa tarkastellaan, että **password1** kohdassa **i** olevan kirjaimen **ASCII** arvo, josta on vähennetty 1 on
erisuuri kuin käyttäjän antaman syötteen kohdassa **i**

![image-25](https://github.com/user-attachments/assets/d02179ca-b816-4fb1-a9ae-931328e84075)

Eli jos **i** on 0 niin siinä kohdassa oleva kirjain on **p**, jonka **ASCII** arvo on **112** ja tästä kun vähennetään 1 eli **112-1=111**,
joka vastaa kirjainta **o**

Ja jos haluttu **ASCII** arvo ja käyttäjän antaman kirjaimen **ASCII** arvo ovat eri niin ohjelma palautta 1

![image-26](https://github.com/user-attachments/assets/8128f3a7-5e57-4279-9740-133e82e20282)

Eli koska ohjelma hyppää pois for loopista heti kun käyttäjän antaman argumentin kirjaimet loppuvat riittää tähän siis pelkkä **o** kirjain,
jotta ohjelma palauttaa 0

![image-27](https://github.com/user-attachments/assets/d2e4cadb-4c0c-432b-8d7c-367e4fa79d5a)

Mutta jos haluaa kirjoittaa koko salasanan niin pitää vain muuttaa **password1** ASCII arvoiksi ja miinustaa jokaisesta 1 ja kirjoittaa se

- p --> 112 - 1 = 111 --> o 
- a --> 97 - 1 = 96 --> `
- s --> 115 - 1 = 114 --> r
- s --> 115 - 1 = 114 --> r
- w --> 119 - 1 = 118 --> v
- o --> 111 - 1 = 110 --> n
- r --> 114 - 1 = 113 --> q
- d --> 100 - 1 = 99 --> c
- 1 --> 49 - 1 = 48 --> 0

![image-28](https://github.com/user-attachments/assets/86c6ee8c-005e-4a50-ac24-123331c3a716)

### H.

En tiedä lasketaanko tämä, mutta koska ohjelma tarkistaa käyttäjän syötteestä vain ensimmäiset kahdeksan merkkiä

![image-29](https://github.com/user-attachments/assets/045f8c02-ebd3-493f-b290-5fa767b1fb99)

niin sillä ei ole mitään väliä, mitä **password1** perään laittaa ohjelma palauttaa silti 0

![image-30](https://github.com/user-attachments/assets/6128cca9-cac9-4f99-8a74-ff14c2248b90)

### J.

Tämä ohjelma toimii samalla tavalla kuin kohdassa G. mutta salasana jonka kirjaimet muutetaan on nyt **yuvmnpoi**, joka löytyy `strings` komennolla

![image-31](https://github.com/user-attachments/assets/ee3f7894-6619-44bd-bbe2-ae7f3cf6a6d9)

Ja tällä kertaa kirjainten **ASCII** arvosta vähennetään kaksi

![image-32](https://github.com/user-attachments/assets/5f313932-d39b-4614-bcad-67560241fb56)

eli

- y --> 121 - 2 = 119 --> w
- u --> 117 - 2 = 115 --> s
- v --> 118 - 2 = 116 --> t
- m --> 109 - 2 = 107 --> k
- n --> 110 - 2 = 108 --> l
- p --> 112 - 2 = 110 --> n
- o --> 111 - 2 = 109 --> m
- i --> 105 - 2 = 103 --> g

![image-33](https://github.com/user-attachments/assets/3822e8b1-3893-45f3-91d6-d41b17cb6d2a)

Mutta taas tässäkin pelkkä w käy tai mikä tahansa lyhennelmä tosta merkkijonosta ja myös tyhjä palauttaa tässäkin 0

![image-35](https://github.com/user-attachments/assets/81e753eb-f9ea-42a9-b58f-8c06518b4b70)

### Lähteet

ASCII taulukko https://www.asciitable.com/

