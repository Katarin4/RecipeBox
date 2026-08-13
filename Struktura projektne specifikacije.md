# Projektna specifikacija – Digitalna knjiga recepata

## 1\. Opis projekta

Digitalna knjiga recepata je web aplikacija namenjena korisnicima koji žele da svoje recepte čuvaju, organizuju, uređuju i dele na jednom mestu. Osnovna ideja projekta nije da odmah predstavlja javnu bazu recepata, već da svaki korisnik ima svoju ličnu digitalnu knjigu recepata.

Korisnik nakon registracije i prijave može da kreira sopstvene recepte, dodaje fotografije, sastojke i korake pripreme, određuje vreme pripreme, težinu i broj porcija, kao i da receptu dodeli više tagova. Recept nema posebnu obaveznu kategoriju: karakteristike kao što su `posno`, `doručak`, `riba`, `brzo`, `slatko` i `slano` predstavljaju tagove. Jedan recept zato može istovremeno odgovarati većem broju kriterijuma pretrage.

Korisnik može da sačuva recepte koji su mu dostupni, da ih organizuje u sopstvene kolekcije, da prati druge korisnike i da deli svoje recepte. Dostupni recepti mogu biti ocenjeni i komentarisani.

Aplikacija će imati i AI funkcionalnosti. AI će služiti kao pomoć korisniku pri izboru hrane, na primer za predlog priloga uz postojeće jelo, predlog recepta kada korisnik ne zna šta da sprema i pronalaženje sličnih recepata.

Posebna funkcionalnost biće izvoz recepta u PDF format. Korisnik će moći da od svog recepta generiše dokument pogodan za čuvanje, štampanje ili deljenje.

Projekat će se razvijati postepeno. MVP će se fokusirati na korisničke naloge, kreiranje i upravljanje sopstvenim receptima, tagove, pretragu, čuvanje, praćenje korisnika, deljenje i PDF export. Naprednije AI funkcionalnosti i javna pretraga svih dostupnih recepata mogu se dodavati u kasnijim verzijama.

\---

## 2\. Ciljevi projekta

Glavni cilj je razvoj funkcionalne i pregledne web aplikacije koja predstavlja ličnu digitalnu knjigu recepata.

Specifični ciljevi su:

* omogućiti korisniku da napravi nalog i bezbedno se prijavi;
* omogućiti dodavanje, pregled, izmenu i brisanje sopstvenih recepata;
* omogućiti čuvanje svih relevantnih informacija o receptu;
* omogućiti dodavanje slike recepta;
* omogućiti veliki broj tagova po receptu, bez posebne kategorije recepta;
* omogućiti kombinovanu pretragu po više tagova;
* omogućiti korisniku da organizuje sačuvane recepte pomoću sopstvenih kolekcija;
* omogućiti praćenje drugih korisnika;
* omogućiti kontrolisano deljenje recepata;
* omogućiti ocenjivanje i komentarisanje recepata koji su dostupni korisniku;
* omogućiti generisanje PDF verzije recepta;
* integrisati AI servis preko backend-a;
* napraviti jasnu podelu odgovornosti između React frontend-a, Quarkus backend-a i MongoDB baze;
* napraviti arhitekturu koja može da se proširuje novim funkcionalnostima bez velikih promena postojećeg sistema.

\---

## 3\. Korisnici i korisničke uloge

U MVP verziji postoji samo jedna korisnička uloga: **User**.

Ne postoji administratorska uloga i ne postoji poseban gostujući režim. Korisnik mora da se registruje i prijavi kako bi koristio aplikaciju.

Svi korisnici imaju iste osnovne mogućnosti. Razlika između korisnika nastaje na osnovu toga da li je određeni recept njihov, da li je recept podeljen sa njima i koga prate.

Korisnik može da:

* kreira svoj nalog;
* uređuje svoj profil;
* kreira recepte;
* uređuje i briše svoje recepte;
* pregleda svoje recepte;
* pretražuje svoje recepte;
* pravi kolekcije;
* čuva recepte kojima ima pristup;
* prati druge korisnike;
* deli svoje recepte;
* pregleda recepte koji su mu dostupni;
* ocenjuje i komentariše dostupne recepte;
* koristi AI funkcionalnosti;
* generiše PDF verziju recepta.

Prava pristupa će se proveravati na backend-u. Frontend ne sme biti jedina zaštita, jer korisnik može direktno poslati HTTP zahtev backend-u.

\---

## 4\. Kompletan spisak funkcionalnosti

### 4.1. Registracija i prijava

Korisnik može da kreira nalog unosom osnovnih podataka, kao što su korisničko ime, email adresa i lozinka.

Nakon registracije korisnik može da se prijavi. Backend proverava podatke za prijavu i kreira autentifikacioni rezultat koji frontend koristi za naredne zahteve.

Korisnik može da se odjavi. Lozinke se ne čuvaju u MongoDB-u. Njima upravlja Keycloak.

### 4.2. Korisnički profil

Profil sadrži osnovne informacije o korisniku i društvene podatke, na primer:

* korisničko ime;
* prikazano ime;
* profilnu sliku;
* opis profila;
* broj recepata;
* broj pratilaca;
* broj korisnika koje prati.

Vlasnik profila ima akciju `Edit profile`, kojom menja prikazano ime, korisničko ime, biografiju i profilnu sliku. Na tuđem profilu prikazuje se akcija `Follow` ili `Unfollow`, umesto izmene profila. Profil može prikazivati samo recepte koji su dostupni posmatraču.

### 4.3. Dodavanje recepta

Korisnik otvara stranicu za dodavanje recepta i popunjava formu.

Recept sadrži najmanje:

* naziv;
* opis;
* fotografiju;
* vreme pripreme;
* vreme kuvanja/pečenja ako bude potrebno;
* ukupno vreme;
* težinu;
* broj početnih porcija;
* sastojke;
* korake pripreme;
* tagove;
* autora;
* vidljivost.

Korisnik može da doda proizvoljan broj sastojaka i koraka pripreme.

### 4.4. Izmena i brisanje

Autor može da izmeni svoj recept ili da ga obriše.

Backend mora proveriti da je trenutno prijavljeni korisnik zaista autor recepta pre nego što dozvoli izmenu ili brisanje.

### 4.5. Pregled recepta

Stranica recepta prikazuje sve informacije o receptu, uključujući fotografiju, naziv, autora, vreme pripreme, težinu, broj porcija, sastojke, način pripreme, tagove, ocenu i komentare.

Na ovoj stranici korisnik može imati opcije za čuvanje, deljenje, generisanje PDF-a i korišćenje AI preporuka.

### 4.6. Promena broja porcija

Recept ima početni broj porcija. Korisnik može promeniti broj porcija, nakon čega frontend preračunava količine sastojaka.

Na primer, recept za 4 osobe može se promeniti na 2 osobe. Ako je potrebno 300 g brašna za 4 osobe, prikazaće se 150 g za 2 osobe. Originalne vrednosti u bazi ostaju nepromenjene.

### 4.7. Pretraga

U MVP-u pretraga se prvenstveno odnosi na recepte kojima korisnik ima pristup. Korisnik sa jedne centralne Search stranice može da pretražuje naziv, opis i sastojke, a zatim rezultate sužava pomoću tagova i osnovnih filtera.

Posebno pravilo je kombinovanje više tagova: izbor `posno` i `doručak` znači da recept mora imati oba taga. Filteri nisu zamena za kategoriju, već rade zajedno sa tekstualnom pretragom.

Rezultati se prikazuju kao kartice sa osnovnim podacima, slikom, tagovima i prosečnom ocenom. Interfejs treba da prikazuje broj rezultata, aktivne filtere, opciju za uklanjanje pojedinačnog filtera i `Clear all`. U prototipu se učitavanje može simulirati, dok će prava aplikacija kasnije koristiti backend pretragu i paginaciju.

Kasnije se može dodati javna pretraga svih recepata koje su korisnici označili kao javne.

### 4.8. Filteri

Korisnik može da filtrira recepte prema:

* jednom ili više tagova;
* vremenu pripreme;
* težini;
* autoru kada ima pristup receptima drugih korisnika;
* sačuvanim receptima.

Filteri mogu da se kombinuju.

### 4.9. Čuvanje recepata

Korisnik može da sačuva recept koji mu je dostupan. Sačuvani recepti se prikazuju na posebnoj stranici.

Sačuvani recept ne znači da korisnik postaje njegov autor. Originalni autor ostaje vlasnik recepta.

### 4.10. Kolekcije

Kolekcije su korisničke liste recepata i razlikuju se od tagova.

Tag opisuje recept, dok kolekcija predstavlja način na koji korisnik želi da grupiše recepte.

Primer:

Recept „Riblja pašteta“ može imati tagove `posno`, `doručak`, `riba`, `brzo`.

Istovremeno može biti dodat u kolekcije `Za probati` i `Doručak za posao`.

Jedan recept može pripadati većem broju kolekcija.

### 4.11. Praćenje korisnika

Korisnik može da prati drugog korisnika.

Na profilu se prikazuju pratioci i korisnici koje profil prati.

U kasnijoj verziji mogu se dodati zahtevi za prijateljstvo, ali MVP koristi jednostavniji sistem praćenja.

### 4.12. Deljenje

Autor može da promeni vidljivost recepta i tako odredi ko može da mu pristupi. Vidljivost i funkcija deljenja nisu ista stvar: vidljivost određuje prava pristupa, dok funkcija Share služi za deljenje ili kopiranje linka do recepta.

Predviđene vrednosti vidljivosti su:

* `PRIVATE` – recept vidi samo autor;
* `FOLLOWERS` – recept mogu da vide korisnici koji prate autora;
* `PUBLIC` – recept mogu da vide svi prijavljeni korisnici, kada javna baza bude uvedena.

U MVP-u osnovni društveni model koristi `PRIVATE` i `FOLLOWERS`, dok je `PUBLIC` pripremljen kao buduća mogućnost. Prilikom prikaza bilo kog recepta backend mora proveriti pravo pristupa; frontend samo prilagođava prikaz dostupnim akcijama.

### 4.13. Ocene

Korisnik koji ima pristup receptu može da ostavi ocenu od 1 do 5 zvezdica.

Backend sprečava da isti korisnik napravi više nezavisnih ocena za isti recept. Postojeća ocena može biti izmenjena.

### 4.14. Komentari

Korisnik koji ima pristup receptu može ostaviti komentar. Komentar sadrži autora, tekst, vreme kreiranja i identifikator recepta.

Autor recepta može kasnije imati mogućnost da obriše komentar na svom receptu, dok se detaljna pravila moderacije mogu dodati u kasnijoj verziji.

### 4.15. PDF export

Korisnik može da generiše PDF dokument koji sadrži:

* naziv recepta;
* fotografiju;
* autora;
* opis;
* vreme pripreme;
* težinu;
* broj porcija;
* sastojke;
* korake pripreme;
* tagove.

PDF je namenjen čuvanju, štampanju ili deljenju i mora izgledati kao samostalan recept, a ne kao screenshot web stranice. U kasnijoj verziji ista funkcionalnost može biti proširena na izvoz cele kolekcije kao personalizovane digitalne kuvarice.

\---

## 5\. MVP funkcionalnosti

MVP obuhvata:

1. registraciju;
2. login i logout;
3. korisnički profil;
4. dodavanje recepta;
5. pregled recepta;
6. izmenu recepta;
7. brisanje recepta;
8. upload slike;
9. sastojke i korake pripreme;
10. vreme, težinu i broj porcija;
11. tagove bez posebne kategorije;
12. pretragu i filtere;
13. kombinovanje više tagova u filterima;
14. sačuvane recepte;
15. kolekcije;
16. praćenje korisnika;
17. osnovno deljenje recepata;
18. ocene;
19. komentare;
20. PDF export.

AI se može implementirati u završnom delu MVP-a ili neposredno nakon osnovnog MVP-a, jer ne treba da blokira razvoj osnovnog sistema.

\---

## 6\. Buduće funkcionalnosti

Nakon završetka MVP-a mogu se dodavati:

* javna baza recepata;
* globalna pretraga;
* Discover stranica;
* napredniji sistem preporuka;
* AI preporuke na osnovu sastojaka;
* AI planiranje obroka;
* AI predlog priloga;
* AI generisanje varijacija postojećeg recepta;
* javni profili;
* prijateljstva umesto jednostavnog praćenja;
* deljenje putem javnog linka;
* naprednija podešavanja privatnosti;
* mobilna aplikacija;
* notifikacije;
* moderacija sadržaja;
* administratorska uloga.

\---

## 7\. Detaljne user stories

### US-01 – Registracija

Kao novi korisnik, želim da napravim nalog kako bih mogao da koristim aplikaciju i čuvam svoje recepte.

Kriterijumi prihvatanja:

* korisnik unosi validne podatke;
* email mora biti jedinstven;
* lozinka se bezbedno hash-uje;
* nakon uspešne registracije korisnik može da se prijavi.

### US-02 – Login

Kao registrovani korisnik, želim da se prijavim kako bih pristupio svom sadržaju.

Kriterijumi:

* korisnik unosi email i lozinku;
* Keycloak proverava podatke i izdaje token;
* React dobija autentifikacioni token;
* neuspešna prijava vraća odgovarajuću grešku.

### US-03 – Dodavanje recepta

Kao korisnik, želim da dodam recept kako bih sačuvao svoj recept u digitalnoj knjizi.

### US-04 – Izmena recepta

Kao autor recepta, želim da izmenim svoj recept kada želim da ispravim sastojak, postupak ili druge podatke.

### US-05 – Brisanje recepta

Kao autor recepta, želim da obrišem recept koji mi više nije potreban.

### US-06 – Tagovanje

Kao korisnik, želim da receptu dodam više tagova kako bih kasnije mogao da ga pronađem kroz različite kriterijume.

### US-07 – Kombinovana pretraga

Kao korisnik, želim da mogu da izaberem više tagova istovremeno kako bih dobio samo recepte koji odgovaraju svim izabranim kriterijumima.

Primer: `posno + doručak` vraća recepte koji imaju oba taga.

### US-08 – Čuvanje recepta

Kao korisnik, želim da sačuvam recept koji mi se dopada kako bih mu kasnije lako pristupio.

### US-09 – Kolekcija

Kao korisnik, želim da napravim kolekciju kako bih organizovao sačuvane recepte prema sopstvenim potrebama.

### US-10 – Praćenje

Kao korisnik, želim da pratim drugog korisnika kako bih mogao da pronađem njegove dostupne recepte.

### US-11 – Deljenje

Kao autor, želim da podelim recept sa drugim korisnicima kako bi mogli da ga vide.

### US-12 – Ocenjivanje

Kao korisnik koji ima pristup receptu, želim da ostavim ocenu kako bih izrazio svoje mišljenje o receptu.

### US-13 – Komentarisanje

Kao korisnik koji ima pristup receptu, želim da ostavim komentar kako bih podelio utisak ili savet.

### US-14 – PDF export

Kao korisnik, želim da izvezem recept u PDF kako bih ga mogao sačuvati, odštampati ili podeliti.

### US-15 – AI preporuka

Kao korisnik, želim da dobijem AI predlog kada ne znam šta da spremim kako bih dobio inspiraciju na osnovu svojih recepata i kriterijuma.

\---

### US-16 – Vođeni izbor recepta

Kao korisnik koji ne zna šta da sprema, želim da kroz nekoliko kratkih pitanja navedem vreme i ukus/preference kako bih dobio konkretne predloge iz svoje knjige.

Kriterijumi prihvatanja:

* početak je sa početne stranice;
* korisnik bira vreme ili opciju bez ograničenja;
* korisnik bira slatko, slano ili svejedno;
* korisnik može dodati preference;
* rezultat prikazuje recepte koji odgovaraju kriterijumima;
* ako nema rezultata, korisnik može promeniti kriterijume.

### US-17 – Upravljanje slikom recepta

Kao korisnik, želim da dodam fotografiju receptu i vidim preview pre čuvanja kako bih znao kako će recept izgledati.

Kriterijumi prihvatanja:

* korisnik može izabrati sliku;
* preview se prikazuje odmah;
* korisnik može promeniti ili ukloniti sliku;
* interfejs prikazuje jasnu grešku ako format ili veličina nisu prihvatljivi.

### US-18 – Uklanjanje recepta iz kolekcije

Kao korisnik, želim da uklonim recept iz kolekcije bez brisanja samog recepta.

### US-19 – Upravljanje profilom

Kao korisnik, želim da izmenim svoje osnovne profilne podatke kako bih održavao profil ažurnim.

### US-20 – Deljenje recepta

Kao korisnik, želim da kopiram ili podelim link recepta, dok autor zasebno kontroliše njegovu vidljivost.

### US-21 – Slični recepti i predlozi priloga

Kao korisnik, želim da na stranici recepta vidim slične recepte i predloge priloga kako bih lakše pronašao šta još mogu da spremim uz postojeće jelo.

## 8\. User flow

Osnovni tok korisnika je:

`Registracija → Login → Početna → Moji recepti → Dodaj recept → Sačuvaj → Pregled recepta`

Sa početne stranice korisnik može da ode na:

* pretragu;
* filtriranje po tagovima;
* sačuvane recepte;
* kolekcije;
* profile drugih korisnika;
* AI preporuke;
* svoj profil;
* dodavanje recepta.

Tok dodavanja recepta:

`Dodaj recept → Popuni osnovne podatke → Dodaj sastojke → Dodaj korake → Dodaj tagove → Odredi vidljivost → Sačuvaj`

Tok deljenja:

`Moji recepti → Otvori recept → Podeli → Odredi dostupnost → Potvrdi`

Tok PDF exporta:

`Recept → Izvezi PDF → Backend priprema dokument → Browser dobija PDF → Korisnik čuva/štampa/deli`

\---

## 9\. Mapa stranica

### /login

Stranica za prijavu.

### /register

Stranica za registraciju.

### /

Početna stranica sa centralnim pitanjem **„Šta pravimo danas?“**.

Početna stranica može ponuditi:

* Slatko;
* Slano;
* Dodaj recept;
* Ne znam, daj mi predloge.

Na vrhu se nalazi search bar, a sa strane glavna navigacija.

### /recipes

Moji recepti.

### /recipes/new

Forma za dodavanje recepta.

### /recipes/:id

Detaljan prikaz recepta.

### /recipes/:id/edit

Forma za izmenu recepta.

### /saved

Sačuvani recepti.

### /collections

Lista korisničkih kolekcija.

### /collections/:id

Sadržaj određene kolekcije.

### /users/:id

Profil drugog korisnika.

### /followers

Pratioci korisnika.

### /following

Korisnici koje trenutni korisnik prati.

### /profile

Profil trenutno prijavljenog korisnika.

### /ai

AI preporuke i pomoćnik.

### /settings

Podešavanja naloga.

\---

## 10\. Struktura recepta

Recept je jedan od centralnih entiteta sistema.

Logički model recepta:

```text
Recipe
├── id
├── title
├── description
├── image
├── authorId
├── preparationTime
├── cookingTime
├── totalTime
├── difficulty
├── servings
├── ingredients\\\[]
├── preparationSteps\\\[]
├── tags\\\[]
├── visibility
├── createdAt
└── updatedAt
```

Sastojak:

```text
Ingredient
├── quantity
├── unit
├── name
└── note (optional)
```

Korak pripreme:

```text
PreparationStep
├── order
└── description
```

Ocene i komentari ne moraju biti veliki ugnežđeni nizovi unutar recepta. Mogu biti zasebne MongoDB kolekcije povezane pomoću `recipeId`, što olakšava rad sa većim brojem komentara i ocena.

\---

## 11\. Tag sistem

Tagovi predstavljaju jednu od ključnih funkcionalnosti aplikacije.

Tag nije isto što i folder ili kolekcija.

Tag predstavlja osobinu recepta.

Primer:

`Riblja pašteta`

Tagovi:

`posno`, `doručak`, `riba`, `brzo`, `namaz`

Korisnik može pretraživati samo jedan tag, na primer `posno`, ili više tagova, na primer `posno + doručak`.

Kod kombinacije više tagova koristi se AND logika, odnosno recept mora imati sve izabrane tagove.

U početnoj verziji tagovi mogu biti string vrednosti. Kasnije se može napraviti posebna `tags` kolekcija sa normalizovanim nazivima kako bi se izbeglo stvaranje različitih verzija istog taga.

\---

## 12\. Sistem praćenja i deljenja

Praćenje i deljenje su povezane, ali različite funkcionalnosti.

Ako korisnik A prati korisnika B, to ne mora automatski značiti da korisnik A ima pristup svakom receptu korisnika B. Vidljivost recepta je zasebna odluka autora.

Primer:

Korisnik B ima recept `Riblja pašteta` sa vidljivošću `SHARED`. Korisnik A prati B i može da vidi recept.

Drugi recept korisnika B ima vidljivost `PRIVATE` i korisnik A ga ne može videti.

Backend pri svakom zahtevu proverava:

1. ko je prijavljen;
2. koji recept pokušava da otvori;
3. ko je autor;
4. kakva je vidljivost recepta;
5. da li trenutni korisnik ima pravo pristupa.

\---

## 13\. AI funkcionalnosti

AI se ne poziva direktno iz React aplikacije. React šalje zahtev Quarkus backend-u, a backend komunicira sa AI provajderom.

Arhitektura:

`React → Quarkus → AI Service → Gemini/Claude → Quarkus → React`

API ključ se čuva isključivo na backend-u.

AI servis treba da bude izdvojen kao posebna backend komponenta, na primer `AiService`. Na taj način ostatak aplikacije ne mora da zna da li se koristi Gemini ili Claude.

Moguća apstrakcija:

```text
AiService
    ↓
AiProvider
    ↓
GeminiProvider ili ClaudeProvider
```

### Predlog priloga

Na stranici recepta korisnik bira „Predloži prilog“. React šalje identifikator recepta backend-u. Backend učitava relevantne podatke, formira prompt i poziva AI. Rezultat se vraća React-u.

### „Ne znam, daj mi predloge“

Korisnik može navesti da li želi slatko ili slano, koliko vremena ima, koje sastojke ima i druge kriterijume. Backend može prvo pronaći relevantne recepte iz korisnikove baze, a AI zatim može pomoći pri rangiranju i objašnjenju preporuke.

### Slični recepti

Prva verzija može koristiti tagove i druge strukturisane podatke za pronalaženje sličnih recepata. Kasnije se može dodati AI semantička sličnost.

\---

## 14\. PDF export

Korisnik bira **„Izvezi kao PDF“** na stranici recepta.

React šalje zahtev Quarkus backend-u, na primer:

`GET /api/recipes/{id}/pdf`

Backend proverava da korisnik ima pravo pristupa receptu. Ako ima pravo, učitava podatke recepta iz MongoDB-a i generiše PDF.

PDF se vraća kao HTTP odgovor sa odgovarajućim `Content-Type` zaglavljem. Browser omogućava korisniku da dokument sačuva ili otvori za štampanje.

PDF treba da bude dizajniran kao samostalan recept, a ne kao screenshot web stranice.

\---

## 15\. Detaljna specifikacija korisničkog interfejsa i ponašanja prototipa

Ovo poglavlje definiše kako funkcionalnosti treba da izgledaju i kako korisnik treba da ih koristi iz ugla frontend-a. Ono predstavlja detaljniju specifikaciju prototipa i služi kao veza između ideje proizvoda i kasnije React implementacije. Pravila koja zahtevaju bazu, autentifikaciju, autorizaciju, AI servis ili serverski generisan PDF u ovom poglavlju se samo predstavljaju kroz korisničko ponašanje; stvarna implementacija tih pravila pripada backend-u i servisima opisanim u kasnijim poglavljima.

### 15.1. Početna stranica – „Šta pravimo danas?"

Početna stranica treba da bude polazna tačka za najčešće korisničke namere, a ne samo pregled navigacije. Centralna poruka treba da bude kratka i jasna, na primer **„Šta pravimo danas?“**. Ispod nje korisnik dobija nekoliko direktnih akcija:

* **Pretraži recepte** – vodi na pretragu i omogućava tekstualno traženje recepata;
* **Nešto slatko** – prikazuje recepte sa tagom `slatko`;
* **Nešto slano** – prikazuje recepte sa tagom `slano`;
* **+ Dodaj recept** – otvara formu za unos novog recepta;
* **Ne znam, daj mi predloge** – otvara vođeni proces za izbor recepta.

Poenta početne stranice je da korisnik ne mora prvo da razmišlja koju funkciju aplikacije treba da otvori. Umesto toga, aplikacija kreće od pitanja koje korisnik prirodno ima: šta želi da sprema.

Na desktopu se glavna navigacija može nalaziti u bočnom meniju, dok se na manjim ekranima pretvara u hamburger ili donju navigaciju. Search bar treba da bude lako uočljiv i dostupan sa početne stranice.

### 15.2. Navigacija

Glavne destinacije aplikacije su:

* Home;
* My Recipes;
* Search / Discover;
* Saved;
* Collections;
* AI suggestions;
* Profile.

`+ Add recipe` treba da bude posebno istaknuta akcija jer je kreiranje sopstvenih recepata centralna funkcija proizvoda. Na mobilnom prikazu može biti centralno dugme u donjoj navigaciji.

Navigacija mora jasno pokazati gde se korisnik trenutno nalazi. Povratak sa detalja recepta, kolekcije ili profila treba da bude intuitivan i bez gubitka trenutnog stanja pretrage kada je to moguće.

### 15.3. Recept kao centralni objekat interfejsa

Recept treba da ima konzistentan prikaz bez obzira da li se nalazi u `My Recipes`, `Saved`, kolekciji ili rezultatima pretrage. Recipe Card treba da sadrži najmanje fotografiju, naziv, autora kada recept nije korisnikov, osnovno vreme, broj porcija ili drugi koristan sažetak, tagove i prosečnu ocenu kada postoji.

Klik na karticu otvara Recipe Detail stranicu. Akcije na kartici ne treba da budu prenatrpane; glavne akcije ostaju na detaljnoj stranici.

### 15.4. Dodavanje recepta

Forma za dodavanje recepta treba da bude podeljena na logične celine kako korisnik ne bi dobio jedan veliki zid polja.

**Osnovni podaci:**

* naziv recepta;
* kratak opis;
* fotografija;
* vreme pripreme;
* vreme kuvanja/pečenja;
* broj početnih porcija;
* težina, ukoliko ostane deo specifikacije.

Ukupno vreme ne treba da korisnik ručno računa kada je jednako zbiru vremena pripreme i kuvanja; frontend može da ga prikaže kao izvedenu vrednost.

**Fotografija:** korisnik bira sliku, dobija preview i može da je promeni ili ukloni. U prototipu se upload može simulirati lokalnim preview-em; trajno čuvanje slike biće rešeno kasnije.

**Sastojci:** svaki sastojak je odvojen red koji sadrži količinu, jedinicu, naziv i opcionu napomenu. Korisnik može dodavati i brisati redove. Primer napomene je `prosejano` uz brašno ili `umućena` uz jaja.

**Koraci:** korisnik dodaje tekstualne korake bez ručnog upisivanja brojeva. UI automatski prikazuje 1, 2, 3... i omogućava dodavanje, brisanje i izmenu koraka. Ako je lako izvodljivo u prototipu, treba omogućiti i promenu redosleda.

**Tagovi:** korisnik može izabrati postojeće predložene tagove ili dodati novi. Izabrani tagovi se prikazuju kao chipovi koji se mogu ukloniti. Isti tag ne sme biti dodat dva puta.

**Vidljivost:** forma jasno objašnjava razliku između `Private` i `Followers`, a `Public` se može prikazati kao buduća opcija. Korisnik mora razumeti ko može videti recept pre nego što ga sačuva.

Pre čuvanja treba proveriti obavezna polja i prikazati jasne poruke greške pored polja, umesto generičkog upozorenja.

### 15.5. Izmena i brisanje recepta

Na Recipe Detail stranici samo autor dobija `Edit` i `Delete` akcije. Edit otvara istu strukturu forme kao dodavanje, ali popunjenu postojećim vrednostima.

Delete nikada ne treba da bude trenutna akcija. Klik otvara confirmation modal koji jasno navodi naziv recepta i objašnjava da brisanje nije isto što i uklanjanje iz kolekcije. Tek potvrdom se recept uklanja iz prototipskog stanja.

### 15.6. Recipe Detail stranica

Recipe Detail je najvažnija stranica za čitanje i korišćenje recepta. Predložena struktura je:

1. velika fotografija;
2. naziv i autor;
3. prosečna ocena i broj ocena;
4. tagovi;
5. opis;
6. vreme pripreme i kuvanja;
7. kontrola broja porcija;
8. glavne akcije;
9. sastojci;
10. koraci pripreme;
11. slični recepti;
12. predlozi priloga;
13. ocene i komentari.

Glavne akcije su `Save`, `Add to collection`, `Share`, `Export PDF`, a autoru se dodatno prikazuju `Edit` i `Delete`.

### 15.7. Save i Collections

`Save` i `Add to collection` su dve nezavisne funkcije.

**Save** predstavlja bookmark: korisnik želi brz pristup receptu kasnije. Jedan recept može biti sačuvan ili nes ačuvan, a akcija se menja u zavisnosti od trenutnog stanja.

**Collection** predstavlja korisnikovu organizaciju. Jedan recept može pripadati većem broju kolekcija. Klik na `Add to collection` otvara izbor korisnikovih kolekcija i jasno označava gde je recept već dodat.

U kolekciji mora postojati mogućnost `Remove from collection`. Ova akcija uklanja samo vezu između recepta i kolekcije i nikada ne briše sam recept.

Brisanje kolekcije takođe ne briše recepte u njoj. Confirmation modal treba eksplicitno da kaže da se briše samo kolekcija.

### 15.8. Tag sistem i pretraga

Tagovi su osnovni mehanizam za klasifikovanje i pronalaženje recepata. Ne postoji posebna kategorija koja bi ograničila recept na jednu grupu.

Primer:

`Riblja pašteta → posno, doručak, riba, brzo, namaz`

Pretraga `posno` vraća sve recepte sa tagom `posno`. Pretraga `doručak` vraća sve recepte sa tagom `doručak`. Pretraga `posno + doručak` vraća samo recepte koji imaju oba taga.

Search ekran treba da prikazuje:

* search input;
* aktivne tag filtere;
* dostupne ili predložene tagove;
* broj rezultata;
* opciju uklanjanja pojedinačnog filtera;
* `Clear all`;
* osnovno sortiranje.

Predloženo sortiranje je `Most relevant`, `Newest`, `Highest rated` i `Quickest`. U prototipu se sortiranje izvršava nad mock podacima.

Rezultati treba da budu paginirani ili simulirano učitavani u manjim grupama. Za prototip je dovoljno prikazati početni broj kartica i dugme `Load more`.

### 15.9. Empty states

Svaka prazna sekcija treba da objasni šta korisnik trenutno vidi i šta može sledeće da uradi. Ne koristiti generičko `No data`.

Primer za My Recipes:

> \\\*\\\*Tvoja knjiga recepata je prazna.\\\*\\\*
> Dodaj prvi recept i počni da gradiš svoju digitalnu kuvaricu.
> `\\\[+ Dodaj recept]`

Primer za Saved:

> \\\*\\\*Još nemaš sačuvane recepte.\\\*\\\*
> Sačuvaj recept da bi mu se kasnije brzo vratila.

Primer za Search:

> \\\*\\\*Nema rezultata.\\\*\\\*
> Probaj da ukloniš neki filter ili promeniš pojam za pretragu.

### 15.10. Servings calculator

Na detalju recepta korisnik može promeniti broj porcija pomoću `−` i `+`. Količine sastojaka se preračunavaju proporcionalno, ali originalni recept ostaje nepromenjen.

UI treba da prikazuje rezultat čitljivo. Decimalne količine mogu se pretvarati u razlomke kada je to smisleno, na primer `0.5` u `½` i `1.5` u `1½`.

Broj porcija ne sme pasti ispod 1. U prototipu se preračunavanje radi lokalno; kasnije se originalne vrednosti učitavaju iz backend-a, a izračunata vrednost ostaje samo prikaz na ekranu.

### 15.11. Share i visibility

Potrebno je jasno razlikovati promenu vidljivosti od deljenja linka.

`Visibility` određuje ko ima pravo pristupa receptu. `Share` je korisnička akcija kojom se recept može podeliti ili kopirati njegov link.

Share modal može sadržati:

* `Copy link`;
* `Share` za podržane browser/device opcije;
* kratak prikaz trenutne vidljivosti.

Promenu vidljivosti sme da vrši samo autor. U prototipu se promena može simulirati lokalno.

### 15.12. Profili

Moj profil prikazuje profilnu sliku, prikazano ime, username, bio, broj recepata, followers i following, kao i korisnikov sadržaj.

`Edit profile` otvara jednostavnu formu za izmenu profilne slike, prikazanog imena, username-a i bio-a. Nakon čuvanja treba prikazati toast potvrdu.

Na tuđem profilu korisnik vidi osnovne javno/dostupne informacije i akciju `Follow` ili `Unfollow`. Ne treba prikazivati akcije koje pripadaju samo vlasniku profila.

### 15.13. Followers i Following

Followers i Following treba da budu odvojene liste sa avatarom, username-om i osnovnom akcijom `Follow`/`Unfollow` gde je primenljivo. Klik na korisnika vodi na njegov profil.

MVP koristi jednostavno praćenje. Ne uvoditi prijateljstva, zahteve za prijateljstvo ili kompleksne odnose dok osnovni sistem nije stabilan.

### 15.14. Ocene

Ocena je od 1 do 5 zvezdica. Korisnik može jednom da oceni recept i kasnije promeni svoju ocenu.

Na receptu treba prikazati prosečnu ocenu i broj ocena. Kada korisnik već ima ocenu, UI treba da kaže `Your rating` i omogući promenu.

U prototipu se stanje može čuvati lokalno; u pravoj aplikaciji jedinstvenost ocene po korisniku i receptu garantuje backend/baza.

### 15.15. Komentari

Komentari se prikazuju ispod recepta. Korisnik koji ima pristup receptu dobija polje za unos i dugme `Post comment`.

Komentar prikazuje autora, tekst i vreme. Korisnik može obrisati svoj komentar, a autor recepta može u kasnijoj implementaciji imati mogućnost uklanjanja komentara sa svog recepta.

Nije potrebno uvoditi reply/thread sistem u MVP-u.

Nakon uspešnog postavljanja komentara treba prikazati toast, očistiti input i odmah prikazati novi komentar.

### 15.16. AI – „Ne znam, daj mi predloge"

AI ne treba da bude predstavljen kao generički chatbot. U osnovnom korisničkom toku treba da rešava konkretan problem: korisnik ne zna šta da sprema.

U prototipu se koristi vođeni wizard. Prvo se pita koliko vremena korisnik ima, na primer `15 min`, `30 min`, `1 sat` ili `Nije bitno`. Zatim se bira raspoloženje `Slatko`, `Slano` ili `Svejedno`, a zatim opciono preference kao `Posno`, `Vegetarijansko`, `Riba`, `Brzo` ili `Bez preference`.

Na kraju se prikazuje jedna ili više preporuka iz mock recepata, uz kratko objašnjenje zašto su izabrane. Ako nema odgovarajućih rezultata, korisnik dobija `Try different preferences`.

Pravi AI će kasnije ovu logiku unaprediti, ali prototip treba već sada da pokaže konačan korisnički tok.

### 15.17. AI – predlog priloga

Na Recipe Detail stranici postoji akcija tipa `What goes well with this?` ili `Predloži prilog`.

Prototip treba da prikaže nekoliko smislenih mock predloga na osnovu tagova ili tipa recepta. Predlozi treba da budu predstavljeni kao male recipe cards ili jednostavne preporuke koje korisnik može otvoriti.

Kasnije Quarkus šalje AI servisu relevantne podatke recepta i vraća preporuke. API ključ nikada nije deo frontend-a.

### 15.18. AI – slični recepti

`You might also like` prikazuje 3–4 recepta koji dele više tagova sa trenutno otvorenim receptom. Trenutni recept se ne prikazuje kao sopstvena preporuka.

U prototipu je dovoljno koristiti jednostavan skor, na primer broj zajedničkih tagova. Kasnije se može dodati AI semantička sličnost.

### 15.19. PDF export

Na Recipe Detail stranici postoji `Export PDF`. U prototipu se može simulirati print/PDF ponašanje, ali UI treba da predstavlja konačnu funkcionalnost: korisnik klikne dugme i dobija recept pripremljen za čuvanje ili štampanje.

Dugme treba da ima stanje učitavanja, na primer `Preparing PDF...`, a nakon završetka jasnu povratnu informaciju. Pravi PDF će kasnije generisati Quarkus i sadržati samostalan dizajn recepta.

Kasnija proširena verzija može omogućiti izvoz cele Collection u jednu digitalnu kuvaricu.

### 15.20. Responsive dizajn

Aplikacija mora od početka predvideti desktop i mobilni prikaz. Desktop može koristiti sidebar, dok mobilni prikaz koristi hamburger ili bottom navigation.

Recipe cards se na mobilnom prikazu slažu u jednu kolonu. Filteri se mogu otvoriti preko posebnog `Filters` dugmeta. Forme za dodavanje recepta moraju imati dovoljno velike inpute i dugmad za touch.

Posebno proveriti Home, Search, Recipe Detail, Add Recipe, Collections i Profile na uskim ekranima.

### 15.21. Toast poruke i feedback

Svaka uspešna ili neuspešna korisnička akcija treba da dobije jasnu povratnu informaciju. Primeri su:

* `Recipe saved`;
* `Removed from saved`;
* `Added to collection`;
* `Removed from collection`;
* `Profile updated`;
* `Rating saved`;
* `Comment posted`;
* `Link copied`;
* `Recipe deleted`.

Koristiti toast/snackbar komponentu umesto browser `alert()` prozora.

### 15.22. Opšta pravila UX-a

Interfejs treba da bude dosledan: ista akcija treba da izgleda i ponaša se isto na svim mestima. Destruktivne akcije koriste confirmation modal. Prazna stanja nude sledeći korak. Forme daju grešku pored konkretnog polja. Dugmad tokom obrade treba da imaju loading stanje kako korisnik ne bi slučajno poslao istu akciju više puta.

Prototip ne treba širiti dodatnim funkcionalnostima kao što su notifikacije, meal planner, shopping lista, marketplace, administratorski panel ili creator analytics. Te funkcionalnosti ostaju van trenutnog MVP-a i mogu se razmatrati tek nakon stabilizacije osnovnog proizvoda.

\---

## 16\. Tehnička arhitektura

Sistem je organizovan kroz frontend, backend, servis za autentifikaciju, bazu podataka i spoljne servise za AI. Svaka komponenta ima jasno definisanu odgovornost.

```text
                    ┌──────────────────┐
                    │      Korisnik     │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │      React       │
                    │    Frontend      │
                    └──────┬─────┬─────┘
                           │     │
                Login/Auth │     │ HTTPS / REST
                           ▼     ▼
                    ┌────────┐  ┌──────────────────┐
                    │Keycloak│  │      Quarkus     │
                    │  Auth  │  │      Backend     │
                    └────────┘  │                  │
                                │ REST Resources   │
                                │ Services         │
                                │ Repositories     │
                                │ Security         │
                                │ AI Integration   │
                                │ PDF Generation   │
                                └───────┬──────────┘
                                        │
                              ┌─────────┴─────────┐
                              ▼                   ▼
                       ┌──────────────┐   ┌──────────────┐
                       │   MongoDB    │   │ Gemini/Claude│
                       └──────────────┘   └──────────────┘
```

### React frontend

React je odgovoran za korisnički interfejs, navigaciju, forme, lokalno stanje interfejsa i komunikaciju sa REST API-jem. React ne pristupa direktno MongoDB bazi i ne komunicira direktno sa Gemini ili Claude API-jem.

### Keycloak

Keycloak je zasebna komponenta zadužena za autentifikaciju i upravljanje identitetom korisnika. Koristi se OpenID Connect protokol. Keycloak obrađuje registraciju, login i logout, čuva korisničke kredencijale i izdaje access token nakon uspešne autentifikacije. Lozinke se zbog toga ne čuvaju u MongoDB bazi.

React koristi Keycloak klijentsku konfiguraciju za pokretanje prijave. Nakon uspešnog login-a dobija token koji šalje Quarkus backend-u u `Authorization: Bearer <token>` zaglavlju. Quarkus validira token i iz njega dobija identitet trenutno prijavljenog korisnika.

### Quarkus backend

Quarkus predstavlja centralni backend aplikacije. Odgovoran je za poslovnu logiku, validaciju, REST API, autorizaciju, komunikaciju sa MongoDB bazom, AI servisom i PDF generatorom. Quarkus ne čuva lozinke korisnika.

Backend treba da bude organizovan po slojevima kako bi poslovna logika ostala odvojena od HTTP komunikacije i pristupa bazi.

### MongoDB

MongoDB čuva trajne podatke aplikacije, kao što su profili korisnika, recepti, kolekcije, tagovi, praćenja, sačuvani recepti, ocene i komentari. Veza između Keycloak identiteta i korisničkog profila u MongoDB-u ostvaruje se preko identifikatora korisnika iz Keycloak-a.

### AI servis

AI funkcionalnosti se pozivaju isključivo preko Quarkus backend-a. Quarkus priprema zahtev, poziva izabranog AI provajdera i vraća rezultat React aplikaciji. API ključevi se nikada ne smeju nalaziti u frontend kodu.

### PDF generator

PDF dokumente generiše backend. Quarkus prvo proverava da korisnik ima pravo pristupa receptu, zatim učitava podatke iz MongoDB-a, generiše PDF i vraća ga frontend-u kao fajl.

\---

## 17\. Tehnološki stack

### Frontend

* React
* JavaScript
* HTML
* CSS
* React Router za navigaciju
* Fetch API ili Axios za HTTP komunikaciju

React će biti organizovan kroz reusable komponente, na primer `RecipeCard`, `Tag`, `SearchBar`, `IngredientList`, `CommentList` i `Navbar`.

### Backend

* Java
* Quarkus
* Quarkus REST
* Keycloak / OpenID Connect
* Quarkus MongoDB Client / Panache MongoDB

Predložena struktura backend-a:

```text
resource/
service/
repository/
model/
dto/
security/
ai/
pdf/
exception/
config/
```

`resource/` sadrži REST endpoint-e, `service/` poslovnu logiku, `repository/` pristup MongoDB-u, `model/` modele podataka, `dto/` objekte za API komunikaciju, `security/` konfiguraciju bezbednosti i Keycloak integracije, `ai/` komunikaciju sa AI provajderom, a `pdf/` generisanje PDF dokumenata.

REST Resource prima HTTP zahtev. Service sadrži poslovnu logiku. Repository komunicira sa MongoDB-om. DTO objekti definišu podatke koji se šalju kroz API. Keycloak i Quarkus Security sloj obrađuju autentifikaciju i autorizaciju.

### Baza

* MongoDB
* MongoDB Atlas za cloud okruženje

### Verzije koda

* Git
* GitHub

### Deployment

React frontend može biti hostovan na GitHub Pages-u. Quarkus backend mora biti hostovan na servisu koji podržava Java aplikacije. MongoDB može biti hostovan preko MongoDB Atlas-a.

\---

## 18\. Predložena struktura MongoDB baze

MongoDB će koristiti više kolekcija.

### users

```text
{
  \\\_id,
  username,
  email,
  keycloakUserId,
  profileImage,
  bio,
  createdAt,
  updatedAt
}
```

### recipes

```text
{
  \\\_id,
  title,
  description,
  imageUrl,
  authorId,
  preparationTime,
  cookingTime,
  totalTime,
  difficulty,
  servings,
  ingredients: \\\[],
  preparationSteps: \\\[],
  tags: \\\[],
  visibility,
  createdAt,
  updatedAt
}
```

### tags

U početnoj verziji tag može biti običan string unutar recepta. Ako sistem poraste, može se napraviti posebna kolekcija:

```text
{
  \\\_id,
  name,
  normalizedName
}
```

### collections

```text
{
  \\\_id,
  ownerId,
  name,
  description,
  recipeIds: \\\[],
  createdAt,
  updatedAt
}
```

### savedRecipes

```text
{
  \\\_id,
  userId,
  recipeId,
  savedAt
}
```

### follows

```text
{
  \\\_id,
  followerId,
  followingId,
  createdAt
}
```

### ratings

```text
{
  \\\_id,
  userId,
  recipeId,
  rating,
  createdAt,
  updatedAt
}
```

### comments

```text
{
  \\\_id,
  userId,
  recipeId,
  text,
  createdAt,
  updatedAt
}
```

Posebne kolekcije za ocene, komentare, praćenja i sačuvane recepte sprečavaju da jedan Recipe dokument postane nepotrebno velik.

Konačan model baze biće potvrđen pre implementacije.

\---

## 19\. REST API

Frontend i backend komuniciraju preko REST API-ja.

### Authentication

```text
GET  /api/auth/me

Registracija, login i logout se obrađuju preko Keycloak-a, a Quarkus endpoint /api/auth/me služi za dobijanje podataka o trenutno prijavljenom korisniku.
```

### Users

```text
GET    /api/users/{id}
PUT    /api/users/me
GET    /api/users/me/followers
GET    /api/users/me/following
POST   /api/users/{id}/follow
DELETE /api/users/{id}/follow
```

### Recipes

```text
GET    /api/recipes
GET    /api/recipes/{id}
POST   /api/recipes
PUT    /api/recipes/{id}
DELETE /api/recipes/{id}
```

Pretraga i filteri mogu se proslediti kao query parametri:

```text
GET /api/recipes?search=pasta
GET /api/recipes?tags=posno
GET /api/recipes?tags=posno,dorucak
GET /api/recipes?difficulty=EASY
```

### Saved recipes

```text
GET    /api/saved-recipes
POST   /api/saved-recipes/{recipeId}
DELETE /api/saved-recipes/{recipeId}
```

### Collections

```text
GET    /api/collections
POST   /api/collections
GET    /api/collections/{id}
PUT    /api/collections/{id}
DELETE /api/collections/{id}
POST   /api/collections/{id}/recipes/{recipeId}
DELETE /api/collections/{id}/recipes/{recipeId}
```

### Ratings

```text
POST   /api/recipes/{id}/rating
PUT    /api/recipes/{id}/rating
DELETE /api/recipes/{id}/rating
```

### Comments

```text
GET    /api/recipes/{id}/comments
POST   /api/recipes/{id}/comments
PUT    /api/comments/{commentId}
DELETE /api/comments/{commentId}
```

### PDF

```text
GET /api/recipes/{id}/pdf
```

### AI

```text
POST /api/ai/suggestions
POST /api/ai/side-dishes
POST /api/ai/recipe-recommendations
```

Konačan API će biti detaljno definisan pre početka implementacije backenda.

\---

## 20\. Povezivanje komponenti sistema

Kada korisnik doda recept, tok podataka izgleda ovako:

```text
React forma
    ↓
HTTP POST
    ↓
Quarkus REST Resource
    ↓
DTO validacija
    ↓
RecipeService
    ↓
RecipeRepository
    ↓
MongoDB
    ↓
Response DTO
    ↓
React
    ↓
Prikaz rezultata
```

Kada korisnik pretražuje recepte:

```text
SearchBar
    ↓
React state
    ↓
GET /api/recipes?tags=posno,dorucak
    ↓
RecipeController
    ↓
RecipeService
    ↓
MongoDB query
    ↓
Rezultati
    ↓
React RecipeCard komponente
```

Kada korisnik koristi AI:

```text
React
    ↓
POST /api/ai/side-dishes
    ↓
Quarkus
    ↓
AiService
    ↓
Gemini ili Claude API
    ↓
AiService
    ↓
Quarkus response
    ↓
React
```

Kada korisnik generiše PDF:

```text
React
    ↓
GET /api/recipes/{id}/pdf
    ↓
Quarkus
    ↓
Provera prava pristupa
    ↓
MongoDB
    ↓
PDF generator
    ↓
PDF response
    ↓
Browser
```

\---

## 21\. Plan razvoja po fazama

### Faza 0 – Konačna specifikacija

Definišu se konačne funkcionalnosti, user stories, user flow, stranice, modeli podataka, API struktura, pravila pristupa i granice MVP-a.

### Faza 1 – Podešavanje razvojnog okruženja

Instalirati i podesiti Git, GitHub, Java JDK, Maven ili Gradle, Quarkus CLI (po potrebi), Node.js, React razvojno okruženje, MongoDB, Keycloak i IDE/editor.

Napraviti GitHub repository i početnu strukturu projekta.

### Faza 2 – Backend osnova

Napraviti Quarkus aplikaciju i podesiti Quarkus REST, Quarkus MongoDB Client / Panache MongoDB, konfiguraciju baze, osnovne modele, repository sloj, service sloj, REST resource sloj i globalno rukovanje greškama.

### Faza 3 – Frontend osnova

Napraviti React aplikaciju i osnovni layout. Implementirati routing, navbar/sidebar, login i register stranice, početnu stranicu i osnovne reusable komponente.

### Faza 4 – Autentifikacija

Podesiti Keycloak realm, client i OpenID Connect tok. React koristi Keycloak za registraciju, login i logout, a Quarkus validira access token i štiti REST endpoint-e.

### Faza 5 – Recepti

Implementirati kompletan CRUD, sastojke, korake, tagove, slike i ostale podatke recepta.

### Faza 6 – Pretraga i tagovi

Implementirati search bar, sistem tagova bez posebne kategorije, kombinovanje više tagova AND logikom, aktivne filtere, Clear all, sortiranje i paginaciju/load more.

### Faza 7 – Sačuvani recepti i kolekcije

Implementirati čuvanje recepata, uklanjanje iz sačuvanih, kreiranje kolekcija i dodavanje/uklanjanje recepata iz kolekcija.

### Faza 8 – Profili i društvene funkcionalnosti

Implementirati profil, praćenje, pratioce, korisnike koje korisnik prati, deljenje recepata i pravila pristupa.

### Faza 9 – Ocene i komentari

Implementirati dodavanje i izmenu ocene, prosečnu ocenu, komentare i izmenu/brisanje sopstvenih komentara.

### Faza 10 – PDF export

Implementirati generisanje PDF-a na backend-u i testirati recepte sa različitim količinama podataka.

### Faza 11 – AI integracija

Napraviti apstrakciju AI servisa i povezati izabrani provajder. Prvo implementirati jednu konkretnu funkcionalnost, na primer predlog priloga, a zatim dodavati ostale.

API ključevi se čuvaju isključivo u backend environment varijablama.

### Faza 12 – Testiranje

Testirati frontend, REST API, autentifikaciju, prava pristupa, CRUD operacije, pretragu, tagove, kolekcije, praćenje, komentare, PDF export i AI integraciju.

Posebno testirati pokušaje pristupa receptima kojima korisnik nema pravo pristupa.

### Faza 13 – Deployment

Frontend postaviti na GitHub Pages, Quarkus backend na odgovarajući Java hosting servis, a MongoDB na MongoDB Atlas. Podesiti environment varijable i CORS između frontenda i backenda.

### Faza 14 – Dokumentacija i završna obrada

Dodati README, opis projekta, uputstvo za pokretanje, arhitekturu sistema, API dokumentaciju, opis baze, screenshots aplikacije, poznata ograničenja i plan budućeg razvoja.

\---

## 22\. Pravila arhitekture projekta

Da bi projekat ostao pregledan, frontend i backend imaju jasno odvojene odgovornosti.

React ne pristupa direktno MongoDB bazi.

React ne sme sadržati MongoDB credentials.

React ne sme sadržati AI API ključ.

Quarkus je komponenta koja komunicira sa MongoDB bazom.

Quarkus komunicira sa AI provajderom.

Frontend šalje zahteve kroz REST API.

Controller ne treba da sadrži kompletnu poslovnu logiku. Controller prosleđuje zahtev Service sloju.

Service sloj odlučuje šta aplikacija treba da uradi.

Repository sloj je zadužen za komunikaciju sa bazom.

DTO objekti služe za komunikaciju između API-ja i klijenta, umesto da se direktno izlažu svi interni modeli baze.

\---

## 23\. Očekivani krajnji sistem

Korisnik se registruje i prijavi. Na početnoj stranici vidi pitanje „Šta pravimo danas?“ i može odmah da doda novi recept, pretraži svoje recepte ili zatraži predlog.

Korisnik kreira recept, dodaje fotografiju, sastojke, korake pripreme, vreme, težinu, broj porcija i više tagova. Recept se čuva u MongoDB bazi preko Quarkus REST API-ja.

Kasnije korisnik može da pronađe recept pomoću jednog ili više tagova, na primer `posno + doručak`. Može ga sačuvati u svoju kolekciju, podeliti sa drugim korisnikom ili generisati PDF verziju.

Korisnik može da prati druge korisnike i da pregleda recepte koji su mu dostupni. Na takvim receptima može ostaviti ocenu ili komentar.

AI funkcionalnosti korisniku daju dodatnu pomoć pri izboru hrane, pronalaženju sličnih recepata i izboru priloga.

Sistem je organizovan tako da se početni MVP može završiti bez implementiranja svih budućih funkcionalnosti, ali da arhitektura ostane dovoljno fleksibilna za kasnije proširenje na javnu bazu recepata, napredne AI preporuke i druge funkcionalnosti.

\---

## 24\. Napomena o daljoj implementaciji

Ovaj dokument predstavlja početnu tehničku specifikaciju projekta. Pre početka programiranja potrebno je još jednom proći kroz funkcionalnosti i potvrditi šta tačno ulazi u MVP.

Nakon potvrde specifikacije, razvoj treba započeti podešavanjem GitHub repozitorijuma i razvojnog okruženja, zatim kreiranjem Quarkus backenda i React frontenda. Nakon toga će se postepeno implementirati baza, autentifikacija, recepti, tagovi, pretraga, društvene funkcionalnosti, PDF export i AI integracija.

Svaka faza treba da bude završena i testirana pre prelaska na sledeću fazu.

\---

## 25\. Mogući poslovni model i monetizacija

Monetizacija aplikacije može biti zasnovana na **freemium modelu**, odnosno kombinaciji besplatnog osnovnog paketa i plaćenih naprednih funkcionalnosti. Cilj je da besplatna verzija bude dovoljno korisna da korisnik može da napravi i koristi svoju digitalnu knjigu recepata, dok se plaćanjem otključavaju funkcionalnosti koje pružaju veću vrednost intenzivnim korisnicima.

### 25.1. Besplatni paket – Free

Besplatna verzija može da omogući:

* kreiranje korisničkog naloga;
* dodavanje do određenog broja recepata, na primer 50;
* dodavanje slika, sastojaka, koraka i tagova;
* osnovnu pretragu i filtriranje;
* osnovne kolekcije;
* praćenje drugih korisnika;
* deljenje dostupnih recepata;
* ocene i komentare;
* osnovni PDF export;
* ograničen broj AI upita, na primer 5 mesečno.

Besplatni paket treba da omogući korisniku da u potpunosti razume vrednost aplikacije pre nego što odluči da plati.

### 25.2. Premium paket

Predložena cena Premium paketa je približno **4,99 € mesečno** ili **39,99 € godišnje**. Godišnji paket bi predstavljao finansijski povoljniju opciju za korisnike koji žele dugoročno da koriste aplikaciju.

Premium može da obuhvati:

* neograničen broj recepata;
* neograničen broj kolekcija;
* napredne filtere i pretragu;
* veći broj AI upita, na primer do 100 AI kredita mesečno;
* AI predloge priloga;
* AI preporuke recepata;
* preporuke na osnovu sastojaka koje korisnik ima;
* napredni PDF export;
* generisanje personalizovane digitalne kuvarice;
* dodatne opcije organizacije i privatnosti.

AI korišćenje ne bi trebalo predstavljati potpuno neograničenu funkcionalnost čak ni u Premium paketu, jer svaki zahtev ka Gemini ili Claude API-ju može predstavljati trošak za vlasnika aplikacije. Zbog toga se može koristiti sistem AI kredita.

### 25.3. AI krediti

AI funkcionalnosti mogu koristiti sistem kredita. Besplatni korisnik dobija ograničen broj kredita mesečno, dok Premium korisnik dobija veću količinu. Dodatni AI krediti mogu se u budućnosti prodavati kao jednokratni paketi.

Mogući primer:

* Free – 5 AI kredita mesečno;
* Premium – 100 AI kredita mesečno;
* dodatni paket – na primer 100 AI kredita za jednokratnu cenu.

Konačne količine i cene treba odrediti na osnovu stvarne cene izabranog AI provajdera i prosečne potrošnje korisnika.

### 25.4. Lifetime Premium

Kao promotivna ili jednokratna opcija može postojati **Lifetime Premium** paket po ceni od približno **49,99 €**.

Lifetime paket ne bi trebalo da obećava potpuno neograničeno AI korišćenje, jer bi dugoročni trošak AI API-ja mogao biti veći od jednokratne uplate korisnika. AI bi zato mogao imati poseban mesečni limit ili određenu količinu kredita.

Lifetime paket može biti posebno koristan u početnoj fazi proizvoda kao promotivna ponuda za prve korisnike.

### 25.5. Personalizovana PDF kuvarica

Pored pretplate, moguća je i jednokratna naplata generisanja personalizovane digitalne kuvarice. Korisnik može izabrati kolekciju ili veći broj svojih recepata i od njih napraviti dizajniranu PDF knjigu.

Predložena cena može biti približno **4,99 € po kuvarici**.

Na primer, korisnik može napraviti kuvaricu pod nazivom „Moji omiljeni recepti“, „Bakin receptar“ ili „Porodična kuvarica“. PDF može sadržati naslovnu stranu, fotografije, recepte, tagove i podatke o autoru.

Ova funkcionalnost predstavlja potencijalno zanimljiv proizvod i za poklone, štampanje i dugoročno čuvanje porodičnih recepata.

### 25.6. Creator Pro

U kasnijoj fazi, nakon razvoja javne baze recepata i većeg broja korisnika, može se uvesti poseban **Creator Pro** paket po predloženoj ceni od približno **9,99 € mesečno**.

Creator Pro bi bio namenjen food blogerima, kreatorima sadržaja, kuvarima i drugim korisnicima koji žele javniji profil. Funkcionalnosti mogu uključivati:

* javni profil;
* veći broj pratilaca i javnih recepata;
* napredne statistike pregleda recepata;
* statistiku popularnosti recepata;
* javne kolekcije;
* dodatne mogućnosti personalizacije profila.

### 25.7. Budući izvori prihoda

Nakon što aplikacija dostigne dovoljan broj korisnika, mogu se razmotriti dodatni izvori prihoda:

* partnerske kampanje sa proizvođačima hrane;
* sponzorisani recepti ili tematske kolekcije;
* affiliate programi za kuhinjsku opremu i sastojke;
* saradnja sa food kreatorima;
* dodatni premium AI paketi.

Ove mogućnosti nisu deo početnog MVP-a i ne bi trebalo da utiču na osnovnu funkcionalnost aplikacije.

### 25.8. Dugoročni Premium AI meal planner

Jedna od potencijalno najvrednijih premium funkcionalnosti može biti AI planer obroka. Korisnik može zadati period, na primer pet ili sedam dana, svoje recepte, preferencije, raspoloživo vreme i sastojke koje već ima. AI može predložiti raspored obroka koristeći recepte iz njegove knjige.

Na osnovu izabranog plana aplikacija može generisati i **shopping listu** potrebnih sastojaka. Na ovaj način AI ne služi samo za generisanje odgovora, već rešava konkretan problem planiranja ishrane i kupovine.

### 25.9. Predložena struktura prihoda

Početni poslovni model može se zasnivati na sledećim izvorima:

|Proizvod/funkcionalnost|Predložena cena|
|-|-:|
|Free|0 €|
|Premium mesečno|4,99 € / mesec|
|Premium godišnje|39,99 € / godina|
|Lifetime Premium|49,99 € jednokratno|
|Personalizovana PDF kuvarica|4,99 € po kuvarici|
|Creator Pro|9,99 € / mesec|
|Dodatni AI krediti|cena prema količini kredita|

Ove cene predstavljaju početne poslovne pretpostavke, a ne konačan cenovnik. Pre stvarnog lansiranja potrebno je analizirati troškove hostinga, baze, skladištenja slika, PDF generisanja, AI API-ja, procesora plaćanja i drugih operativnih troškova. Na osnovu tih troškova i ponašanja korisnika mogu se naknadno odrediti konačne cene i ograničenja paketa.

Najvažniji cilj monetizacije je da osnovna verzija ostane dovoljno korisna i pristupačna, dok premium funkcionalnosti nude jasnu dodatnu vrednost: naprednu organizaciju, AI pomoć, napredni export i mogućnost stvaranja lične digitalne kuvarice.

