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

- *kupacID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije te obavezno mora imati vrijednost zbog PRIMARY KEY ograničenja
- *ime* – podatak tipa VARCHAR(255), osobni podaci kupca
- *prezime* – podatak tipa VARCHAR(255), osobni podaci kupca
- *telefon* – podatak tipa VARCHAR(255), kontakt 
- *email* – podatak tipa VARCHAR(255), kontakt
- *statusVjernosti* – podatak tipa VARCHAR(255), označava status lojalnosti kupca (npr. standardni, zlatni, VIP)


<img width="255" alt="image" src="https://github.com/user-attachments/assets/d2121af8-fcb0-4efd-aa2e-4bdee13d38e4" />

Relacija **zaposlenik**\
Sadrži informacije o zaposlenicima kafića. Svaki zaposlenik pripada određenoj ulozi i može biti zadužen za narudžbe ili smjene. Sastoji od sljedećih atributa:

- *zaposlenikID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije i obavezno mora imati vrijednost (zbog PRIMARY KEY ograničenja)
- *ime* – podatak tipa VARCHAR(255), osobni podaci
- *prezime* – podatak tipa VARCHAR(255), osobni podaci
- *ulogaID* – podatak tipa INTEGER, predstavlja strani ključ koji referencira relaciju uloga
- *telefon* – podatak tipa VARCHAR(255), kontakt
- *email* – podatak tipa VARCHAR(255), kontakt 

<img width="206" alt="image" src="https://github.com/user-attachments/assets/d4c695a5-f55e-45d5-b573-e5a1f4b7d4a8" />

Relacija **uloga**\
Evidentira vrste uloga koje zaposlenici mogu imati. Relacija uloga se sastoji od sljedećih atributa:

- *ulogaID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije i obavezno mora imati vrijednost (zbog PRIMARY KEY ograničenja)
- *nazivUloge* – podatak tipa VARCHAR(255)

<img width="172" alt="image" src="https://github.com/user-attachments/assets/2fc2edd6-5d70-464e-aa35-db0f2226ee7e" />

**Relacija stol**\
Relacija Stol koristi se za pohranu podataka o fizičkim stolovima u kafiću koji su dostupni za rezervaciju i posluživanje narudžbi. Pomaže u organizaciji sjedećih mjesta i upravljanju kapacitetima prostora.

- *stolID* – podatak tipa INTEGER, primarni ključ jedinstveno identificira svaki stol
- *brojStola* – podatak tipa INTEGER, oznaka stola
- *kapacitet* – podatak tipa INTEGER, maksimalan broj osoba koje mogu sjesti za stol.

<img width="173" alt="image" src="https://github.com/user-attachments/assets/0499c256-8096-42a3-bfdf-007be3700498" />

**Relacija narudzba**\
Relacija Narudzba evidentira sve narudžbe koje su kupci napravili, uključujući informacije o vremenu narudžbe, zaposleniku koji je obradio narudžbu te stolu za kojim je narudžba zabilježena.

- *narudzbaID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije
- *datumVrijeme* – podatak tipa DATETIME, označava točno vrijeme narudžbe
- *kupacID* – podatak tipa INTEGER, predstavlja **strani ključ** koji referencira kupca koji je napravio narudžbu
- *stolID* – podatak tipa INTEGER, predstavlja **strani ključ** koji povezuje narudžbu sa stolom za kojim je napravljena
- *zaposlenikID* – podatak tipa INTEGER, predstavlja **strani ključ** koji označava zaposlenika koji je zaprimio narudžbu

<img width="196" alt="image" src="https://github.com/user-attachments/assets/dc5b4609-3c12-4e5c-b975-a63f3125f29d" />

**Relacija rezervacija**\
Relacija Rezervacija prati podatke o rezervacijama koje kupci izrađuju za određene stolove u kafiću. Svaka rezervacija uključuje informaciju o vremenu, broju osoba te statusu rezervacije. 

- *RezervacijaID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije i jedinstveno identificira svaku rezervaciju
- *KupacID* – podatak tipa INTEGER, koji se ponaša kao strani ključ i povezan je s primarnim ključem tablice Kupac
- *StolID* – podatak tipa INTEGER, koji se ponaša kao strani ključ i povezan je s primarnim ključem tablice Stol
- *DatumVrijeme* – podatak tipa DATETIME, koji označava točan datum i vrijeme kada je rezervacija zakazana
- *BrojOsoba* – podatak tipa INTEGER, koji označava broj osoba za koje je rezervacija napravljena
- *Status* – podatak tipa VARCHAR(255). Koristi se za praćenje statusa rezervacije, primjerice: “aktivna”, “otkazana” ili “dovršena”

<img width="209" alt="image" src="https://github.com/user-attachments/assets/911358ea-0c1d-4366-b1ce-f4144d464e2a" />

**Relacija StakvaNarudzbe**\
Relacija StavkaNarudzbe predstavlja vezu između narudžbi i pojedinačnih proizvoda koji su naručeni. Svaka narudžba može sadržavati više stavki, a svaka stavka odnosi se na određeni proizvod s određenom količinom i cijenom.

  - *NarudzbaID* - složeni primarni ključ (zajedno s ProizvodID), označava kojoj narudžbi stavka pripada, podatak tipa INTEGER
  - *ProizvodID* -  složeni primarni ključ  označava koji je proizvod naručen, podatak tipa INTEGER
  - *Kolicina* - količina naručenog proizvoda, podatak tipa INTEGER
  - *JedinicnaCijena* - cijena po jedinici proizvoda u trenutku narudžbe (omogućuje praćenje povijesnih cijena), podatak tipa DECIMAL
  - *PRIMARY KEY (NarudzbaID, ProizvodID)* - NarudzbaID je strani ključ prema tablici Narudzba. ProizvodID je strani ključ prema tablici Proizvod. Jedna narudžba može sadržavati više   stavki, a jedan proizvod može biti dio više narudžbi.
  
<img width="238" alt="image" src="https://github.com/user-attachments/assets/168ce966-159a-4dda-828e-8038db9df5d4" />

**Relacija Proizvod**\
Relacija Proizvod sadrži sve artikle koje kafić nudi kupcima, bilo da se radi o pićima ili drugim uslugama. Svaki proizvod pripada određenoj kategoriji i ima definiranu cijenu i opis.

 - *ProizvodID PRIMARY KEY* - Primarni ključ, jedinstveni identifikator proizvoda, podatak tipa INTEGER
  - *Naziv * -  Ime proizvoda , podatak tipa VARCHAR(255)
  - *Opis* - Detaljniji opis proizvoda, podatak tipa TEXT
  - *Cijena* - Trenutna jedinična cijena proizvoda, podatak tipa DECIMAL
  - *KategorijaID* - Strani ključ, povezuje proizvod s njegovom kategorijom (npr. piće, hrana), podatak tipa INTEGER

Relacije:
- Veza s KategorijaProizvoda: svaki proizvod pripada jednoj kategoriji.
- Veza s StavkaNarudzbe: proizvod može biti dio više narudžbi.
- Veza s ProizvodDobavljac: omogućuje praćenje dobavljača za svaki proizvod.

<img width="197" alt="image" src="https://github.com/user-attachments/assets/3ba03331-8c14-46b8-a20d-6688471e4d68" />

**Relacija KategorijaProizvoda**\
Relacija KategorijaProizvoda služi za klasifikaciju proizvoda. Omogućuje lakše upravljanje i filtriranje proizvoda prema vrsti, što olakšava narudžbu, analizu prodaje i ažuriranje cjenika.

- *KategorijaID PRIMARY KEY* - Primarni ključ, jedinstveni identifikator kategorije proizvoda, podatak tipa INTEGER
- *NazivKategorije varchar(255)* - Tekstualni naziv kategorije (npr. "bezalkoholna pića"), podatak tipa VARCHAR(255)
  
- *Veza s relacijom Proizvod - Jedna kategorija može obuhvaćati više proizvoda, dok svaki proizvod pripada točno jednoj kategoriji.

<img width="211" alt="image" src="https://github.com/user-attachments/assets/6f9d8067-222a-47c1-95b8-723c5112c0d7" />

**Relacija Placanje**\
Relacija Placanje pohranjuje informacije o izvršenim plaćanjima za narudžbe. Omogućuje praćenje ukupnog iznosa naplate, metode plaćanja i vremena transakcije.

  - *PlacanjeID PRIMARY KEY* - Primarni ključ, jedinstveni identifikator plaćanja, podatak tipa INTEGER
  - *NarudzbaID* - Strani ključ, označava narudžbu na koju se plaćanje odnosi, podatak tipa INTEGER
  - *Iznos* - Ukupni iznos plaćen za narudžbu, podatak tipa DECIMAL
  - *NacinPlacanja* - Tekstualna oznaka načina plaćanja (npr. gotovina, kartica) podatak tipa VARCHAR(255),
  - *DatumVrijeme* - Datum i vrijeme kad je plaćanje izvršeno, podatak tipa DATETIME

  - Relacije:
-*Veza 1:1 s tablicom Narudzba – svaka narudžba može imati jedno plaćanje*
-*Omogućuje analizu prodaje i financijskog poslovanja-*

<img width="182" alt="image" src="https://github.com/user-attachments/assets/c5d65043-de19-40ac-b97d-c9a22b4a8d61" />

**Relacija Dobavljac**\
Relacija Dobavljac sadrži informacije o vanjskim dobavljačima koji isporučuju sirovine ili gotove proizvode. Omogućuje praćenje podataka i izgradnju odnosa s partnerima.

 - *DobavljacID PRIMARY KEY* - Primarni ključ, jedinstveni identifikator dobavljača, podatak tipa INTEGER
 - *Naziv* - Ime dobavljačke firme, podatak tipa VARCHAR(255),
  - *Telefon* -Kontakt podaci dobavljača, podatak tipa VARCHAR(255),
  - *Email*  - Kontakt podaci dobavljača, podatak tipa VARCHAR(255)
  - 
  Relacije:
- *Veza s ProizvodDobavljac i NabavnaNarudzba: jedan dobavljač može isporučivati više proizvoda ili sirovina*

  <img width="203" alt="image" src="https://github.com/user-attachments/assets/8cc06499-8d77-4c45-8fac-07a5a73c30fd" />

**Relacija ProizvodDobavljac**\
Predstavlja vezu između proizvoda i dobavljača. Koristi se za praćenje koji dobavljač može dostaviti koji proizvod i u kojem roku.

 - *ProizvodID* - Složeni primarni ključ, definira vezu između određenog proizvoda i dobavljača, podatak tipa INTEGER
  - *DobavljacID* - Složeni primarni ključ, definira vezu između određenog proizvoda i dobavljača, podatak tipa INTEGER
  - *RokIsporuke* - Datum ili vremenski rok unutar kojeg se očekuje isporuka proizvoda, podatak tipa DATETIME
  - *PRIMARY KEY (ProizvodID, DobavljacID)* - Primarni ključ predstavlja složeni ključ koji jedinstveno identificira svaki zapis kao kombinaciju određenog proizvoda i dobavljača. Na taj način se sprječava ponavljanje istih parova i osigurava ispravnost veze više na više između proizvoda i dobavljača.

<img width="251" alt="image" src="https://github.com/user-attachments/assets/db862383-40ba-4c99-a58f-3fd280e12e27" />

**Relacija Sirovina**\
Sadrži popis svih sirovina koje se koriste za pripremu proizvoda (npr. kava, mlijeko). Svaka sirovina ima mjeru koja određuje kako se zalihe vode.

- *SirovinaID PRIMARY KEY* -  Primarni ključ jedinstveni identifikator sirovine, podatak tipa INTEGER
 - *Naziv* -Naziv sirovine, podatak tipa VARCHAR(255),
 - *JedinicaMjere* - Mjerna jedinica (npr. litra, gram), podatak tipa VARCHAR(255)

Relacije:
- * Sirovina — StavkaNabavneNarudzbe: Veza jedan na jedan. Ista sirovina može biti sadržana u više stavki različitih nabavnih narudžbi.*

<img width="204" alt="image" src="https://github.com/user-attachments/assets/518e288e-90b3-43b6-8223-9a69b2979dcb" />

**Relacija ZalihaSirovina**\
Prati trenutno stanje sirovina na skladištu. Ključno za praćenje potrošnje i pravovremenu nabavu novih zaliha.

 -*SirovinaID PRIMARY KEY* - Primarni ključ i strani ključ prema Sirovina, podatak tipa INTEGER
 - *KolicinaNaSkladistu* - Trenutna dostupna količina, podatak tipa  DECIMAL,
  - *GranicaNarudzbe* -  Donja granica ispod koje se automatski pokreće narudžba, podatak tipa  DECIMAL

Relacije:
- * Tablica ZalihaSirovina je u vezi jedan na jedan s tablicom Sirovina, jer se za svaku sirovinu evidentira točno jedno stanje zaliha. Osigurava da svaka sirovina ima jedan zapis o trenutnoj količini i granici za ponovnu narudžbu.*

<img width="204" alt="image" src="https://github.com/user-attachments/assets/97e4b503-8105-436e-8e06-b2f1edb1b8a9" />

**Relacija NabavnaNarudzba**\
Evidentira sve narudžbe sirovina koje se šalju dobavljačima. Omogućuje praćenje datuma narudžbe i očekivanog dolaska robe.

- *NabavnaNarudzbaID* – Primarni ključ, podatak tipa INTEGER
- *DobavljacID* – Strani ključ, označava kojem dobavljaču je narudžba upućena, podatak tipa INTEGER
- *DatumNarudzbe* – Datum kada je narudžba napravljena, podatak tipa DATE
- *OcekivaniDatum* – Datum kada se očekuje isporuka, podatak tipa DATE

Relacije:

- *NabavnaNarudzba — Dobavljac: Veza više na jedan – više narudžbi može biti upućeno istom dobavljaču. Povezivanje se vrši putem stranog ključa DobavljacID*
- *NabavnaNarudzba — StavkaNabavneNarudzbe: Veza jedan na više – jedna nabavna narudžba može sadržavati više različitih sirovina. Povezivanje se vrši preko NabavnaNarudzbaID.*

  <img width="221" alt="image" src="https://github.com/user-attachments/assets/7e85c5ad-1906-4810-bb21-e985c7852155" />

**Relacija StavkaNabavneNarudzbe**\
Povezuje svaku narudžbu s konkretnim sirovinama koje su naručene i njihovim količinama.

- *NabavnaNarudzbaID* - Predstavlja identifikator nabavne narudžbe, podatak tipa INTEGER. Ova vrijednost određuje kojoj narudžbi pripada pojedinačna stavka.
- *SirovinaID* – Složeni primarni ključ, podatak tipa INTEGER. Označava točno koju sirovinu sadrži ta stavka unutar određene narudžbe.
- *Kolicina* – Količina naručene sirovine, podatak tipa DECIMAL. 
- *PRIMARY KEY (NabavnaNarudzbaID, SirovinaID)* - Složeni primarni ključ. Jedinstveno identificira svaki red u tablici kombinacijom NabavnaNarudzbaID i SirovinaID.  Sprječava da ista sirovina bude više puta unesena unutar iste narudžbe.
  
Relacije:
- StavkaNabavneNarudzbe — NabavnaNarudzba: Tip veze: više na jedan. Više stavki može pripadati istoj narudžbi. Veza se ostvaruje preko: stranog ključa NabavnaNarudzbaID.
- StavkaNabavneNarudzbe — Sirovina: Tip veze: više na jedan. Više stavki može sadržavati istu sirovinu, jer ista sirovina može biti naručena u različitim narudžbama. Veza se ostvaruje preko: stranog ključa SirovinaID.

<img width="281" alt="image" src="https://github.com/user-attachments/assets/a8e49f80-e032-498a-aaaf-cc440677c5f2" />

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

```sql
INSERT INTO Uloga (UlogaID, NazivUloge) VALUES
(1, 'Konobar'),
(2, 'Menadžer'),
(3, 'Šanker');

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


INSERT INTO Kupac (KupacID, Ime, Prezime, Telefon, Email, StatusVjernosti) VALUES
(1, 'Ivana', 'Horvat', '0911234567', 'ivana.horvat@example.com', 'Zlatni'),
(2, 'Marko', 'Babić', '0912345678', 'marko.babic@example.com', 'Srebrni'),
(3, 'Ana', 'Kovač', '0913456789', 'ana.kovac@example.com', 'Brončani'),
(4, 'Petra', 'Babić', '0984433221', 'petra.babic@email.com', 'Srebrni'),
(5, 'Luka', 'Novak', '0929988776', 'luka.novak@email.com', 'Zlatni'),
(6, 'Sara', 'Jurić', '0918887776', 'sara.juric@email.com', 'Brončani'),
(7, 'Nikola', 'Pavić', '0982223344', 'nikola.pavic@email.com', 'Srebrni'),
(8, 'Maja', 'Knežević', '0991112223', 'maja.knezevic@email.com', 'Zlatni'),
(9, 'Ivan', 'Božić', '0923344556', 'ivan.bozic@email.com', 'Brončani'),
(10, 'Ema', 'Perić', '0955566778', 'ema.peric@email.com', 'Srebrni'),
(11, 'Tomislav', 'Šarić', '0914455667', 'tomislav.saric@email.com', 'Zlatni'),
(12, 'Iva', 'Đurić', '0981113333', 'iva.djuric@email.com', 'Srebrni'),
(13, 'Josip', 'Klarić', '0910009998', 'josip.klaric@email.com', 'Brončani'),
(14, 'Marta', 'Milinković', '0953322110', 'marta.milinkovic@email.com', 'Srebrni'),
(15, 'Kristijan', 'Lovrić', '0921212121', 'kristijan.lovric@email.com', 'Zlatni'),
(16, 'Tina', 'Crnković', '0997878787', 'tina.crnkovic@email.com', 'Brončani'),
(17, 'Damir', 'Radoš', '0912323232', 'damir.rados@email.com', 'Zlatni'),
(18, 'Marija', 'Soldo', '0989898989', 'marija.soldo@email.com', 'Srebrni'),
(19, 'Dominik', 'Barišić', '0924545454', 'dominik.barisic@email.com', 'Brončani'),
(20, 'Lana', 'Jukić', '0958787878', 'lana.jukic@email.com', 'Zlatni'),
(21, 'Filip', 'Grgić', '0913232323', 'filip.grgic@email.com', 'Brončani'),
(22, 'Lea', 'Radić', '0986565656', 'lea.radic@email.com', 'Srebrni'),
(23, 'Dario', 'Tomić', '0999898989', 'dario.tomic@email.com', 'Srebrni'),
(24, 'Karla', 'Vuković', '0917878787', 'karla.vukovic@email.com', 'Zlatni'),
(25, 'Matej', 'Knežić', '0954545454', 'matej.knezic@email.com', 'Brončani'),
(26, 'Ines', 'Zorić', '0929090909', 'ines.zoric@email.com', 'Srebrni'),
(27, 'Zoran', 'Bubalo', '0993434343', 'zoran.bubalo@email.com', 'Brončani'),
(28, 'Tea', 'Šimić', '0911112222', 'tea.simic@email.com', 'Zlatni'),
(29, 'Boris', 'Tadić', '0956667777', 'boris.tadic@email.com', 'Srebrni'),
(30, 'Nika', 'Petrović', '0921110000', 'nika.petrovic@email.com', 'Brončani'),
(31, 'David', 'Rašić', '0982323232', 'david.rasic@email.com', 'Zlatni'),
(32, 'Mia', 'Cindrić', '0919090909', 'mia.cindric@email.com', 'Srebrni'),
(33, 'Lovro', 'Lončar', '0951010101', 'lovro.loncar@email.com', 'Brončani'),
(34, 'Nina', 'Šarić', '0928989898', 'nina.saric@email.com', 'Srebrni'),
(35, 'Stjepan', 'Dragić', '0995656565', 'stjepan.dragic@email.com', 'Zlatni'),
(36, 'Ena', 'Ilić', '0913939393', 'ena.ilic@email.com', 'Brončani'),
(37, 'Antonio', 'Šimunović', '0984747474', 'antonio.simunovic@email.com', 'Srebrni'),
(38, 'Lucija', 'Pavlović', '0927575757', 'lucija.pavlovic@email.com', 'Zlatni'),
(39, 'Viktor', 'Rajić', '0956363636', 'viktor.rajic@email.com', 'Brončani'),
(40, 'Ivana', 'Katić', '0998282828', 'ivana.katic@email.com', 'Srebrni'),
(41, 'Fran', 'Živković', '0918484848', 'fran.zivkovic@email.com', 'Zlatni'),
(42, 'Paula', 'Ribić', '0988181818', 'paula.ribic@email.com', 'Brončani'),
(43, 'Mario', 'Matošević', '0958383838', 'mario.matosevic@email.com', 'Srebrni'),
(44, 'Dorotea', 'Mikulić', '0928484848', 'dorotea.mikulic@email.com', 'Zlatni'),
(45, 'Jakov', 'Orešković', '0998585858', 'jakov.oreskovic@email.com', 'Brončani'),
(46, 'Helena', 'Vidović', '0918686868', 'helena.vidovic@email.com', 'Srebrni'),
(47, 'Krešimir', 'Janković', '0988787878', 'kresimir.jankovic@email.com', 'Zlatni'),
(48, 'Elena', 'Barić', '0958889999', 'elena.baric@email.com', 'Srebrni'),
(49, 'Sebastian', 'Lukić', '0928989890', 'sebastian.lukic@email.com', 'Brončani'),
(50, 'Tea', 'Pavlić', '0998009001', 'tea.pavlic@email.com', 'Zlatni'),
(51, 'Ana', 'Kovačević', '+385987654321', 'ana.kovacevic@email.hr', 'Zlatni'),
(52, 'Marko', 'Babić', '+385951112233', 'marko.babic@gmail.com', 'Brončani'),
(53, 'Petra', 'Marić', '+385998877665', 'petra.maric@outlook.com', 'Novi'),
(54, 'Luka', 'Jurić', '+385923344556', 'luka.juric@yahoo.com', 'Srebrni'),
(55, 'Marija', 'Novak', '+385976543210', 'marija.novak@email.com', 'Zlatni'),
(56, 'Josip', 'Knežević', '+385919876543', 'josip.knezevic@gmail.com', 'Brončani'),
(57, 'Ivana', 'Vuković', '+385981234567', 'ivana.vukovic@email.hr', 'Srebrni'),
(58, 'Tomislav', 'Matić', '+385957654321', 'tomislav.matic@outlook.com', 'Novi'),
(59, 'Kristina', 'Perić', '+385991237890', 'kristina.peric@yahoo.com', 'Srebrni'),
(60, 'Stjepan', 'Horvat', '+385918765432', 'stjepan.horvat@email.com', 'Zlatni'),
(61, 'Martina', 'Kovačić', '+385982345678', 'martina.kovacic@gmail.com', 'Brončani'),
(62, 'Filip', 'Šimić', '+385928765432', 'filip.simic@email.hr', 'Srebrni'),
(63, 'Nikolina', 'Grgić', '+385971234567', 'nikolina.grgic@outlook.com', 'Novi'),
(64, 'Ante', 'Pavlović', '+385911122334', 'ante.pavlovic@yahoo.com', 'Zlatni'),
(65, 'Jelena', 'Božić', '+385988877665', 'jelena.bozic@email.com', 'Srebrni'),
(66, 'Domagoj', 'Blažević', '+385955566778', 'domagoj.blazevic@gmail.com', 'Brončani'),
(67, 'Katarina', 'Lovrić', '+385992233445', 'katarina.lovric@email.hr', 'Novi'),
(68, 'Hrvoje', 'Radić', '+385927788990', 'hrvoje.radic@outlook.com', 'Srebrni'),
(69, 'Tea', 'Vidović', '+385973344556', 'tea.vidovic@yahoo.com', 'Zlatni'),
(70, 'Damir', 'Barišić', '+385914455667', 'damir.barisic@email.com', 'Brončani'),
(71, 'Lucija', 'Popović', '+385985566778', 'lucija.popovic@gmail.com', 'Srebrni'),
(72, 'Borna', 'Marković', '+385952233445', 'borna.markovic@email.hr', 'Novi'),
(73, 'Monika', 'Petrović', '+385997788990', 'monika.petrovic@outlook.com', 'Zlatni'),
(74, 'Dario', 'Klarić', '+385921122334', 'dario.klaric@yahoo.com', 'Srebrni'),
(75, 'Ema', 'Đurić', '+385978877665', 'ema.djuric@email.com', 'Brončani'),
(76, 'Goran', 'Sabljak', '+385915566778', 'goran.sabljak@gmail.com', 'Novi'),
(77, 'Lorena', 'Tomić', '+385982233445', 'lorena.tomic@email.hr', 'Srebrni'),
(78, 'Mihael', 'Rukavina', '+385957788990', 'mihael.rukavina@outlook.com', 'Zlatni'),
(79, 'Valentina', 'Kralj', '+385991122334', 'valentina.kralj@yahoo.com', 'Brončani'),
(80, 'Luka', 'Ključ', '+385991552371', 'luka.kljuc@yahoo.com', 'Srebrni');



INSERT INTO Stol (StolID, BrojStola, Kapacitet) VALUES
(1, 1, 2),
(2, 2, 4),
(3, 3, 6),
(4, 4, 4),
(5, 5, 8),
(6, 6, 2),
(7, 7, 4),
(8, 8, 6),
(9, 9, 4),
(10, 10, 2),
(11, 11, 4),
(12, 12, 8),
(13, 13, 6),
(14, 14, 2),
(15, 15, 4),
(16, 16, 6),
(17, 17, 4),
(18, 18, 2),
(19, 19, 8),
(20, 20, 6),
(21, 21, 2),
(22, 22, 6),
(23, 23, 4),
(24, 24, 4),
(25, 25, 2),
(26, 26, 6),
(27, 27, 10),
(28, 28, 2),
(29, 29, 4),
(30, 30, 6);



INSERT INTO Rezervacija (RezervacijaID, KupacID, StolID, DatumVrijeme, BrojOsoba, Status) VALUES
(1, 12, 5, '2025-06-01 12:00:00', 6, 'Potvrđena'),
(2, 8, 2, '2025-06-01 13:30:00', 3, 'Otkazana'),
(3, 23, 8, '2025-06-02 15:00:00', 6, 'Potvrđena'),
(4, 7, 1, '2025-06-02 18:00:00', 2, 'Potvrđena'),
(5, 15, 10, '2025-06-03 09:00:00', 2, 'Potvrđena'),
(6, 31, 4, '2025-06-03 11:00:00', 4, 'Otkazana'),
(7, 19, 6, '2025-06-03 14:00:00', 2, 'Potvrđena'),
(8, 4, 13, '2025-06-03 16:30:00', 6, 'Potvrđena'),
(9, 27, 12, '2025-06-04 12:15:00', 8, 'Potvrđena'),
(10, 13, 18, '2025-06-04 17:00:00', 2, 'Otkazana'),
(11, 44, 7, '2025-06-05 13:30:00', 4, 'Potvrđena'),
(12, 5, 14, '2025-06-05 10:00:00', 2, 'Potvrđena'),
(13, 21, 27, '2025-06-06 19:00:00', 9, 'Potvrđena'),
(14, 9, 22, '2025-06-06 20:00:00', 5, 'Otkazana'),
(15, 38, 29, '2025-06-07 08:45:00', 4, 'Potvrđena'),
(16, 33, 9, '2025-06-07 11:30:00', 4, 'Potvrđena'),
(17, 2, 11, '2025-06-08 14:00:00', 4, 'Potvrđena'),
(18, 17, 3, '2025-06-08 16:00:00', 5, 'Otkazana'),
(19, 30, 17, '2025-06-09 12:00:00', 4, 'Potvrđena'),
(20, 11, 20, '2025-06-09 18:30:00', 6, 'Potvrđena'),
(21, 50, 15, '2025-06-10 10:30:00', 3, 'Potvrđena'),
(22, 55, 20, '2025-06-10 12:45:00', 5, 'Potvrđena'),
(23, 62, 3, '2025-06-11 14:15:00', 6, 'Otkazana'),
(24, 70, 25, '2025-06-11 17:00:00', 2, 'Potvrđena'),
(25, 51, 5, '2025-06-12 09:30:00', 7, 'Potvrđena'),
(26, 68, 12, '2025-06-12 11:00:00', 8, 'Potvrđena'),
(27, 75, 1, '2025-06-13 13:00:00', 2, 'Otkazana'),
(28, 53, 19, '2025-06-13 15:30:00', 5, 'Potvrđena'),
(29, 60, 27, '2025-06-14 12:00:00', 9, 'Potvrđena'),
(30, 78, 8, '2025-06-14 18:45:00', 4, 'Potvrđena'),
(31, 25, 16, '2025-06-15 11:15:00', 5, 'Otkazana'),
(32, 35, 23, '2025-06-15 14:30:00', 3, 'Potvrđena'),
(33, 49, 6, '2025-06-16 10:00:00', 2, 'Potvrđena'),
(34, 58, 14, '2025-06-16 16:00:00', 1, 'Potvrđena'),
(35, 65, 28, '2025-06-17 19:00:00', 2, 'Otkazana'),
(36, 73, 30, '2025-06-17 20:00:00', 6, 'Potvrđena'),
(37, 29, 10, '2025-06-18 08:00:00', 2, 'Potvrđena'),
(38, 39, 21, '2025-06-18 12:30:00', 1, 'Potvrđena'),
(39, 56, 17, '2025-06-19 17:45:00', 4, 'Otkazana'),
(40, 67, 24, '2025-06-19 20:00:00', 4, 'Potvrđena');



INSERT INTO Narudzba (NarudzbaID, DatumVrijeme, KupacID, StolID, ZaposlenikID) VALUES
(1, '2025-06-01 12:04:00', 12, 5, 10),
(2, '2025-06-02 15:07:00', 23, 8, 9),
(3, '2025-06-02 18:03:00', 7, 1, 3),
(4, '2025-06-03 09:07:00', 15, 10, 8),
(5, '2025-06-03 14:05:00', 19, 6, 7),
(6, '2025-06-03 16:33:00', 4, 13, 4),
(7, '2025-06-04 12:18:00', 27, 12, 7),
(8, '2025-06-05 13:34:00', 44, 7, 8),
(9, '2025-06-05 10:06:00', 5, 14, 10),
(10, '2025-06-06 19:03:00', 21, 27, 3),
(11, '2025-06-07 08:47:00', 38, 29, 6),
(12, '2025-06-07 11:40:00', 33, 9, 3),
(13, '2025-06-08 14:07:00', 2, 11, 10),
(14, '2025-06-09 12:04:00', 30, 17, 10),
(15, '2025-06-09 18:34:00', 11, 20, 1),
(16, '2025-06-10 10:36:00', 50, 15, 3),
(17, '2025-06-10 12:51:00', 55, 20, 6),
(18, '2025-06-11 17:02:00', 70, 25, 7),
(19, '2025-06-12 09:32:00', 51, 5, 4),
(20, '2025-06-12 11:06:00', 68, 12, 3),
(21, '2025-06-13 15:34:00', 53, 19, 1),
(22, '2025-06-14 12:04:00', 60, 27, 3),
(23, '2025-06-14 18:54:00', 78, 8, 7),
(24, '2025-06-15 14:36:00', 35, 23, 3),
(25, '2025-06-16 10:04:00', 49, 6, 3),
(26, '2025-06-16 16:03:00', 58, 14, 1),
(27, '2025-06-17 20:00:00', 73, 30, 6),
(28, '2025-06-18 08:08:00', 29, 10, 1),
(29, '2025-06-18 12:30:00', 39, 21, 6),
(30, '2025-06-19 20:00:00', 67, 24, 10),
(31, '2025-06-01 12:06:00', 12, 5, 4),
(32, '2025-06-02 15:09:00', 23, 8, 5),
(33, '2025-06-02 18:06:00', 7, 1, 2),
(34, '2025-06-03 09:00:00', 15, 10, 7),
(35, '2025-06-03 14:04:00', 19, 6, 5),
(36, '2025-06-03 16:40:00', 4, 13, 10),
(37, '2025-06-04 12:22:00', 27, 12, 3),
(38, '2025-06-05 13:30:00', 44, 7, 4),
(39, '2025-06-05 10:01:00', 5, 14, 4),
(40, '2025-06-06 19:10:00', 21, 27, 3),
(41, '2025-06-07 08:49:00', 38, 29, 10),
(42, '2025-06-07 11:32:00', 33, 9, 6),
(43, '2025-06-08 14:10:00', 2, 11, 8),
(44, '2025-06-09 12:06:00', 30, 17, 2),
(45, '2025-06-09 18:36:00', 11, 20, 10),
(46, '2025-06-10 10:38:00', 50, 15, 4),
(47, '2025-06-10 12:52:00', 55, 20, 3),
(48, '2025-06-11 17:09:00', 70, 25, 3),
(49, '2025-06-12 09:38:00', 51, 5, 6),
(50, '2025-06-12 11:05:00', 68, 12, 2),
(51, '2025-06-13 15:30:00', 53, 19, 8),
(52, '2025-06-14 12:04:00', 60, 27, 7),
(53, '2025-06-14 18:51:00', 78, 8, 3),
(54, '2025-06-15 14:32:00', 35, 23, 10),
(55, '2025-06-16 10:05:00', 49, 6, 1),
(56, '2025-06-16 16:05:00', 58, 14, 10),
(57, '2025-06-17 20:08:00', 73, 30, 6),
(58, '2025-06-18 08:00:00', 29, 10, 2),
(59, '2025-06-18 12:34:00', 39, 21, 3),
(60, '2025-06-19 20:05:00', 67, 24, 3),
(61, '2025-06-01 12:02:00', 12, 5, 3),
(62, '2025-06-02 15:01:00', 23, 8, 3),
(63, '2025-06-02 18:00:00', 7, 1, 10),
(64, '2025-06-03 09:06:00', 15, 10, 7),
(65, '2025-06-03 14:09:00', 19, 6, 3),
(66, '2025-06-03 16:38:00', 4, 13, 10),
(67, '2025-06-04 12:23:00', 27, 12, 10),
(68, '2025-06-05 13:36:00', 44, 7, 9),
(69, '2025-06-05 10:05:00', 5, 14, 9),
(70, '2025-06-06 19:02:00', 21, 27, 5),
(71, '2025-06-07 08:46:00', 38, 29, 4),
(72, '2025-06-07 11:40:00', 33, 9, 2),
(73, '2025-06-08 14:08:00', 2, 11, 10),
(74, '2025-06-09 12:06:00', 30, 17, 5),
(75, '2025-06-09 18:38:00', 11, 20, 8),
(76, '2025-06-10 10:39:00', 50, 15, 10),
(77, '2025-06-10 12:54:00', 55, 20, 10),
(78, '2025-06-11 17:02:00', 70, 25, 2),
(79, '2025-06-12 09:33:00', 51, 5, 8),
(80, '2025-06-12 11:03:00', 68, 12, 4);

INSERT INTO KategorijaProizvoda (KategorijaID, NazivKategorije) VALUES
(1, 'Kava'),
(2, 'Bezalkoholna pića'),
(3, 'Alkoholna pića');

INSERT INTO Proizvod (ProizvodID, Naziv, Opis, Cijena, KategorijaID) VALUES
(1, 'Espresso', 'Kratka crna kava', 1.50, 1),
(2, 'Cappuccino', 'Kava s mlijekom i pjenom', 2.00, 1),
(3, 'Sok od naranče', 'Prirodni sok', 2.50, 2),
(4, 'Macchiato', 'Espresso s malo mlijeka', 1.80, 1),
(5, 'Latte', 'Kava s puno toplog mlijeka', 2.20, 1),
(6, 'Americano', 'Espresso razrijeđen s vodom', 1.70, 1),
(7, 'Mocha', 'Kava s čokoladom i mlijekom', 2.50, 1),
(8, 'Turska kava', 'Tradicionalna kava iz džezve', 1.60, 1),
(9, 'Ledena kava', 'Hladna kava s mlijekom i ledom', 2.80, 1),
(10, 'Coca-Cola', 'Gazirani napitak', 2.50, 2),
(11, 'Fanta', 'Gazirani napitak od naranče', 2.50, 2),
(12, 'Sprite', 'Gazirani napitak od limuna', 2.50, 2),
(13, 'Iced Tea breskva', 'Hladni čaj s okusom breskve', 2.30, 2),
(14, 'Cedevita', 'Vitaminski napitak', 2.20, 2),
(15, 'Mineralna voda', 'Prirodna gazirana voda', 1.80, 2),
(16, 'Heineken', 'Pivo u boci 0.5L', 3.00, 3),
(17, 'Jack Daniels', 'Američki Tennessee viski', 5.50, 3),
(18, 'Jameson', 'Irski viski', 5.00, 3),
(19, 'Ballantine', 'Škotski blended viski', 4.80, 3),
(20, 'Tanqueray Gin', 'Suhi gin iz Londona', 5.20, 3),
(21, 'Bombay Sapphire', 'Aromatični londonski gin', 5.40, 3),
(22, 'Absolut Vodka', 'Švedska vodka', 4.50, 3),
(23, 'Smirnoff', 'Klasična vodka', 4.30, 3),
(24, 'Pelinkovac', 'Biljni liker popularan u regiji', 3.00, 3);


INSERT INTO StavkaNarudzbe (NarudzbaID, ProizvodID, Kolicina, JedinicnaCijena) VALUES
(1, 5, 2, 2.20),
(1, 15, 2, 1.80),
(1, 18, 2, 5.00),
(2, 1, 1, 1.50),
(2, 19, 1, 4.80),
(3, 22, 1, 4.50),
(3, 9, 3, 2.80),
(3, 11, 3, 2.50),
(4, 19, 2, 4.80),
(4, 15, 2, 1.80),
(5, 18, 2, 5.00),
(5, 21, 3, 5.40),
(5, 23, 1, 4.30),
(6, 2, 1, 2.00),
(6, 4, 2, 1.80),
(7, 13, 3, 2.30),
(7, 3, 2, 2.50),
(8, 10, 3, 2.50),
(8, 23, 1, 4.30),
(9, 4, 1, 1.80),
(9, 12, 2, 2.50),
(9, 3, 2, 2.50),
(9, 21, 1, 5.40),
(10, 24, 2, 3.00),
(10, 2, 1, 2.00),
(11, 11, 3, 2.50),
(11, 16, 2, 3.00),
(11, 6, 2, 1.70),
(12, 14, 2, 2.20),
(12, 4, 3, 1.80),
(13, 9, 2, 2.80),
(13, 24, 1, 3.00),
(13, 15, 3, 1.80),
(13, 22, 1, 4.50),
(14, 12, 2, 2.50),
(14, 4, 3, 1.80),
(14, 22, 3, 4.50),
(14, 5, 1, 2.20),
(15, 2, 1, 2.00),
(15, 22, 1, 4.50),
(15, 17, 3, 5.50),
(16, 12, 2, 2.50),
(16, 18, 3, 5.00),
(16, 17, 1, 5.50),
(17, 1, 3, 1.50),
(17, 12, 2, 2.50),
(17, 24, 2, 3.00),
(18, 22, 2, 4.50),
(18, 6, 2, 1.70),
(18, 23, 2, 4.30),
(18, 4, 2, 1.80),
(19, 16, 3, 3.00),
(19, 14, 3, 2.20),
(19, 6, 2, 1.70),
(20, 23, 1, 4.30),
(20, 2, 1, 2.00),
(20, 5, 1, 2.20),
(20, 13, 1, 2.30),
(21, 16, 2, 3.00),
(21, 10, 2, 2.50),
(21, 12, 1, 2.50),
(22, 14, 2, 2.20),
(22, 10, 3, 2.50),
(22, 1, 1, 1.50),
(23, 14, 2, 2.20),
(23, 15, 1, 1.80),
(23, 23, 1, 4.30),
(24, 13, 2, 2.30),
(24, 10, 3, 2.50),
(24, 11, 3, 2.50),
(24, 24, 3, 3.00),
(25, 12, 2, 2.50),
(25, 20, 1, 5.20),
(25, 21, 1, 5.40),
(25, 16, 1, 3.00),
(26, 18, 2, 5.00),
(26, 10, 2, 2.50),
(26, 7, 1, 2.50),
(27, 19, 1, 4.80),
(27, 18, 3, 5.00),
(27, 16, 2, 3.00),
(27, 22, 3, 4.50),
(28, 11, 1, 2.50),
(28, 19, 1, 4.80),
(28, 13, 2, 2.30),
(29, 23, 2, 4.30),
(29, 6, 1, 1.70),
(29, 12, 2, 2.50),
(29, 3, 2, 2.50),
(30, 18, 1, 5.00),
(30, 19, 3, 4.80),
(30, 14, 2, 2.20),
(30, 10, 2, 2.50),
(31, 11, 3, 2.50),
(31, 13, 1, 2.30),
(31, 12, 2, 2.50),
(32, 23, 3, 4.30),
(32, 21, 3, 5.40),
(32, 8, 2, 1.60),
(32, 11, 1, 2.50),
(33, 7, 1, 2.50),
(33, 16, 2, 3.00),
(33, 5, 3, 2.20),
(34, 18, 2, 5.00),
(34, 14, 1, 2.20),
(34, 7, 3, 2.50),
(34, 8, 2, 1.60),
(35, 20, 1, 5.20),
(35, 9, 1, 2.80),
(35, 24, 3, 3.00),
(35, 11, 1, 2.50),
(36, 21, 1, 5.40),
(36, 13, 2, 2.30),
(37, 6, 1, 1.70),
(37, 13, 1, 2.30),
(37, 4, 3, 1.80),
(38, 4, 2, 1.80),
(38, 13, 1, 2.30),
(38, 15, 1, 1.80),
(39, 8, 2, 1.60),
(39, 5, 1, 2.20),
(40, 22, 1, 4.50),
(40, 20, 3, 5.20),
(40, 15, 2, 1.80),
(41, 11, 3, 2.50),
(41, 2, 2, 2.00),
(41, 14, 1, 2.20),
(42, 1, 2, 1.50),
(42, 17, 1, 5.50),
(43, 23, 3, 4.30),
(43, 15, 2, 1.80),
(43, 16, 1, 3.00),
(43, 4, 2, 1.80),
(44, 24, 1, 3.00),
(44, 7, 3, 2.50),
(44, 21, 3, 5.40),
(45, 23, 2, 4.30),
(45, 15, 2, 1.80),
(46, 15, 3, 1.80),
(46, 5, 1, 2.20),
(47, 23, 1, 4.30),
(47, 7, 3, 2.50),
(48, 12, 3, 2.50),
(48, 2, 1, 2.00),
(48, 21, 1, 5.40),
(48, 3, 2, 2.50),
(49, 10, 3, 2.50),
(49, 4, 2, 1.80),
(49, 1, 2, 1.50),
(49, 16, 2, 3.00),
(50, 8, 3, 1.60),
(50, 21, 2, 5.40),
(50, 23, 2, 4.30),
(50, 20, 3, 5.20),
(51, 20, 1, 5.20),
(51, 4, 3, 1.80),
(51, 1, 2, 1.50),
(52, 23, 3, 4.30),
(52, 13, 3, 2.30),
(53, 23, 3, 4.30),
(53, 3, 1, 2.50),
(53, 22, 3, 4.50),
(53, 8, 1, 1.60),
(54, 8, 1, 1.60),
(54, 18, 1, 5.00),
(54, 9, 3, 2.80),
(55, 3, 2, 2.50),
(55, 12, 1, 2.50),
(55, 13, 2, 2.30),
(56, 3, 2, 2.50),
(56, 8, 3, 1.60),
(56, 16, 2, 3.00),
(57, 1, 2, 1.50),
(57, 2, 1, 2.00),
(58, 18, 1, 5.00),
(58, 12, 2, 2.50),
(58, 9, 1, 2.80),
(58, 22, 2, 4.50),
(59, 6, 3, 1.70),
(59, 8, 2, 1.60),
(60, 22, 3, 4.50),
(60, 4, 3, 1.80),
(60, 15, 2, 1.80),
(60, 19, 1, 4.80),
(61, 17, 1, 5.50),
(61, 2, 1, 2.00),
(61, 24, 2, 3.00),
(61, 14, 3, 2.20),
(62, 3, 2, 2.50),
(62, 8, 3, 1.60),
(62, 7, 2, 2.50),
(62, 10, 3, 2.50),
(63, 22, 2, 4.50),
(63, 20, 3, 5.20),
(63, 15, 2, 1.80),
(63, 19, 1, 4.80),
(64, 21, 2, 5.40),
(64, 11, 1, 2.50),
(65, 15, 3, 1.80),
(65, 8, 3, 1.60),
(66, 14, 3, 2.20),
(66, 18, 2, 5.00),
(67, 11, 3, 2.50),
(67, 19, 1, 4.80),
(67, 6, 3, 1.70),
(68, 4, 2, 1.80),
(68, 3, 2, 2.50),
(69, 8, 1, 1.60),
(69, 1, 2, 1.50),
(69, 12, 1, 2.50),
(69, 17, 1, 5.50),
(70, 2, 2, 2.00),
(70, 10, 3, 2.50),
(70, 14, 2, 2.20),
(70, 4, 2, 1.80),
(71, 4, 1, 1.80),
(71, 10, 3, 2.50),
(71, 13, 1, 2.30),
(71, 8, 2, 1.60),
(72, 10, 2, 2.50),
(72, 11, 3, 2.50),
(73, 23, 3, 4.30),
(73, 17, 2, 5.50),
(73, 4, 1, 1.80),
(73, 16, 3, 3.00),
(74, 15, 1, 1.80),
(74, 22, 3, 4.50),
(75, 14, 2, 2.20),
(75, 11, 1, 2.50),
(75, 22, 3, 4.50),
(76, 1, 3, 1.50),
(76, 8, 2, 1.60),
(77, 5, 3, 2.20),
(77, 19, 1, 4.80),
(77, 7, 3, 2.50),
(77, 6, 1, 1.70),
(78, 6, 2, 1.70),
(78, 13, 3, 2.30),
(78, 11, 2, 2.50),
(78, 20, 3, 5.20),
(79, 22, 1, 4.50),
(79, 18, 3, 5.00),
(79, 12, 2, 2.50),
(80, 6, 1, 1.70),
(80, 13, 3, 2.30),
(80, 24, 2, 3.00),
(80, 19, 1, 4.80);


INSERT INTO Placanje (PlacanjeID, NarudzbaID, Iznos, NacinPlacanja, DatumVrijeme) VALUES
(1, 1, 18.00, 'Gotovina', '2025-06-01 13:26:00'),
(2, 2, 6.30, 'Kartica', '2025-06-02 16:23:00'),
(3, 3, 20.40, 'Kartica', '2025-06-02 19:03:00'),
(4, 4, 13.20, 'Kartica', '2025-06-03 09:52:00'),
(5, 5, 30.50, 'Kartica', '2025-06-03 14:23:00'),
(6, 6, 5.60, 'Kartica', '2025-06-03 17:47:00'),
(7, 7, 11.90, 'Kartica', '2025-06-04 12:29:00'),
(8, 8, 11.80, 'Kartica', '2025-06-05 14:37:00'),
(9, 9, 17.20, 'Gotovina', '2025-06-05 10:21:00'),
(10, 10, 8.00, 'Gotovina', '2025-06-06 20:00:00'),
(11, 11, 16.90, 'Gotovina', '2025-06-07 10:06:00'),
(12, 12, 9.80, 'Kartica', '2025-06-07 13:07:00'),
(13, 13, 18.50, 'Gotovina', '2025-06-08 14:24:00'),
(14, 14, 26.10, 'Kartica', '2025-06-09 12:25:00'),
(15, 15, 23.00, 'Gotovina', '2025-06-09 19:33:00'),
(16, 16, 25.50, 'Kartica', '2025-06-10 11:29:00'),
(17, 17, 15.50, 'Kartica', '2025-06-10 13:09:00'),
(18, 18, 24.60, 'Kartica', '2025-06-11 17:37:00'),
(19, 19, 19.00, 'Gotovina', '2025-06-12 10:02:00'),
(20, 20, 10.80, 'Gotovina', '2025-06-12 12:11:00'),
(21, 21, 13.50, 'Gotovina', '2025-06-13 17:00:00'),
(22, 22, 13.40, 'Kartica', '2025-06-14 13:11:00'),
(23, 23, 10.50, 'Gotovina', '2025-06-14 19:29:00'),
(24, 24, 28.60, 'Kartica', '2025-06-15 15:20:00'),
(25, 25, 18.60, 'Gotovina', '2025-06-16 11:32:00'),
(26, 26, 17.50, 'Gotovina', '2025-06-16 16:33:00'),
(27, 27, 39.30, 'Kartica', '2025-06-17 20:35:00'),
(28, 28, 11.90, 'Kartica', '2025-06-18 08:55:00'),
(29, 29, 20.30, 'Kartica', '2025-06-18 13:02:00'),
(30, 30, 28.80, 'Kartica', '2025-06-19 20:40:00'),
(31, 31, 14.80, 'Gotovina', '2025-06-01 12:31:00'),
(32, 32, 34.80, 'Kartica', '2025-06-02 16:13:00'),
(33, 33, 15.10, 'Kartica', '2025-06-02 18:31:00'),
(34, 34, 22.90, 'Gotovina', '2025-06-03 10:24:00'),
(35, 35, 19.50, 'Gotovina', '2025-06-03 15:00:00'),
(36, 36, 10.00, 'Kartica', '2025-06-03 18:07:00'),
(37, 37, 9.40, 'Gotovina', '2025-06-04 12:34:00'),
(38, 38, 7.70, 'Gotovina', '2025-06-05 14:25:00'),
(39, 39, 5.40, 'Gotovina', '2025-06-05 11:02:00'),
(40, 40, 23.70, 'Gotovina', '2025-06-06 19:57:00'),
(41, 41, 13.70, 'Gotovina', '2025-06-07 10:01:00'),
(42, 42, 8.50, 'Kartica', '2025-06-07 12:15:00'),
(43, 43, 23.10, 'Gotovina', '2025-06-08 15:32:00'),
(44, 44, 26.70, 'Gotovina', '2025-06-09 12:46:00'),
(45, 45, 12.20, 'Gotovina', '2025-06-09 19:55:00'),
(46, 46, 7.60, 'Gotovina', '2025-06-10 11:52:00'),
(47, 47, 11.80, 'Kartica', '2025-06-10 14:05:00'),
(48, 48, 19.90, 'Gotovina', '2025-06-11 18:20:00'),
(49, 49, 20.10, 'Kartica', '2025-06-12 09:59:00'),
(50, 50, 39.80, 'Kartica', '2025-06-12 11:10:00'),
(51, 51, 13.60, 'Kartica', '2025-06-13 15:39:00'),
(52, 52, 19.80, 'Gotovina', '2025-06-14 12:43:00'),
(53, 53, 30.50, 'Kartica', '2025-06-14 19:05:00'),
(54, 54, 15.00, 'Gotovina', '2025-06-15 15:06:00'),
(55, 55, 12.10, 'Kartica', '2025-06-16 11:04:00'),
(56, 56, 15.80, 'Gotovina', '2025-06-16 17:07:00'),
(57, 57, 5.00, 'Kartica', '2025-06-17 20:55:00'),
(58, 58, 21.80, 'Gotovina', '2025-06-18 08:42:00'),
(59, 59, 8.30, 'Gotovina', '2025-06-18 13:40:00'),
(60, 60, 27.30, 'Gotovina', '2025-06-19 21:13:00'),
(61, 61, 20.10, 'Kartica', '2025-06-01 13:31:00'),
(62, 62, 22.30, 'Gotovina', '2025-06-02 15:24:00'),
(63, 63, 33.00, 'Gotovina', '2025-06-02 18:14:00'),
(64, 64, 13.30, 'Kartica', '2025-06-03 09:31:00'),
(65, 65, 10.20, 'Kartica', '2025-06-03 15:24:00'),
(66, 66, 16.60, 'Gotovina', '2025-06-03 17:59:00'),
(67, 67, 17.40, 'Gotovina', '2025-06-04 13:04:00'),
(68, 68, 8.60, 'Kartica', '2025-06-05 14:49:00'),
(69, 69, 12.60, 'Kartica', '2025-06-05 11:29:00'),
(70, 70, 19.50, 'Gotovina', '2025-06-06 19:40:00'),
(71, 71, 14.80, 'Kartica', '2025-06-07 09:01:00'),
(72, 72, 12.50, 'Kartica', '2025-06-07 12:57:00'),
(73, 73, 34.70, 'Gotovina', '2025-06-08 14:28:00'),
(74, 74, 15.30, 'Gotovina', '2025-06-09 13:11:00'),
(75, 75, 20.40, 'Gotovina', '2025-06-09 19:15:00'),
(76, 76, 7.70, 'Kartica', '2025-06-10 11:16:00'),
(77, 77, 20.60, 'Gotovina', '2025-06-10 13:18:00'),
(78, 78, 30.90, 'Kartica', '2025-06-11 18:00:00'),
(79, 79, 24.50, 'Kartica', '2025-06-12 09:55:00'),
(80, 80, 19.40, 'Kartica', '2025-06-12 11:30:00');


# MISLIM DA NIJE DOBRO
INSERT INTO Dobavljac (DobavljacID, Naziv, Telefon, Email) VALUES
(1, 'Franck d.d.', '+38512340001', 'info@franck.hr'),               -- za kave
(2, 'Barcaffe d.o.o.', '+38512340002', 'prodaja@barcaffe.hr'),      -- za kave
(3, 'Coca-Cola HBC Hrvatska', '+38512340003', 'kontakt@coca-cola.hr'), -- Coca-Cola, Fanta, Sprite
(4, 'Jamnica plus d.o.o.', '+38512340004', 'info@jamnica.hr'),      -- voda, iced tea
(5, 'Atlantic Grupa', '+38512340005', 'cedevita@atlantic.hr'),      -- Cedevita
(6, 'Heineken Hrvatska d.o.o.', '+38512340006', 'heineken@hr.com'), -- Heineken
(7, 'Badel 1862 d.d.', '+38512340007', 'badel@badel1862.hr'),       -- Pelinkovac, Smirnoff, Absolut
(8, 'Pernod Ricard Hrvatska', '+38512340008', 'pr.hr@pernod-ricard.com'), -- Viski, gin
(9, 'Mljekara Latus d.o.o.', '+38552662255', 'info@mljekaralatus.hr'),
(10, 'Zelena Dolina Voće i Povrće', '+38552777888', 'narudzbe@zelenadolina-vp.hr'),
(11, 'SveZaUgostiteljstvo d.o.o.', '+38514555666', 'prodaja@svezaUgostiteljstvo.hr'),
(12, 'Ledo plus d.o.o.', '+38512385555', 'info@ledo.hr'),
(13, 'Higijena Standard d.o.o.', '+38516543210', 'kontakt@higijena-standard.hr');

INSERT INTO ProizvodDobavljac (ProizvodID, DobavljacID, RokIsporuke) VALUES
(1, 1, '2025-06-03 08:00:00'),  -- +2 dana -- Espresso - Franck
(1, 2, '2025-06-04 08:00:00'),  -- +3 dana -- Espresso - Barcaffe
(2, 1, '2025-06-03 08:00:00'),  -- +2 dana  -- Cappuccino - Franck
(3, 4, '2025-06-04 08:00:00'),  -- +3 dana  -- Sok od naranče - Jamnica
(4, 2, '2025-06-03 08:00:00'),  -- +2 dana -- Macchiato - Barcaffe
(5, 1, '2025-06-04 08:00:00'),  -- +3 dana -- Latte - Franck
(6, 2, '2025-06-03 08:00:00'),  -- +2 dana
(7, 1, '2025-06-05 08:00:00'),  -- +4 dana
(8, 2, '2025-06-04 08:00:00'),  -- +3 dana
(9, 1, '2025-06-04 08:00:00'),  -- +3 dana
-- Gazirana pića (Coca-Cola HBC)
(10, 3, '2025-06-03 08:00:00'), -- Coca-Cola
(11, 3, '2025-06-03 08:00:00'), -- Fanta
(12, 3, '2025-06-03 08:00:00'), -- Sprite
-- Ostala bezalkoholna (Jamnica i Atlantic Grupa)
(13, 4, '2025-06-10 08:00:00'), -- Iced Tea breskva - Jamnica
(14, 5, '2025-06-05 08:00:00'), -- Cedevita - Atlantic
(15, 4, '2025-06-08 08:00:00'), -- Mineralna voda - Jamnica
-- Alkoholna pića
(16, 6, '2025-06-06 08:00:00'), -- Heineken - Heineken Hrvatska
(17, 8, '2025-06-07 08:00:00'), -- Jack Daniels - Pernod Ricard
(18, 8, '2025-06-06 08:00:00'), -- Jameson - Pernod Ricard
(19, 8, '2025-06-06 08:00:00'), -- Ballantine - Pernod Ricard
(20, 8, '2025-06-04 08:00:00'), -- Tanqueray Gin - Pernod Ricard
(21, 8, '2025-06-09 08:00:00'), -- Bombay Sapphire - Pernod Ricard
(22, 7, '2025-06-08 08:00:00'), -- Absolut - Badel
(23, 7, '2025-06-10 08:00:00'), -- Smirnoff - Badel
(24, 7, '2025-06-10 08:00:00'); -- Pelinkovac - Badel




INSERT INTO Sirovina (SirovinaID, Naziv, JedinicaMjere) VALUES
(1, 'Kava u zrnu (premium blend)', 'kg'),
(2, 'Mlijeko (svježe, 3.8% m.m.)', 'l'),
(3, 'Šećer (bijeli kristal)', 'kg'),
(4, 'Led (kockice)', 'kg'),
(5, 'Naranče (za cijeđenje)', 'kg'),
(6, 'Limuni (svježi)', 'kg'),
(7, 'Kakao prah (za napitke i posip)', 'kg'),
(8, 'Cedevita prah (okus naranča)', 'kg'),
(9, 'Cedevita prah (okus limun)', 'kg'),
(10, 'Sirup od breskve (za ledeni čaj)', 'l'),
(11, 'Šlag pjena (u dozi)', 'kom'),
(12, 'Slamke (biorazgradive/papirnate)', 'kom'),
(13, 'Salvete (papirnate, jednoslojne)', 'pak'),
(14, 'Šećer u vrećicama (bijeli)', 'kom'),
(15, 'Šećer u vrećicama (smeđi)', 'kom'),
(16, 'Med (monodoza)', 'kom'),
(17, 'Keksići (uz kavu, razni)', 'pak'),
(18, 'Sirup od vanilije (za kavu)', 'l'),
(19, 'Sirup od karamele (za kavu)', 'l'),
(20, 'Sirup od lješnjaka (za kavu)', 'l'),
(21, 'Tonik voda (za miješanje)', 'l'),
(22, 'Sok od brusnice (za miješanje)', 'l'),
(23, 'Miješalice za kavu (drvene)', 'kom'),
(24, 'Sredstvo za čišćenje površina (dezinfekcijsko)', 'l'),
(25, 'Deterdžent za strojno pranje suđa', 'l'),
(26, 'Papirnati ručnici (u roli)', 'kom'),
(27, 'Vreće za smeće (razne veličine)', 'rola'),
(28, 'Tekući sapun za ruke (antibakterijski)', 'l'),
(29, 'Mlijeko (trajno, za zalihe)', 'l');

INSERT INTO ZalihaSirovina (SirovinaID, KolicinaNaSkladistu, GranicaNarudzbe) VALUES
(1, 9.8, 4.0),
(2, 31.5, 5.3),
(3, 20.5, 3.4),
(4, 28, 7),
(5, 19.0, 4.4),
(6, 7.3, 1.0),
(7, 1.96, 0.37),
(8, 2.8, 0.9),
(9, 4.5, 0.8),
(10, 5, 2),
(11, 15, 3),
(12, 911, 293),
(13, 28, 7),
(14, 1311, 335),
(15, 328, 129),
(16, 494, 85),
(17, 14, 7),
(18, 2, 1),
(19, 3, 1),
(20, 3, 1),
(21, 16, 5),
(22, 13, 2),
(23, 1880, 448),
(24, 4, 2),
(25, 4, 2),
(26, 7, 5),
(27, 6, 2),
(28, 3, 2),
(29, 21, 6);

INSERT INTO NabavnaNarudzba (NabavnaNarudzbaID, DobavljacID, DatumNarudzbe, OcekivaniDatum) VALUES
(1, 2, '2025-03-04 14:24:20', '2025-03-09 17:00:00'),
(2, 12, '2025-01-10 11:28:51', '2025-01-17 17:00:00'),
(3, 6, '2025-01-13 15:23:39', '2025-01-16 17:00:00'),
(4, 2, '2025-03-13 09:40:38', '2025-03-18 17:00:00'),
(5, 6, '2025-02-24 11:11:17', '2025-03-02 17:00:00'),
(6, 4, '2025-01-17 16:02:58', '2025-01-21 17:00:00'),
(7, 3, '2025-01-19 14:52:08', '2025-01-25 17:00:00'),
(8, 1, '2025-04-06 09:01:45', '2025-04-12 17:00:00'),
(9, 5, '2025-04-19 15:20:45', '2025-04-22 17:00:00'),
(10, 10, '2025-01-05 14:58:20', '2025-01-12 17:00:00'),
(11, 12, '2025-03-27 13:54:37', '2025-04-01 17:00:00'),
(12, 8, '2025-02-08 12:35:29', '2025-02-13 17:00:00'),
(13, 13, '2025-02-03 12:17:28', '2025-02-05 17:00:00'),
(14, 6, '2025-03-27 11:03:07', '2025-03-31 17:00:00'),
(15, 1, '2025-04-28 15:34:15', '2025-05-04 17:00:00'),
(16, 13, '2025-01-19 09:37:09', '2025-01-24 17:00:00'),
(17, 8, '2025-03-28 12:48:29', '2025-04-04 17:00:00'),
(18, 8, '2025-01-09 10:38:52', '2025-01-15 17:00:00'),
(19, 10, '2025-02-06 11:58:31', '2025-02-12 17:00:00'),
(20, 6, '2025-04-30 14:17:16', '2025-05-05 17:00:00'),
(21, 3, '2025-03-07 10:12:02', '2025-03-10 17:00:00'),
(22, 10, '2025-02-14 12:48:49', '2025-02-18 17:00:00'),
(23, 13, '2025-04-14 09:48:12', '2025-04-21 17:00:00'),
(24, 5, '2025-05-22 14:04:57', '2025-05-25 17:00:00'),
(25, 4, '2025-05-18 13:18:14', '2025-05-21 17:00:00'),
(26, 13, '2025-04-12 10:20:13', '2025-04-18 17:00:00'),
(27, 5, '2025-03-14 12:42:48', '2025-03-21 17:00:00'),
(28, 3, '2025-05-09 15:13:11', '2025-05-15 17:00:00'),
(29, 12, '2025-04-08 09:09:57', '2025-04-10 17:00:00'),
(30, 7, '2025-04-30 11:53:26', '2025-05-07 17:00:00');



INSERT INTO StavkaNabavneNarudzbe (NabavnaNarudzbaID, SirovinaID, Kolicina) VALUES
(1, 1, 7),
(2, 4, 88),
(4, 1, 8),
(6, 21, 32),
(7, 21, 32),
(8, 1, 14),
(9, 9, 3),
(9, 8, 4),
(10, 5, 29),
(10, 6, 7),
(11, 4, 68),
(12, 21, 20),
(12, 22, 6),
(13, 26, 22),
(13, 25, 2),
(13, 24, 3),
(13, 27, 13),
(15, 1, 11),
(16, 25, 1),
(16, 27, 13),
(17, 21, 43),
(17, 22, 8),
(18, 21, 40),
(19, 6, 14),
(21, 21, 46),
(22, 5, 23),
(22, 6, 9),
(23, 27, 14),
(23, 25, 3),
(23, 28, 10),
(23, 24, 5),
(24, 9, 4),
(25, 10, 5),
(25, 21, 42),
(25, 22, 10),
(26, 27, 17),
(26, 26, 23),
(27, 8, 5),
(27, 9, 10),
(28, 21, 47),
(29, 4, 71);

INSERT INTO Smjena (SmjenaID, ZaposlenikID, DatumVrijemePocetka, DatumVrijemeZavrsetka) VALUES
(1, 3, '2025-06-01 07:00:00', '2025-06-01 15:00:00'),
(2, 4, '2025-06-01 07:00:00', '2025-06-01 15:00:00'),
(3, 10, '2025-06-01 07:00:00', '2025-06-01 15:00:00'),
(4, 2, '2025-06-02 14:00:00', '2025-06-02 22:00:00'),
(5, 3, '2025-06-02 14:00:00', '2025-06-02 22:00:00'),
(6, 5, '2025-06-02 14:00:00', '2025-06-02 22:00:00'),
(7, 9, '2025-06-02 14:00:00', '2025-06-02 22:00:00'),
(8, 10, '2025-06-02 14:00:00', '2025-06-02 22:00:00'),
(9, 3, '2025-06-03 07:00:00', '2025-06-03 15:00:00'),
(10, 5, '2025-06-03 07:00:00', '2025-06-03 15:00:00'),
(11, 7, '2025-06-03 07:00:00', '2025-06-03 15:00:00'),
(12, 8, '2025-06-03 07:00:00', '2025-06-03 15:00:00'),
(13, 3, '2025-06-03 14:00:00', '2025-06-03 22:00:00'),
(14, 4, '2025-06-03 14:00:00', '2025-06-03 22:00:00'),
(15, 5, '2025-06-03 14:00:00', '2025-06-03 22:00:00'),
(16, 7, '2025-06-03 14:00:00', '2025-06-03 22:00:00'),
(17, 10, '2025-06-03 14:00:00', '2025-06-03 22:00:00'),
(18, 3, '2025-06-04 07:00:00', '2025-06-04 15:00:00'),
(19, 7, '2025-06-04 07:00:00', '2025-06-04 15:00:00'),
(20, 10, '2025-06-04 07:00:00', '2025-06-04 15:00:00'),
(21, 4, '2025-06-05 07:00:00', '2025-06-05 15:00:00'),
(22, 8, '2025-06-05 07:00:00', '2025-06-05 15:00:00'),
(23, 9, '2025-06-05 07:00:00', '2025-06-05 15:00:00'),
(24, 10, '2025-06-05 07:00:00', '2025-06-05 15:00:00'),
(25, 3, '2025-06-06 14:00:00', '2025-06-06 22:00:00'),
(26, 5, '2025-06-06 14:00:00', '2025-06-06 22:00:00'),
(27, 2, '2025-06-07 07:00:00', '2025-06-07 15:00:00'),
(28, 3, '2025-06-07 07:00:00', '2025-06-07 15:00:00'),
(29, 4, '2025-06-07 07:00:00', '2025-06-07 15:00:00'),
(30, 6, '2025-06-07 07:00:00', '2025-06-07 15:00:00'),
(31, 10, '2025-06-07 07:00:00', '2025-06-07 15:00:00'),
(32, 8, '2025-06-08 07:00:00', '2025-06-08 15:00:00'),
(33, 10, '2025-06-08 07:00:00', '2025-06-08 15:00:00'),
(34, 8, '2025-06-08 14:00:00', '2025-06-08 22:00:00'),
(35, 10, '2025-06-08 14:00:00', '2025-06-08 22:00:00'),
(36, 2, '2025-06-09 07:00:00', '2025-06-09 15:00:00'),
(37, 5, '2025-06-09 07:00:00', '2025-06-09 15:00:00'),
(38, 10, '2025-06-09 07:00:00', '2025-06-09 15:00:00'),
(39, 1, '2025-06-09 14:00:00', '2025-06-09 22:00:00'),
(40, 8, '2025-06-09 14:00:00', '2025-06-09 22:00:00'),
(41, 10, '2025-06-09 14:00:00', '2025-06-09 22:00:00'),
(42, 3, '2025-06-10 07:00:00', '2025-06-10 15:00:00'),
(43, 4, '2025-06-10 07:00:00', '2025-06-10 15:00:00'),
(44, 6, '2025-06-10 07:00:00', '2025-06-10 15:00:00'),
(45, 10, '2025-06-10 07:00:00', '2025-06-10 15:00:00'),
(46, 2, '2025-06-11 14:00:00', '2025-06-11 22:00:00'),
(47, 3, '2025-06-11 14:00:00', '2025-06-11 22:00:00'),
(48, 7, '2025-06-11 14:00:00', '2025-06-11 22:00:00'),
(49, 2, '2025-06-12 07:00:00', '2025-06-12 15:00:00'),
(50, 3, '2025-06-12 07:00:00', '2025-06-12 15:00:00'),
(51, 4, '2025-06-12 07:00:00', '2025-06-12 15:00:00'),
(52, 6, '2025-06-12 07:00:00', '2025-06-12 15:00:00'),
(53, 8, '2025-06-12 07:00:00', '2025-06-12 15:00:00'),
(54, 1, '2025-06-13 14:00:00', '2025-06-13 22:00:00'),
(55, 8, '2025-06-13 14:00:00', '2025-06-13 22:00:00'),
(56, 3, '2025-06-14 07:00:00', '2025-06-14 15:00:00'),
(57, 7, '2025-06-14 07:00:00', '2025-06-14 15:00:00'),
(58, 3, '2025-06-14 14:00:00', '2025-06-14 22:00:00'),
(59, 7, '2025-06-14 14:00:00', '2025-06-14 22:00:00'),
(60, 3, '2025-06-15 07:00:00', '2025-06-15 15:00:00'),
(61, 10, '2025-06-15 07:00:00', '2025-06-15 15:00:00'),
(62, 3, '2025-06-15 14:00:00', '2025-06-15 22:00:00'),
(63, 10, '2025-06-15 14:00:00', '2025-06-15 22:00:00'),
(64, 1, '2025-06-16 07:00:00', '2025-06-16 15:00:00'),
(65, 3, '2025-06-16 07:00:00', '2025-06-16 15:00:00'),
(66, 1, '2025-06-16 14:00:00', '2025-06-16 22:00:00'),
(67, 10, '2025-06-16 14:00:00', '2025-06-16 22:00:00'),
(68, 6, '2025-06-17 14:00:00', '2025-06-17 22:00:00'),
(69, 1, '2025-06-18 07:00:00', '2025-06-18 15:00:00'),
(70, 2, '2025-06-18 07:00:00', '2025-06-18 15:00:00'),
(71, 3, '2025-06-18 07:00:00', '2025-06-18 15:00:00'),
(72, 6, '2025-06-18 07:00:00', '2025-06-18 15:00:00'),
(73, 3, '2025-06-19 14:00:00', '2025-06-19 22:00:00'),
(74, 10, '2025-06-19 14:00:00', '2025-06-19 22:00:00');
```
