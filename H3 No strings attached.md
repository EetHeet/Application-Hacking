# H3 No strings attached

## [Tehtävät](https://terokarvinen.com/application-hacking/#h3-no-strings-attached)

#### a) Strings. Lataa ezbin-challenges.zip Aja 'passtr'. Selvitä oikea salasana 'strings' avulla. Selvitä myös lippu. (Ensisijaisesti katsomatta sorsia, jos osaat.)

#### b) Tee passtr.c -ohjelmasta uusi versio, jossa salasana ei näy suoraan sellaisenaan binääristä. Osoita testillä, että salasana ei näy. (Obfuskointi riittää.)

#### c) Packd. Aja 'packd' paketista ezbin-challenges.zip. Mikä on salasana? Mikä on lippu? (Tämä tehtävä on hieman haastavampi. Kirjaa ylös kokeilemasi lähestymistavat ja keksimäsi hypoteesit. Toivottavasti pääset itse maaliin, mutta jos et, läpikävely paljastuu tunnilla...)

#### d) Vapaaehtoinen bonus: Cryptopals. Crypto Challenge Set 1. Tätä voi tehdä useamman viikon bonuksena. Jos saat ratkaistua kohdat 1 .. "4. Detect single-character XOR", olet jo astunut salakirjoituksen maailmaan.

## Vastaukset

### A.

Kun ajamme **passtr** ohjelman se kysyy salasanaa

![image](https://github.com/user-attachments/assets/91058c07-b1c5-40be-a546-e9d30f7ea304)

Salasanaa emme tietenkää tiedä, joten voimme selvittää sen käyttämällä `strings` komentoa

![image-1](https://github.com/user-attachments/assets/e8829b22-4b84-4357-8c74-ff6f8375e423)

`strings` komento tekee sen että se etsii kaikki teksti muodossa olevat asiat tiedostosta ja tällä saimme salasanan **sala-hakkeri-321** selville

![image-2](https://github.com/user-attachments/assets/4daf3c1d-339a-4d24-96aa-d66e7aa4c875)

### B.

Päätin tehdä tämän siten, että koodaan salasanan **MD5** hashina ohjelmaan ja muutan käyttäjän antaman tekstin **MD5** hashiksi ja vertaan sitä ohjelmassa olevaan hashiin

Ensimmäiseksi piti muuttaa teksti **sala-hakkeri-321** MD5 hashiksi ja tässä käytin pencode ohjelmaa

Komennolla `echo sala-hakkeri-321 | pencode md5` saadaan hash

![image-3](https://github.com/user-attachments/assets/5f628978-f17a-4ce2-aae9-84e4bb67c287)

Sitten koodin pariin

Ensimmäiseksi lisätään **openssl/md5.h** kirjasto

![image-15](https://github.com/user-attachments/assets/c3bfb787-7dc1-46dd-a9e3-c4984924af24)

Sitten koodataan ohjelmaan **sala-hakkeri-321** hash johon käyttäjän syötettä verrataan

Tässä tein taulukon **correct_hash** ja **MD_DIGEST_LENGHT** tarkoittaa että taulukon bituus on **16 tavua** taulukossa jokainen hashin tavu on **heksadesimaalimuodossa**

![image-4](https://github.com/user-attachments/assets/2ccefef9-367b-40e1-a4ba-d1c3c3414bd4)

Sitten määritellään taulukko johon myöhemmin laitetaan käyttäjän syötteen hash ja annteaan tälle pituudeksi myös **16 tavua**

Sitten luodaan **MD5_CTX** struct ja annetaan sille nimeksi **password_hash**

**MD5_CTX** on **openssl/md5.h** kirjastossa määritelty **data structure**, johon tallenetaan **MD5** hashaukseen tarvittavat tiedot

![image-5](https://github.com/user-attachments/assets/5976354b-4fe0-4351-b5b5-49dbda2ef808)

Sitten tulee itse hashaus vaiheet näitä on kolme

![image-6](https://github.com/user-attachments/assets/3d1f642f-2a67-426d-91ca-7ba63b32c9c0)

**MD5_Init** tässä alustetaan **password_hash** ja nollataan kaikki kentät, jotta seuraavat vaiheet toimisivat oikein

**MD5_Update** tässä syötetään **password_hash** structiin käyttäjän antama salasana ja sen pituus

**MD5_Final** tässä hashaus proseesi viimeistellään ja lopullinen arvo tallennetaan **input_hash** taulukkoon

![image-7](https://github.com/user-attachments/assets/d4626cf0-0c44-40e1-8d48-5f205d90ad39)

Viimeisessä kohdassa verrataan tavu tavulta ovatko **input_hash** ja **correct_hash** tismalleen samanlaiset. Tälle annetaan annetaan vielä pituus, jotta ohjelma tietää kuinka monta tavua pitää verrata

Tässä vielä kuva koko koodista

![image-8](https://github.com/user-attachments/assets/9d453e52-1bbf-474c-8923-e267ba817f7e)

Nyt salasanaa ei näyt `strings` komennolla

![image-13](https://github.com/user-attachments/assets/b97c7b4e-9088-46c3-b334-8e5e1e1f4544)

Ja ohjelma toimii

![image-14](https://github.com/user-attachments/assets/43b62e5f-cbec-499b-9f5f-99bb8fb527b7)

**HUOMAUTUS**

C-kieli on minulle täysin vieras enkä ennen tätä tehtävää ole toiminut sen kanssa lainkaan. Tein tehtävän ChatGPT:n ja internetin avulla, mutta koitin selittää ja ymmärtää koodia parhaani mukaan. 
Jos selityksissä on virheitä tai muuta parannettavaa haluaisin mielelläni kuulla niistä. **Lähteet mainittu alhaalla**

### C.

Kun katsoo `strings` komennolla **packd** binääriä niin huomataan, että salasana ei olekkaan enää kunnolla näkyvissä siellä

![image-16](https://github.com/user-attachments/assets/5ee4d886-963e-4fbe-a326-d32bd27a780b)

Toinen kohta mikä kiinnitti huomion oli tämä

![image-9](https://github.com/user-attachments/assets/60b17abf-6ec0-4532-a5d9-86534ae9f787)

En tiennyt mikä tämä on joten googletin

Löysin tämän [Github sivun](https://github.com/upx/upx/blob/devel/doc/upx.pod), jossa neuvotaan miten pakattu ohjelma voidaan purkaa, joten kokeilin sitä

![image-10](https://github.com/user-attachments/assets/cde2ff82-a16e-4a97-bfe3-4272fe53f591)

ja nyt kun kokeillaan `strings` komentoa uudestaan niin saadaan tämä

![image-11](https://github.com/user-attachments/assets/41d1d89c-2f24-4bf9-a12a-76fdfacaca48)

Salasana ja lippu ovat näkyvissä

![image-12](https://github.com/user-attachments/assets/08eea0a4-bc54-44d0-838d-6c2af7396814)

Salasanaa ei siis aluksi näy `strings` komennolla, koska kun binääri on pakattu niin esim. **AnAnAs** sanasta poistetaan toistuvuus ja se encodataan siten, että tietokone ymmärtää, mutta ihmisen on vaikea lukea sitä.

### D.

Teen oman repositorion tälle ja linkitän sen tähän tehtävään.

### Lähteet

**Tehtävä B**

- https://stackoverflow.com/questions/7627723/how-to-create-a-md5-hash-of-a-string-in-c
- https://docs.huihoo.com/doxygen/openssl/1.0.1c/crypto_2md5_2md5_8h.html
- https://processhacker.sourceforge.io/doc/phlib_2include_2md5_8h_source.html

**Tehtävä C**

- https://github.com/upx/upx/blob/devel/doc/upx.pod
- https://en.wikipedia.org/wiki/Data_compression

