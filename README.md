## UVOD

U priloženoj dokumentaciji prezentirat ćemo naš projektni zadatak iz kolegija Baze podataka 1. Cilj projekta je razviti funkcionalan model baze podataka koji omogućuje učinkovito upravljanje različitim aspektima rada kafića, uključujući narudžbe, rezervacije, zaposlenike, zalihe sirovina i financijske transakcije. 
Kao prvi korak izrade baze podataka odlučili smo generirati vlastite tablice i podatke, kako bismo pojednostavili unos točnih podataka i izbjegli oslanjanje na vanjske izvore.
## Opis ER dijagrama

[Link na Entity Relationship (ER) dijagram](https://lucid.app/lucidchart/7e3ca596-78ec-4f8d-9e66-618cb6cf1f40/edit?viewport_loc=-2689%2C-743%2C4235%2C1887%2C0_0&invitationId=inv_76bfcfcb-73cd-451d-8128-f57a1b90cb83)

Skup entiteta **Kupac** povezan je s entitetom **Rezervacija** u odnosu **više naprema jedan**, jer jedan kupac može napraviti više rezervacija, dok svaka rezervacija pripada samo jednom kupcu.

Skup entiteta **Rezervacija** povezan je s entitetom **Stol** preko veze **više naprema jedan**, jer više rezervacija može biti vezano za isti stol, ali pojedina rezervacija odnosi se samo na jedan stol u određeno vrijeme.

Skup entiteta **Kupac** također je posredno povezan s entitetom **Narudzba**, jer nakon što kupac sjedne za stol, može izvršiti jednu ili više narudžbi. Veza između **Kupac** i **Narudzba** bila bi **više naprema jedan** – jedan kupac može napraviti više narudžbi, dok svaka narudžba pripada jednom kupcu (ili stolu).

Entitet **Narudzba** povezan je s entitetom **StavkaNarudzbe** u vezi **jedan naprema više** – jedna narudžba može sadržavati više stavki (jela i pića), dok svaka stavka pripada samo jednoj narudžbi.

Entitet **StavkaNarudzbe** povezan je s entitetom **Proizvod** preko veze **više naprema jedan** – više stavki narudžbe mogu sadržavati isti proizvod, dok se svaka stavka odnosi samo na jedan proizvod.

Entitet **Proizvod** povezan je s entitetom **KategorijaProizvoda** u vezi **više naprema jedan**, jer više proizvoda može pripadati istoj kategoriji (npr. pića, glavna jela), dok svaki proizvod pripada jednoj kategoriji.

Skup entiteta **Narudzba** povezan je s entitetom **Placanje** u vezi **jedan naprema jedan** – svaka narudžba ima točno jedno plaćanje, dok se jedno plaćanje odnosi samo na jednu narudžbu.

Entitet **Zaposlenik** povezan je s entitetom **Uloga** preko veze **više naprema jedan** – više zaposlenika može imati istu ulogu (npr. konobar, kuhar), dok jedan zaposlenik ima samo jednu ulogu.

Entitet **Zaposlenik** povezan je s entitetom **Smjena** u vezi **više naprema više** – jedan zaposlenik može raditi u više smjena, dok jedna smjena može uključivati više zaposlenika. Ta veza omogućuje praćenje rasporeda rada.

Entitet **Sirovina** povezan je s entitetom **ZalihaSirovine** u vezi **jedan naprema jedan** – svaka sirovina ima stanje zalihe (količinu na skladištu), dok se zaliha odnosi samo na jednu sirovinu.

Entitet **Sirovina** također je povezan s entitetom **StavkaNabavneNarudzbe** u vezi **više naprema jedan** – više stavki može uključivati istu sirovinu, dok se svaka stavka odnosi na jednu određenu sirovinu.

Entitet **StavkaNabavneNarudzbe** povezan je s entitetom **NabavnaNarudzba** u vezi **više naprema jedan** – više stavki pripada jednoj nabavnoj narudžbi, dok se jedna stavka ne može dijeliti među više narudžbi.

Entitet **NabavnaNarudzba** povezan je s entitetom **Dobavljac** u vezi **više naprema jedan** – više narudžbi može biti naručeno od istog dobavljača, dok se jedna narudžba upućuje jednom dobavljaču.

Entitet **Proizvod** povezan je s entitetom **Dobavljac** preko pomoćnog entiteta **ProizvodDobavljac**, koji omogućuje vezu **više naprema više** – jedan proizvod može imati više dobavljača, a jedan dobavljač može isporučivati više proizvoda. Ova veza sadrži dodatni atribut **RokIsporuke**.

## Relacije, atributi i ograničenja


Relacija **kupac**\
Služi za pohranu podataka o kupcima kafića. Svaki kupac ima jedinstveni identifikator i može imati više rezervacija i narudžbi te svoj status lojalnosti. Relacija se sastoji od sljedećih atributa:

- *kupacID – podatak tipa INTEGER, koji je primarni ključ unutar relacije te obavezno mora imati vrijednost zbog PRIMARY KEY ograničenja*
- *ime – podatak tipa VARCHAR(255), osobni podaci kupca*
- *prezime – podatak tipa VARCHAR(255), osobni podaci kupca*
- *telefon – podatak tipa VARCHAR(255), kontakt*
- *email – podatak tipa VARCHAR(255), kontakt*
- *statusVjernosti – podatak tipa VARCHAR(255), označava status lojalnosti kupca (standardni, zlatni, VIP)*

```sql
CREATE TABLE Kupac (
  KupacID int PRIMARY KEY,
  Ime varchar(255),
  Prezime varchar(255),
  Telefon varchar(255),
  Email varchar(255),
  StatusVjernosti varchar(255)
);
```

Relacija **zaposlenik**\
Sadrži informacije o zaposlenicima kafića. Svaki zaposlenik pripada određenoj ulozi i može biti zadužen za narudžbe ili smjene. Sastoji od sljedećih atributa:

- *zaposlenikID – podatak tipa INTEGER, koji je primarni ključ unutar relacije i obavezno mora imati vrijednost (zbog PRIMARY KEY ograničenja)*
- *ime – podatak tipa VARCHAR(255), osobni podaci*
- *prezime – podatak tipa VARCHAR(255), osobni podaci*
- *ulogaID – podatak tipa INTEGER, predstavlja strani ključ koji referencira relaciju uloga*
- *telefon – podatak tipa VARCHAR(255), kontakt*
- *email – podatak tipa VARCHAR(255), kontakt* 

```sql
CREATE TABLE Zaposlenik (
  ZaposlenikID int PRIMARY KEY,
  Ime varchar(255),
  Prezime varchar(255),
  UlogaID int,
  Telefon varchar(255),
  Email varchar(255)
);
```

Relacija **uloga**\
Evidentira vrste uloga koje zaposlenici mogu imati. Relacija uloga se sastoji od sljedećih atributa:

- *ulogaID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije i obavezno mora imati vrijednost (zbog PRIMARY KEY ograničenja)
- *nazivUloge* – podatak tipa VARCHAR(255)

```sql
CREATE TABLE Uloga (
  UlogaID int PRIMARY KEY,
  NazivUloge varchar(255)
);
```

**Relacija stol**\
Relacija Stol koristi se za pohranu podataka o fizičkim stolovima u kafiću koji su dostupni za rezervaciju i posluživanje narudžbi. Pomaže u organizaciji sjedećih mjesta i upravljanju kapacitetima prostora.

- *stolID – podatak tipa INTEGER, primarni ključ jedinstveno identificira svaki stol*
- *brojStola – podatak tipa INTEGER, oznaka stola*
- *kapacitet – podatak tipa INTEGER, maksimalan broj osoba koje mogu sjesti za stol.*

```sql
CREATE TABLE Stol (
  StolID int PRIMARY KEY,
  BrojStola int,
  Kapacitet int
);
```

**Relacija narudzba**\
Relacija Narudzba evidentira sve narudžbe koje su kupci napravili, uključujući informacije o vremenu narudžbe, zaposleniku koji je obradio narudžbu te stolu za kojim je narudžba zabilježena.

- *narudzbaID – podatak tipa INTEGER, koji je primarni ključ unutar relacije*
- *datumVrijeme – podatak tipa DATETIME, označava točno vrijeme narudžbe*
- *kupacID – podatak tipa INTEGER, predstavlja **strani ključ** koji referencira kupca koji je napravio narudžbu*
- *stolID – podatak tipa INTEGER, predstavlja **strani ključ** koji povezuje narudžbu sa stolom za kojim je napravljena*
- *zaposlenikID – podatak tipa INTEGER, predstavlja **strani ključ** koji označava zaposlenika koji je zaprimio narudžbu*

```sql
CREATE TABLE Narudzba (
  NarudzbaID int PRIMARY KEY,
  DatumVrijeme datetime,
  KupacID int,
  StolID int,
  ZaposlenikID int
);
```

**Relacija rezervacija**\
Relacija Rezervacija prati podatke o rezervacijama koje kupci izrađuju za određene stolove u kafiću. Svaka rezervacija uključuje informaciju o vremenu, broju osoba te statusu rezervacije. 

- *RezervacijaID – podatak tipa INTEGER, koji je primarni ključ unutar relacije i jedinstveno identificira svaku rezervaciju*
- *KupacID – podatak tipa INTEGER, koji se ponaša kao strani ključ i povezan je s primarnim ključem tablice Kupac*
- *StolID – podatak tipa INTEGER, koji se ponaša kao strani ključ i povezan je s primarnim ključem tablice Stol*
- *DatumVrijeme – podatak tipa DATETIME, koji označava točan datum i vrijeme kada je rezervacija zakazana*
- *BrojOsoba – podatak tipa INTEGER, koji označava broj osoba za koje je rezervacija napravljena*
- *Status – podatak tipa VARCHAR(255). Koristi se za praćenje statusa rezervacije, primjerice: “aktivna”, “otkazana” ili “dovršena”*

```sql
CREATE TABLE Rezervacija (
  RezervacijaID int PRIMARY KEY,
  KupacID int,
  StolID int,
  DatumVrijeme datetime,
  BrojOsoba int,
  `Status` varchar(255)
);
```

**Relacija StakvaNarudzbe**\
Relacija StavkaNarudzbe predstavlja vezu između narudžbi i pojedinačnih proizvoda koji su naručeni. Svaka narudžba može sadržavati više stavki, a svaka stavka odnosi se na određeni proizvod s određenom količinom i cijenom.

  - *NarudzbaID - složeni primarni ključ (zajedno s ProizvodID), označava kojoj narudžbi stavka pripada, podatak tipa INTEGER*
  - *ProizvodID -  složeni primarni ključ  označava koji je proizvod naručen, podatak tipa INTEGER*
  - *Kolicina - količina naručenog proizvoda, podatak tipa INTEGER*
  - *JedinicnaCijena - cijena po jedinici proizvoda u trenutku narudžbe (omogućuje praćenje povijesnih cijena), podatak tipa DECIMAL*
  - *PRIMARY KEY (NarudzbaID, ProizvodID) - NarudzbaID je strani ključ prema tablici Narudzba. ProizvodID je strani ključ prema tablici Proizvod. Jedna narudžba može sadržavati više   stavki, a jedan proizvod može biti dio više narudžbi.*
  
```sql
CREATE TABLE StavkaNarudzbe (
  NarudzbaID int,
  ProizvodID int,
  Kolicina int,
  JedinicnaCijena decimal,
  PRIMARY KEY (NarudzbaID, ProizvodID)
);
```

**Relacija Proizvod**\
Relacija Proizvod sadrži sve artikle koje kafić nudi kupcima, bilo da se radi o pićima ili drugim uslugama. Svaki proizvod pripada određenoj kategoriji i ima definiranu cijenu i opis.

 - *ProizvodID PRIMARY KEY - Primarni ključ, jedinstveni identifikator proizvoda, podatak tipa INTEGER*
  - *Naziv -  Ime proizvoda , podatak tipa VARCHAR(255)*
  - *Opis - Detaljniji opis proizvoda, podatak tipa TEXT*
  - *Cijena - Trenutna jedinična cijena proizvoda, podatak tipa DECIMAL*
  - *KategorijaID - Strani ključ, povezuje proizvod s njegovom kategorijom (npr. piće, hrana), podatak tipa INTEGER*

Relacije:
- Veza s KategorijaProizvoda: svaki proizvod pripada jednoj kategoriji.
- Veza s StavkaNarudzbe: proizvod može biti dio više narudžbi.
- Veza s ProizvodDobavljac: omogućuje praćenje dobavljača za svaki proizvod.

```sql
CREATE TABLE Proizvod (
  ProizvodID int PRIMARY KEY,
  Naziv varchar(255),
  Opis text,
  Cijena decimal,
  KategorijaID int
);
```

**Relacija KategorijaProizvoda**\
Relacija KategorijaProizvoda služi za klasifikaciju proizvoda. Omogućuje lakše upravljanje i filtriranje proizvoda prema vrsti, što olakšava narudžbu, analizu prodaje i ažuriranje cjenika.

- *KategorijaID PRIMARY KEY - Primarni ključ, jedinstveni identifikator kategorije proizvoda, podatak tipa INTEGER*
- *NazivKategorije varchar(255) - Tekstualni naziv kategorije (npr. "bezalkoholna pića"), podatak tipa VARCHAR(255)*
  
- *Veza s relacijom Proizvod - Jedna kategorija može obuhvaćati više proizvoda, dok svaki proizvod pripada točno jednoj kategoriji.

```sql
CREATE TABLE KategorijaProizvoda (
  KategorijaID int PRIMARY KEY,
  NazivKategorije varchar(255)
);
```

**Relacija Placanje**\
Relacija Placanje pohranjuje informacije o izvršenim plaćanjima za narudžbe. Omogućuje praćenje ukupnog iznosa naplate, metode plaćanja i vremena transakcije.

  - *PlacanjeID PRIMARY KEY - Primarni ključ, jedinstveni identifikator plaćanja, podatak tipa INTEGER*
  - *NarudzbaID - Strani ključ, označava narudžbu na koju se plaćanje odnosi, podatak tipa INTEGER*
  - *Iznos - Ukupni iznos plaćen za narudžbu, podatak tipa DECIMAL*
  - *NacinPlacanja - Tekstualna oznaka načina plaćanja (npr. gotovina, kartica) podatak tipa VARCHAR(255)*
  - *DatumVrijeme - Datum i vrijeme kad je plaćanje izvršeno, podatak tipa DATETIME*

  - Relacije:
-*Veza 1:1 s tablicom Narudzba – svaka narudžba može imati jedno plaćanje*
-*Omogućuje analizu prodaje i financijskog poslovanja-*

```sql
CREATE TABLE Placanje (
  PlacanjeID int PRIMARY KEY,
  NarudzbaID int,
  Iznos decimal,
  NacinPlacanja varchar(255),
  DatumVrijeme datetime
);
```

**Relacija Dobavljac**\
Relacija Dobavljac sadrži informacije o vanjskim dobavljačima koji isporučuju sirovine ili gotove proizvode. Omogućuje praćenje podataka i izgradnju odnosa s partnerima.

 - *DobavljacID PRIMARY KEY - Primarni ključ, jedinstveni identifikator dobavljača, podatak tipa INTEGER*
 - *Naziv - Ime dobavljačke firme, podatak tipa VARCHAR(255)*
 - *Telefon -Kontakt podaci dobavljača, podatak tipa VARCHAR(255)*
 - *Email  - Kontakt podaci dobavljača, podatak tipa VARCHAR(255)*
   
  Relacije:
- *Veza s ProizvodDobavljac i NabavnaNarudzba: jedan dobavljač može isporučivati više proizvoda ili sirovina*

```sql
CREATE TABLE Dobavljac (
  DobavljacID int PRIMARY KEY,
  Naziv varchar(255),
  Telefon varchar(255),
  Email varchar(255)
);
```

**Relacija ProizvodDobavljac**\
Predstavlja vezu između proizvoda i dobavljača. Koristi se za praćenje koji dobavljač može dostaviti koji proizvod i u kojem roku.

 - *ProizvodID - Složeni primarni ključ, definira vezu između određenog proizvoda i dobavljača, podatak tipa INTEGER*
 - *DobavljacID - Složeni primarni ključ, definira vezu između određenog proizvoda i dobavljača, podatak tipa INTEGER*
 - *RokIsporuke - Datum ili vremenski rok unutar kojeg se očekuje isporuka proizvoda, podatak tipa DATETIME*
 - *PRIMARY KEY (ProizvodID, DobavljacID) - Primarni ključ predstavlja složeni ključ koji jedinstveno identificira svaki zapis kao kombinaciju određenog proizvoda i dobavljača. Na taj način se sprječava ponavljanje istih parova i osigurava ispravnost veze više na više između proizvoda i dobavljača.*

```sql
CREATE TABLE ProizvodDobavljac (
  ProizvodID int,
  DobavljacID int,
  RokIsporuke datetime,
  PRIMARY KEY (ProizvodID, DobavljacID)
);
```

**Relacija Sirovina**\
Sadrži popis svih sirovina koje se koriste za pripremu proizvoda (npr. kava, mlijeko). Svaka sirovina ima mjeru koja određuje kako se zalihe vode.

- *SirovinaID PRIMARY KEY -  Primarni ključ jedinstveni identifikator sirovine, podatak tipa INTEGER*
- *Naziv - Naziv sirovine, podatak tipa VARCHAR(255)*
- *JedinicaMjere - Mjerna jedinica (npr. litra, gram), podatak tipa VARCHAR(255)*

Relacije:
- *Sirovina — StavkaNabavneNarudzbe: Veza jedan na jedan. Ista sirovina može biti sadržana u više stavki različitih nabavnih narudžbi.*

```sql
CREATE TABLE Sirovina (
  SirovinaID int PRIMARY KEY,
  Naziv varchar(255),
  JedinicaMjere varchar(255)
);
```

**Relacija ZalihaSirovina**\
Prati trenutno stanje sirovina na skladištu. Ključno za praćenje potrošnje i pravovremenu nabavu novih zaliha.

 - *SirovinaID PRIMARY KEY - Primarni ključ i strani ključ prema Sirovina, podatak tipa INTEGER*
 - *KolicinaNaSkladistu - Trenutna dostupna količina, podatak tipa  DECIMAL,*
 - *GranicaNarudzbe -  Donja granica ispod koje se automatski pokreće narudžba, podatak tipa  DECIMAL*

Relacije:
- *Tablica ZalihaSirovina je u vezi jedan na jedan s tablicom Sirovina, jer se za svaku sirovinu evidentira točno jedno stanje zaliha. Osigurava da svaka sirovina ima jedan zapis o trenutnoj količini i granici za ponovnu narudžbu.*

```sql
CREATE TABLE ZalihaSirovina (
  SirovinaID int PRIMARY KEY,
  KolicinaNaSkladistu decimal,
  GranicaNarudzbe decimal
);
```

**Relacija NabavnaNarudzba**\
Evidentira sve narudžbe sirovina koje se šalju dobavljačima. Omogućuje praćenje datuma narudžbe i očekivanog dolaska robe.

- *NabavnaNarudzbaID* – Primarni ključ, podatak tipa INTEGER
- *DobavljacID* – Strani ključ, označava kojem dobavljaču je narudžba upućena, podatak tipa INTEGER
- *DatumNarudzbe* – Datum kada je narudžba napravljena, podatak tipa DATE
- *OcekivaniDatum* – Datum kada se očekuje isporuka, podatak tipa DATE

Relacije:

- *NabavnaNarudzba — Dobavljac: Veza više na jedan – više narudžbi može biti upućeno istom dobavljaču. Povezivanje se vrši putem stranog ključa DobavljacID*
- *NabavnaNarudzba — StavkaNabavneNarudzbe: Veza jedan na više – jedna nabavna narudžba može sadržavati više različitih sirovina. Povezivanje se vrši preko NabavnaNarudzbaID.*

```sql
CREATE TABLE NabavnaNarudzba (
  NabavnaNarudzbaID int PRIMARY KEY,
  DobavljacID int,
  DatumNarudzbe date,
  OcekivaniDatum date
);
```

**Relacija StavkaNabavneNarudzbe**\
Povezuje svaku narudžbu s konkretnim sirovinama koje su naručene i njihovim količinama.

- *NabavnaNarudzbaID - Predstavlja identifikator nabavne narudžbe, podatak tipa INTEGER. Ova vrijednost određuje kojoj narudžbi pripada pojedinačna stavka.*
- *SirovinaID – Složeni primarni ključ, podatak tipa INTEGER. Označava točno koju sirovinu sadrži ta stavka unutar određene narudžbe.*
- *Kolicina – Količina naručene sirovine, podatak tipa DECIMAL.*
- *PRIMARY KEY (NabavnaNarudzbaID, SirovinaID) - Složeni primarni ključ. Jedinstveno identificira svaki red u tablici kombinacijom NabavnaNarudzbaID i SirovinaID.  Sprječava da ista sirovina bude više puta unesena unutar iste narudžbe.*
  
Relacije:
- StavkaNabavneNarudzbe — NabavnaNarudzba: Tip veze: više na jedan. Više stavki može pripadati istoj narudžbi. Veza se ostvaruje preko: stranog ključa NabavnaNarudzbaID.
- StavkaNabavneNarudzbe — Sirovina: Tip veze: više na jedan. Više stavki može sadržavati istu sirovinu, jer ista sirovina može biti naručena u različitim narudžbama. Veza se ostvaruje preko: stranog ključa SirovinaID.

```sql
CREATE TABLE StavkaNabavneNarudzbe (
  NabavnaNarudzbaID int,
  SirovinaID int,
  Kolicina decimal,
  PRIMARY KEY (NabavnaNarudzbaID, SirovinaID)
);
```

**Relacija Smjena**\
Evidentira smjene koje zaposlenici odrađuju, uključujući vrijeme početka i završetka smjene.

- *SmjenaID – Identifikator smjene, podatak tipa INTEGER. Predstavlja jedinstveni ID svake smjene.*
- *ZaposlenikID – Strani ključ, podatak tipa INTEGER. Označava koji zaposlenik radi određenu smjenu.*
- *DatumVrijemePocetka – Početak smjene, podatak tipa DATETIME.*
- *DatumVrijemeZavrsetka – Kraj smjene, podatak tipa DATETIME.*
- *PRIMARY KEY (SmjenaID) – Primarni ključ. Osigurava jedinstvenost svake smjene u tablici.*

Relacije:

Smjena — Zaposlenik
Tip veze: više na jedan. Više smjena može pripadati istom zaposleniku.
Veza se ostvaruje preko: stranog ključa ZaposlenikID.

```sql
CREATE TABLE Smjena (
  SmjenaID int PRIMARY KEY,
  ZaposlenikID int,
  DatumVrijemePocetka datetime,
  DatumVrijemeZavrsetka datetime
);
```

## Alter table ograničenja

**Alter Table Zaposlenik**

Putem stranog ključa osiguravamo da svaki zaposlenik ima točno definiranu ulogu.

- *Ograničenje UlogaID – povezuje zaposlenika s tablicom Uloga, čime se osigurava da svaki zaposlenik može imati samo postojeću i definiranu ulogu poput "Konobar", "Menadžer" itd.*

```sql
ALTER TABLE Zaposlenik ADD FOREIGN KEY (UlogaID) REFERENCES Uloga (UlogaID);
```
**Alter Table Rezervacija**

Povezujemo rezervaciju s kupcem i stolom kako bismo znali tko i gdje ima rezervaciju.

- *KupacID – osigurava da rezervaciju može imati samo postojeći kupac.*
- *StolID – veže rezervaciju za konkretan fizički stol.*

```sql
ALTER TABLE Rezervacija ADD FOREIGN KEY (KupacID) REFERENCES Kupac (KupacID);
ALTER TABLE Rezervacija ADD FOREIGN KEY (StolID) REFERENCES Stol (StolID);
```

**Alter table Narudzba**

Narudžba se mora povezati s kupcem koji naručuje, stolom za kojim sjedi i zaposlenikom koji ju zaprimio.

- *KupacID – narudžba mora biti vezana za kupca koji ju je napravio.*
- *StolID – narudžba mora biti vezana za stvarni stol.*
- *ZaposlenikID – narudžbu mora zaprimiti postojeći zaposlenik.*

```sql
ALTER TABLE Narudzba ADD FOREIGN KEY (KupacID) REFERENCES Kupac (KupacID);
ALTER TABLE Narudzba ADD FOREIGN KEY (StolID) REFERENCES Stol (StolID);
ALTER TABLE Narudzba ADD FOREIGN KEY (ZaposlenikID) REFERENCES Zaposlenik (ZaposlenikID);
```

**Alter table StavkaNarudzbe**

Svaka stavka u narudžbi mora pripadati postojećoj narudžbi i odnositi se na stvarni proizvod.

- *NarudzbaID – stavka mora biti dio postojeće narudžbe.*
- *ProizvodID – stavka mora referencirati stvarni proizvod iz ponude.*
  
```sql
ALTER TABLE StavkaNarudzbe ADD FOREIGN KEY (NarudzbaID) REFERENCES Narudzba (NarudzbaID);
ALTER TABLE StavkaNarudzbe ADD FOREIGN KEY (ProizvodID) REFERENCES Proizvod (ProizvodID);
```

**Alter table Proizvod**

Svaki proizvod mora pripadati točno jednoj kategoriji.

- *KategorijaID – svaki proizvod mora biti klasificiran unutar postojećih kategorija (npr. kava, alkoholna pića itd.)*

```sql
ALTER TABLE Proizvod ADD FOREIGN KEY (KategorijaID) REFERENCES KategorijaProizvoda (KategorijaID);
```
**Alter table Placanje**

Svako plaćanje mora biti povezano s postojećom narudžbom.

- *NarudzbaID – ne može postojati plaćanje za narudžbu koja nije zabilježena u sustavu.*

```sql
ALTER TABLE Placanje ADD FOREIGN KEY (NarudzbaID) REFERENCES Narudzba (NarudzbaID);
```

**Alter table ProizvodDobavljac**

- *Povezujemo proizvode i dobavljače kako bismo znali tko što isporučuje.*
- *ProizvodID – mora biti validan proizvod.*
- *DobavljacID – mora biti validan dobavljač.*

```sql
ALTER TABLE ProizvodDobavljac ADD FOREIGN KEY (ProizvodID) REFERENCES Proizvod (ProizvodID);
ALTER TABLE ProizvodDobavljac ADD FOREIGN KEY (DobavljacID) REFERENCES Dobavljac (DobavljacID);
```

**Alter table ZalihaSirovina**

Za svaku sirovinu mora postojati stanje zaliha.

- *SirovinaID – zaliha mora biti povezana s postojećom sirovinom.*

```sql
ALTER TABLE `ZalihaSirovina` ADD FOREIGN KEY (`SirovinaID`) REFERENCES `Sirovina` (`SirovinaID`);
```

**Alter table NabavnaNarudzba**

Svaka nabavna narudžba mora biti upućena stvarnom dobavljaču.

- *DobavljacID – narudžba mora biti poslana dobavljaču koji postoji u sustavu.*

```sql
ALTER TABLE NabavnaNarudzba ADD FOREIGN KEY (DobavljacID) REFERENCES Dobavljac (DobavljacID);
```

**Alter table StavkaNabavneNarudzbe**

Stavke unutar nabavne narudžbe moraju biti povezane s postojećim narudžbama i sirovinama.

- *NabavnaNarudzbaID – stavka mora pripadati narudžbi.*
- *SirovinaID – stavka mora sadržavati stvarnu sirovinu.*

```sql
ALTER TABLE StavkaNabavneNarudzbe ADD FOREIGN KEY (NabavnaNarudzbaID) REFERENCES NabavnaNarudzba (NabavnaNarudzbaID);
ALTER TABLE StavkaNabavneNarudzbe ADD FOREIGN KEY (SirovinaID) REFERENCES Sirovina (SirovinaID);
```

**Alter table Smjena**

Svaka smjena mora pripadati zaposleniku.

-  *ZaposlenikID – smjenu može imati samo netko tko je zaposlen.*

```sql
ALTER TABLE Smjena ADD FOREIGN KEY (ZaposlenikID) REFERENCES Zaposlenik (ZaposlenikID);
```

**Podaci**

Za unos podataka u tablice koristi se naredba INSERT INTO naziv_tablice VALUES, pri čemu se u zagradama navode vrijednosti u istom redoslijedu kojim su definirani stupci prilikom kreiranja tablice. Primjer:

```sql

-- Zaposlenik
INSERT INTO Zaposlenik (ZaposlenikID, Ime, Prezime, UlogaID, Telefon, Email) VALUES
(1, 'Ana', 'Perić', 1, '0911111111', 'ana.peric@caffe.com'),
(2, 'Marko', 'Ivić', 1, '0922222222', 'marko.ivic@caffe.com'),
(3, 'Luka', 'Marić', 1, '0933333333', 'luka.maric@caffe.com'),
(4, 'Petra', 'Horvat', 1, '0944444444', 'petra.horvat@caffe.com'),
(5, 'Ivana', 'Kovač', 1, '0955555555', 'ivana.kovac@caffe.com'),
(6, 'Dino', 'Tomić', 1, '0966666666', 'dino.tomic@caffe.com'),
(7, 'Jelena', 'Babić', 1, '0977777777', 'jelena.babic@caffe.com'),
(8, 'Ivan', 'Đukić', 1, '0988888888', 'ivan.dukic@caffe.com'),
(9, 'Lea', 'Božić', 1, '0999999999', 'lea.bozic@caffe.com'),
(10, 'Niko', 'Radić', 1, '0910000001', 'niko.radic@caffe.com');

```

## UPITI

**UPIT 1**

 Upit 1 prikazuje Koliko naruzdbi ima svaki dobavljač

 - *Spajamo dvije tablice NabavnaNarudzba i Dobavljac, Spajamo DobavljacID iz tablice i pomoću operacije COUNT prebrojimo sve naruzbe pojedinog Dobavljaca, Sortiramo ih premo kolicini naruzba.*

```sql
SELECT D.Naziv AS NazivDobavljaca, 
COUNT(N.NabavnaNarudzbaID) AS BrojNarudzbi 
FROM NabavnaNarudzba AS N 
JOIN Dobavljac AS D ON N.DobavljacID = D.DobavljacID 
GROUP BY D.DobavljacID, D.Naziv 
ORDER BY BrojNarudzbi DESC, D.Naziv;
```
**UPIT 2**

Upit 2 prikazuje koliko rezervacija ima kafić po svakom danu

- *Koristimo operaciju DATE kako bih limitirali podatke samo na datum, Koristeći COUNT operaciju prebrojimo sve rezervacije, koje ona grupiramo po datumu sa operacijom GROUP BY.*
  
```sql
SELECT DATE(DatumVrijeme) AS Dan, 
       COUNT(*) AS RezervacijePoDanu 
FROM Rezervacija 
GROUP BY DATE(DatumVrijeme);
```

**UPIT 3**

Upit 3 prikazuje prosječni broj dana isporuke svakog Dobavljaca 

- *Koristeći DATEFIFF operaciju dobimo razliku dana između OcekivaniDatum i DatumNarudzbe. Zatim spojimo DobavkljacID od tablice Dobavljač i NabavaNarudzba. Nakraju grupiramo podatke pomoću DobavljacID sa operacijom GROUP BY, i zatim sortiramo rezultat uzlaznim redosljedom*

```sql
  SELECT D.Naziv AS NazivDobavljaca,
       AVG(DATEDIFF(N.OcekivaniDatum, N.DatumNarudzbe)) AS ProsjecnoIsporukeDana 
FROM NabavnaNarudzba AS N 
JOIN Dobavljac AS D ON N.DobavljacID = D.DobavljacID 
GROUP BY D.DobavljacID, D.Naziv 
ORDER BY ProsjecnoIsporukeDana, D.Naziv;
```

**UPIT 4**

Upit 4 prikazuje Količinu, Cijenu i Naziv proizvoda koji je pojedini kupac naručio.

- *Želimo spojiti 4 tablice, Korisitmo INNER JOIN operaciju za tablicu Kupac,StavkaNarudzbe i Proizvod da se spoje sa tablicom Narudzba, Sortiramo rezultat operacijom ORDER BY Uzlaznim redosljedom.*

```sql
  SELECT Naru.NarudzbaID, DatumVrijeme, Ime, Prezime, Kolicina, JedinicnaCijena, Naziv 
FROM (((Narudzba AS Naru  
INNER JOIN Kupac AS Kup ON Naru.KupacID = Kup.KupacID)  
INNER JOIN StavkaNarudzbe AS Stav ON Naru.NarudzbaID = Stav.NarudzbaID) 
INNER JOIN Proizvod AS Pro ON Stav.ProizvodID = Pro.ProizvodID) 
ORDER BY Naru.DatumVrijeme;
```

**UPIT 5**

Upit 5 prikazuje smjene i radne dane svakoga zaposlenika.

- *Želimo prebrojiti broj radni dana na kojim je Zaposlenik radio,Koristimo COUNT() operator sa DISTINCT DATE() koji koji neće brojati isti datum. Koristimo INNER JOIN za spajanje tablice Zaposlenik i Uloga sa tablicom Smjena, zatim Grupiramo podatke i sortiramo sa ORDER BY.*

```sql
SELECT Z.Ime, Z.Prezime, U.NazivUloge, 
       COUNT(S.SmjenaID) AS BrojSmjena,
       COUNT(DISTINCT DATE(S.DatumVrijemePocetka)) AS BrojRadnihDana
FROM (Smjena AS S
INNER JOIN Zaposlenik AS Z ON S.ZaposlenikID = Z.ZaposlenikID)
INNER JOIN Uloga AS U ON Z.UlogaID = U.UlogaID
GROUP BY Z.ZaposlenikID, Z.Ime, Z.Prezime, U.NazivUloge
ORDER BY BrojSmjena DESC, Z.Prezime;
```

**UPIT 6**

Upit 6 dohvaća sve proizvode i njihove kategorije.

- *U ovom upitu svi proizvodi su uređeni po kateriji alkoholna pića, kava ili bezalkoholna pića.
Kod SELECT-a smo stavili potrebne stupce iz obje tablice i promijenili smo ime tako da se moze bolje razumijeti.
I na kraju smo s tablicom Proizvod sa naredbom JOIN povezali tablicu KategorijaProizvoda i 
sa naredbom ON smo napravili da vrijednosti KategorijaID koji smo naveli u tablicu Proizvod bude jednaka s virijednosti
kKategorijaID u tablici KategorijaProizvoda.* 

```sql
SELECT proizvod.Naziv AS NazivProizvoda, KategorijaProizvoda.NazivKategorije AS Kategorija
FROM Proizvod 
JOIN KategorijaProizvoda ON proizvod.KategorijaID = KategorijaProizvoda.KategorijaID;
```

**UPIT 7**

Upit 7 ronalazi sve kupce sa statusom vjernosti 'Zlatni'.

- *U ovom upitu naveli sva imena kupca s statusom zlatni.
Kod SELECT smo stavili određene stupce iz tablice Kupac i sa WHERE naredbom smo rekli da kupci 
moraju imati status 'Zlatni'.*

```sql
SELECT KupacID, Ime, Prezime, Email
FROM Kupac
WHERE StatusVjernosti = 'Zlatni';
```

**UPIT 8**

Upit 8 broji rezervacija za svaki stol.

- *U ovom uvijetu prebrojavamo koliko imamo rezervacija za svani stol.
Kod SELECT smo uzeli određene stupce koje želimo da se ispiše iz obje tablice koje povezujemo.
COUNT koristimo kako bi se ispisala ukupan broj rezervacija za svaki stol.
JOIN koristimo kako bi smo povezali tablicu Rezervacija sa tablicom stol i ON smo napravili da vrijednosti StolID koji smo naveli 
u tablicu Rezervacija bude jednaka s virijednosti StolID u tablici Stol. 
GROUP BY korisimo da bi dobili ukupan broj rezervacija za pojedini Stol, da nemamo tu naredbu samo bi nam izracunalo
ukupan broj rezervacija iz cijele tablice i vratio nam samo jedan broj.
ORDER BY BrojRezervacija DESC koristio da dam izpiše broj rezervacija za svaki stol redoslijedom pocevši od najvećeg.* 

```sql
SELECT Stol.BrojStola, COUNT(Rezervacija.RezervacijaID) AS BrojRezervacija
FROM Rezervacija 
JOIN Stol ON Rezervacija.StolID = Stol.StolID
GROUP BY Stol.BrojStola
ORDER BY BrojRezervacija DESC;
```

**UPIT 9**

Upit 9 prikazuje Imena kupaca koji su napravili rezervacije za stolove s kapacitetom većim od 4 osobe.

- *Kod SELECTA smo stavili i naredbu DISTINCT kako bi izbjegli se isti kupac više puta ispiše jer kupac može imati više rezervacija 
broj rezervacija.
sa tablicom Kupac smo spojili dvije tablise i koristili ON da bi se povezane tablice podudarale sa stranim ključevima.
Kod WHERE smo stavili sa ispiše sve kupce koje su rezervirali stolove s kapasitetom većim od 4.*

```sql
SELECT
    DISTINCT Kupac.Ime AS ImeKupca,
    Kupac.Prezime AS PrezimeKupca
FROM Kupac 
JOIN Rezervacija  ON Kupac.KupacID = Rezervacija.KupacID
JOIN Stol  ON Rezervacija.StolID = Stol.StolID
WHERE Stol.Kapacitet > 4;
```

**UPIT 10**

Upit 10 prikazuje ukupan broj prodanih proizvoda po zaposleniku.

- *U ovom upitu vidimo koliko je svaki zaposlenik prodao proizvoda.
Kod SELECTA ispisuje nam se ime i prezime zaposlenika i koliko je ukupno prodao proizvoda sa agregacijom SUM(sn.Kolicina) i 
nazive stupca smo preimenovali da se bolje razumije. Za naziv tablice koristimo kratice da nam bude lakše za pisati
z = Zaposlenik i sn = StavkaNarudzbe. Prvo smo tablicu Narudžba povezali sa tablicom Zaposlenik, pa onda StavkaNarudzbe sa 
tablicom Narudžba. GROUP BY smo koristili da iznačuna sumu prdanih proizvoda po jednom zaposleniku.
ORDER BY UkupnoProdanihJedinica DESC koristimo da počne ispisivati od zaposlenika sa najviše prodanih proizvoda.*

```sql
SELECT z.Ime AS ImeZaposlenika, z.Prezime AS PrezimeZaposlenika, SUM(sn.Kolicina) AS UkupnoProdanihJedinica
FROM Zaposlenik AS z
JOIN Narudzba AS n ON z.ZaposlenikID = n.ZaposlenikID
JOIN StavkaNarudzbe AS sn ON n.NarudzbaID = sn.NarudzbaID
GROUP BY z.ZaposlenikID, z.Ime, z.Prezime
ORDER BY
    UkupnoProdanihJedinica DESC;
```

**UPIT 11**

Upit 11 prikazuje opis svih rezervacija koje su potvrđene.

- *SELECT RezervacijaID, KupacID, StolID, DatumVrijeme – biramo stupce koje želimo prikazati.
FROM Rezervacija – podatke dohvaćamo iz tablice *Rezervacija*.
WHERE Status = Potvrđena – uzimamo samo redove gdje je status rezervacije potvrđen.
ORDER BY DatumVrijeme – rezultati su sortirani kronološki po datumu i vremenu rezervacije.*

```sql
SELECT 
    RezervacijaID, 
    KupacID, 
    StolID, 
    DatumVrijeme 
FROM Rezervacija
WHERE Status = 'Potvrđena'
ORDER BY DatumVrijeme;
```

**UPIT 12**

Upit 12 prikazuje popis narudžbi sa imenima kupaca i konobara.

- *Ovaj upit može se koristiti kada vlasnik restorana želi dobiti detaljan pregled svih narudžbi, uključujući informacije o tome tko je naručio, tko je zaprimio narudžbu i kada se ta narudžba dogodila.
Povezivanje se vrši pomoću ključeva KupacID i ZaposlenikID, pri čemu se koristi JOIN kako bi se dohvatili svi podaci. Imena i prezimena kupaca i konobara spajaju se funkcijom CONCAT, zbog boljeg prikaza.
Rezultat prikazuje ID narudžbe, ime kupca, ime konobara i datum narudžbe. Pomoću ORDER BY n.DatumVrijeme DESC najnovije narudžbe prikazujemo prve.*

```sql
SELECT 
    n.NarudzbaID,
    CONCAT(k.Ime, ' ', k.Prezime) AS Kupac,
    CONCAT(z.Ime, ' ', z.Prezime) AS Konobar,
    n.DatumVrijeme
FROM Narudzba n
JOIN Kupac k ON n.KupacID = k.KupacID
JOIN Zaposlenik z ON n.ZaposlenikID = z.ZaposlenikID
ORDER BY n.DatumVrijeme DESC;
```

**UPIT 13**

Upit 13 prikazuje popis kupaca koji su platili narudžbu gotovinom, te ima datumom i iznos.

- *Ovaj upit vraća popis svih kupaca koji su platili narudžbu gotovinom, zajedno s datumom plaćanja i ukupnim iznosom, može koristiti kada želimo uvid u sve  gotovinske transakcije unutar sustava.
Povezuju se Placanje, Narudzba i Kupac,  Prvo se Placanje spaja s Narudzba putem NarudzbaID, a zatim s tablicom Kupac pomoću KupacID, kako bi našli osobu koja je platila narudžbu.
CONCAT kombinira ime i prezime kako bi se prikazali u jedinstvenom stupcu Kupac. Kroz WHERE se filtriraju samo oni zapisi koji kao način plaćanja imaju 'Gotovina'.
Rezultati su sortirani po datumu plaćanja u silaznom redoslijedu, tako da se najnovija gotovinska plaćanja prikazuju prva. Ovaj upit pomaže u financijskoj kontroli gotovine i jednostavnom pregledu gotovinskih transakcija po kupcima.*

```sql
SELECT 
    CONCAT(k.Ime, ' ', k.Prezime) AS Kupac,
    p.DatumVrijeme,
    p.Iznos
FROM Placanje p
JOIN Narudzba n ON p.NarudzbaID = n.NarudzbaID
JOIN Kupac k ON n.KupacID = k.KupacID
WHERE p.NacinPlacanja = 'Gotovina'
ORDER BY p.DatumVrijeme DESC;
```

**UPIT 14**

Upit 14 prikazuje listu lop 10 kupaca po ukupnoj potrošnji.

- *Upit prikazuje top 10 kupaca koji su najviše potrošili u restoranu. To omogućuje vlasniku uvid u najvrijednije kupce. Ovo može poslužiti pri nagrađivanju određenih kupaca.
Povezuju se Kupac, Narudzba i Placanje. Prvo se Kupac povezuje s Narudzba pomoću KupacID, a zatim se Narudzba povezuje s Placanje preko NarudzbaID. Na taj način se dolazi do svih plaćenih iznosa koje je pojedini kupac ostvario kroz svoje narudžbe.
SUM(p.Iznos) računa ukupnu potrošnju za svakog kupca, a ROUND zaokružuje iznos na dvije decimale. Grupiranjem po KupacID, Ime i Prezime osigurava se ispravno zbrajanje po osobi, dok ORDER BY UkupnoPotroseno DESC sortira rezultate tako da se kupci koji troše najviše nalaze na vrhu.
LIMIT 10 ograničava rezultat samo na prvih 10 kupaca.*

```sql
SELECT 
    k.KupacID,
    CONCAT(k.Ime, ' ', k.Prezime) AS Kupac,
    ROUND(SUM(p.Iznos), 2) AS UkupnoPotroseno
FROM Kupac k
JOIN Narudzba n ON k.KupacID = n.KupacID
JOIN Placanje p ON n.NarudzbaID = p.NarudzbaID
GROUP BY k.KupacID, k.Ime, k.Prezime
ORDER BY UkupnoPotroseno DESC
LIMIT 10;
```

**UPIT 15**

prikazuje listu top 5 proizvoda po broju narudžbi.

- *Ovaj upit prikazuje nove informacije o top 5 narudžbi. To može poslužiti kako bi se prilagodili potražnji.
Pogled Top5Proizvoda prikazuje pet najprodavanijih proizvoda prema ukupnoj količini narudžbi. Kreira se spajanjem tablice Proizvod s StavkaNarudzbe, gdje se za svaki proizvod zbraja koliko puta je naručen. Grupiranjem po nazivu proizvoda dobiva se ukupan broj naručenih jedinica, a pomoću ORDER BY i LIMIT 5 izdvajaju se oni proizvodi koji su najviše traženi.*

```sql
CREATE VIEW Top5Proizvoda AS
SELECT 
    p.Naziv AS Proizvod,
    SUM(sn.Kolicina) AS UkupnaKolicina
FROM Proizvod p
JOIN StavkaNarudzbe sn ON p.ProizvodID = sn.ProizvodID
GROUP BY p.Naziv
ORDER BY UkupnaKolicina DESC
LIMIT 5;
```

**UPIT 16**

Ovaj upit prikazuje top 3 konobara prema ukupnoj vrijednosti naplaćenih narudžbi u lipnju 2025.

```sql
SELECT
    Z.Ime,
    Z.Prezime,
    U.NazivUloge,
    SUM(P.Iznos) AS UkupnaNaplacenaVrijednost
FROM
    Zaposlenik Z
JOIN
    Uloga U ON Z.UlogaID = U.UlogaID
JOIN
    Narudzba N ON Z.ZaposlenikID = N.ZaposlenikID
JOIN
    Placanje P ON N.NarudzbaID = P.NarudzbaID
WHERE
    U.NazivUloge = 'Konobar'
    AND P.DatumVrijeme >= '2025-06-01 00:00:00'  -- Početak lipnja
    AND P.DatumVrijeme < '2025-07-01 00:00:00'   -- Kraj lipnja (prije početka srpnja)
GROUP BY
    Z.ZaposlenikID, Z.Ime, Z.Prezime, U.NazivUloge
ORDER BY
    UkupnaNaplacenaVrijednost DESC
LIMIT 3;
```


**UPIT 17**

Ovaj upit prikazuje kupce sa 'Zlatnim' statusom vjernosti koji su imali rezervaciju u svibnju 2025., ali nisu napravili niti jednu narudžbu tog mjeseca.

```sql
SELECT
    K.KupacID,
    K.Ime,
    K.Prezime,
    K.Email,
    K.StatusVjernosti,
    R.DatumVrijeme AS DatumRezervacije,
    R.Status AS StatusRezervacije
FROM
    Kupac K
JOIN
    Rezervacija R ON K.KupacID = R.KupacID
WHERE
    K.StatusVjernosti = 'Zlatni'
    AND R.DatumVrijeme >= '2025-06-01 00:00:00' -- Početak lipnja
    AND R.DatumVrijeme < '2025-07-01 00:00:00'  -- Kraj lipnja (prije početka srpnja)
    AND NOT EXISTS (
        SELECT 1
        FROM Narudzba N
        WHERE N.KupacID = K.KupacID
        AND N.DatumVrijeme >= '2025-06-01 00:00:00' -- Početak lipnja
        AND N.DatumVrijeme < '2025-07-01 00:00:00'  -- Kraj lipnja
    )
ORDER BY
    K.Prezime, K.Ime;
```
    

**UPIT 18**

Ovaj upit prikazuje mjesečni trend prodaje za 2025. godinu: Ukupni prihod i broj jedinstvenih kupaca po mjesecu.
    
```sql
SELECT
    DATE_FORMAT(P.DatumVrijeme, '%Y-%m') AS GodinaMjesec,
    SUM(P.Iznos) AS UkupniPrihod,
    COUNT(DISTINCT N.KupacID) AS BrojJedinstvenihKupaca
FROM
    Placanje P
JOIN
    Narudzba N ON P.NarudzbaID = N.NarudzbaID
WHERE
    YEAR(P.DatumVrijeme) = 2025
GROUP BY
    GodinaMjesec
ORDER BY
    GodinaMjesec;
```

