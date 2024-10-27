# H1 Korkeat Standardit

## [Tehtävät](https://terokarvinen.com/application-hacking/#h1-korkeat-standardit)

#### a) Tutustu kurssin sanastoon, joka on määritelty SFS-EN ISO/IEC 27000:2020:en standardissa, kappaleessa 3. Terms and Definitions. Selvitä seuraavien kappaleiden määritteet ja selitä omin sanoin mitä ne tarkoittavat: 3.2, 3.31, 3.56, 3.58, 3.77. (Ei edellytä teknisiä testejä tietokoneella)

#### b) Tutustu standardiin ISO 27034-1 - 5. Selvitä mistä standardi kokonaisuudesta on kyse. (Ei edellytä teknisiä testejä tietokoneella)

#### c) Kuuntele podcast: Meurman 2021: Laatulöpinät 30: Tietoturvallisuus ohjelmistokehityksessä (jakson mp3, Laatolöpinät RSS-syöte). Mitä mieltä olet podcastin väittämistä? (Ei edellytä teknisiä testejä tietokoneella)

#### d) Asenna Debian 12-Bookworm virtuaalikoneeseen. Päivitä kaikki ohjelmat. (Poikkeuksellisesti tätä alakohtaa ei tarvitse raportoida, paitsi jos jokin ei toimi. Ympäristö tarvitaan seuraavalle oppitunnille. Joten tämän kohdan vastaukseksi riittää kuittaus, että Linux on asennettu.)

## Vastaukset

### A.

#### **3.2 Attack**

Tarkoittaa kohteen luvatonta käyttöä, manipulointia, tuhoamista, varastamista jne.

#### **3.31 information security incident**

Tarkoittaa yllättävää tapahtumaa tai tapahtumia, joilla on iso mahdolisuus, että tiedon eheys, saatavuus ja luottamuksellisuus, sekä liiketoiminta on vaarantunut.

#### **3.56 Requirement**

Tarkoittaa tarvetta tai oletusta, joka on määritelty, pakollinen tai on muuten tavanomaista organisaatiossa.

#### **3.58 Review**

Tarkoittaa aktiviitettia, jossa selvitetään onko käsiteltävä asia hyödyllinen, tehokas ja riittävä haluttujen tavotteiden saavuttamisessa.

#### **3.77 Vulnerability**

Tarkoittaa kohteesa olevaa heikkoutta, jota voidaan hyväksikäyttää.

### B.

Standardi kokonaisuudessa on kyse auttaa yritystä hallitsemaan sovelluksen turvallisuutta koko sovelluksen elinkaaren ajan. 

Osa 1 esittelee tärkeimpiä käsitteitä ja antaa yleiskuvan ohjelmisto turvallisuudesta.

Osa 2 auttaa yritystä kehittämään standardin muikaiset käytännöt ja ottaa ne käyttöön omissa prosesseissaan.

Osa 3 auttaa yritystä kehittämään standardin muikaiset käytännöt ja ottaa ne käyttöön omissa prosesseissaan ohjelmisto projektin tasolla.

Osa 4 kertoo kuinka yritys voi jatkuvasti validoida ohjelmiston turvallisuutta.

Osa 5 kertoo kuinka ohjelmiston turvallisuutta voidaan toteuttaa käytännössä.

### C.

#### **Mikään ohjelmisto ei ole täysin tietoturvallinen.**

Olen samaa mieltä väittämän kanssa, koska sitä mukaan kun tietoturva kehittyy niin kehittyy myös tavat miten sitä murretaan,
joten se on ikuista kisaa siitä kumpi puoli on voitolla.

#### **Hallinnollinen tietoturva on teknisen tietoturvan onnistumisen edellytys.**

Olen samaa mieltä, koska vaikka olisi kuinka hyvät tekniset tietoturva systeemit, mutta ihmiset käyttävät niitä väärin
tai eivät noudata sopivia protokollia niin ei niistä systeemeistä ole mitään hyötyä. Tietokoneet eivät tee virheitä, mutta ihmiset
tekevät.

#### **Automaatiotestaus on ohjelmiston tietoturvan kannalta erittäin tärkeää.**

Olen jokseenkin samaa mieltä, mielestäni automaatiotestausta voisi käyttään ns. vähemän kriittisissä systeemeissä, jotta vähennetään
työmäärää, mutta koska nekin on ihmisten tekemiä ja ihmiset tekee virheitä, niin ne eivät ole mielestäni kovin luotettava testauskeino
tietoturvan kannalta.

#### **Ohjelmistoa suunniteltaessa voidaan tehdä paljonkin auttamaan käyttäjää toimimaan tietoturvallisesti. Usein nämä toimenpiteet kuitenkin vaikuttavat negatiivisesti käytettävyyteen.**

Olen samaa mieltä, koska tottakai käytettävyyden kannalta olisi helpompi jos esimerkiksi johonkin ohjelmaan voisi vaan laittaa oman nimen ja kirjatua sillä, mutta se 
ei olisi kovin turvallista, mutta kun aletaan antamaan ohjeita miten tehdään esim. monimutkainen salasana ja siihen viel toisenasteen tunnistus, niin se vaikuttaa 
huomattavan negatiivisesti käytettävyyteen.

#### **Ohjelmiston tietoturvallisuuden suunnitteluun vaikuttaa paljolti se, kuinka arkaluonteisia tietoja ohjelmistolla on tarkoitus käsitellä.**

Olen samaa mieltä, koska on paljon todennäkösempää, että esim. jonkin valtion tai ison yrityksen tietoturvaa koitetaan murtaa, kuin se että joku koittaisi murtautua
jonkun kalenteri ohjelmaan tms.

#### **Ohjelmistokehittäjät näkevät omat ohjelmistonsa aina merkittävästi riskialttiimpina, kuin muiden tekemät ohjelmistot.**

Mielestäni tämä riippuu ohjelmistokehittäjästä, jos on vaikka hirveän ylimielinen ohjelmistokehittäjä, joka ajattelee että hän ei tee ikinä mitään väärin ja kaikki 
hänen ohjelmansa ovat täydellisiä niin ei hän varmaan nää omaa ohelmaansa yhtään riskialttiina, mutta sitten taas jos on tosi itsekriittinen ja epävarma ohjelmistokehittäjä
niin hän voi nähdä oman ohjelmansa hyvinki riskialttiina vaikkei se sitä välttämättä olisi.

### D.

Linux on asennettu

### Lähteet

Podcasti ja väittämät: https://www.arter.fi/podcast/laatulopinat-podcast-tietoturvallisuus-ohjelmistokehityksessa-tarkastele-kokonaisuutta-ja-hyodynna-viitekehykset/

Application Security Management with ISO/IEC 27034 https://pecb.com/article/application-security-management-with-isoiec-27034
