## UVOD

U priloženoj dokumentaciji prezentirat ćemo naš projektni zadatak iz kolegija Baze podataka 1. Cilj projekta je razviti funkcionalan model baze podataka koji omogućuje učinkovito upravljanje različitim aspektima rada kafića, uključujući narudžbe, rezervacije, zaposlenike, zalihe sirovina i financijske transakcije. 
Kao prvi korak izrade baze podataka odlučili smo generirati vlastite tablice i podatke, kako bismo pojednostavili unos točnih podataka i izbjegli oslanjanje na vanjske izvore.
## Opis ER dijagrama

[Link na ER dijagram](https://lucid.app/lucidchart/7e3ca596-78ec-4f8d-9e66-618cb6cf1f40/edit?viewport_loc=-2689%2C-743%2C4235%2C1887%2C0_0&invitationId=inv_76bfcfcb-73cd-451d-8128-f57a1b90cb83)

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
Evidentira podatke o kupcima, sastoji od sljedećih atributa:

- *kupacID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije
- *ime* – podatak tipa VARCHAR(255), koji ne smije biti NULL
- *prezime* – podatak tipa VARCHAR(255), koji ne smije biti NULL
- *telefon* – podatak tipa VARCHAR(255)
- *email* – podatak tipa VARCHAR(255), koji mora biti jedinstven (UNIQUE)
- *statusVjernosti* – podatak tipa VARCHAR(255), označava status lojalnosti kupca (npr. standardni, zlatni, VIP)

Ograničenje NOT NULL označava da podaci ime i prezime moraju biti uneseni. Atribut email je dodatno ograničen kao jedinstven, čime se sprječava da više kupaca koristi istu e-mail adresu.

![Screenshot 2025-05-26 184714](https://github.com/user-attachments/assets/086bf875-16fa-44cc-a705-26f5cb7b3344)


Relacija **zaposlenik**\
Evidentira podatke o zaposlenicima, sastoji od sljedećih atributa:

- *zaposlenikID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije
- *ime* – podatak tipa VARCHAR(255), koji ne smije biti null
- *prezime* – podatak tipa VARCHAR(255), koji ne smije biti null
- *ulogaID* – podatak tipa INTEGER, predstavlja strani ključ koji referencira relaciju uloga
- *telefon* – podatak tipa VARCHAR(255)
- *email* – podatak tipa VARCHAR(255), mora biti jedinstven

Ograničenje NOT NULL označava da podaci ime i prezime ne smiju biti null tipa podatka. Atribut email je dodatno ograničen kao jedinstven (UNIQUE). Atribut ulogaID može biti null ako zaposlenik nije dodijeljen nijednoj ulozi.

![Screenshot 2025-05-26 184845](https://github.com/user-attachments/assets/4444456b-e53f-4486-9b9d-53b162e5e36b)

Relacija **uloga**\
Evidentira vrste uloga koje zaposlenici mogu imati. Relacija uloga se sastoji od sljedećih atributa:

- *ulogaID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije
- *nazivUloge* – podatak tipa VARCHAR(255), koji ne smije biti NOT NULL

Ograničenje NOT NULL označava da vrijednost za atribut nazivUloge mora biti unesena – npr. konobar, kuhar, menadžer itd.

![Screenshot 2025-05-26 190336](https://github.com/user-attachments/assets/8060f073-e63a-4022-8199-d8365452edd0)

**Relacija stol**\
Evidentira informacije o stolovima u objektu, sastoji od sljedećih atributa:  

- *stolID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije  
- *brojStola* – podatak tipa INTEGER, koji ne smije biti NOT NULL  
- *kapacitet* – podatak tipa INTEGER, koji ne smije biti NOT NULL

Ograničenje NOT NULL označava da vrijednosti za atribute brojStola i kapacitet moraju biti unesene. Na taj način se osigurava da svaki stol ima dodijeljen broj i broj osoba koje može primiti.

![Screenshot 2025-05-26 195426](https://github.com/user-attachments/assets/22701f11-b191-41e8-a60e-5a7dd9ba70cc)

**Relacija narudzba**\
Evidentira informacije o narudžbama koje kupci ostvaruju, sastoji se od sljedećih atributa:

- **narudzbaID** – podatak tipa INTEGER, koji je **primarni ključ** unutar relacije
- **datumVrijeme** – podatak tipa DATETIME, označava točno vrijeme narudžbe
- **kupacID** – podatak tipa INTEGER, predstavlja **strani ključ** koji referencira kupca koji je napravio narudžbu
- **stolID** – podatak tipa INTEGER, predstavlja **strani ključ** koji povezuje narudžbu sa stolom za kojim je napravljena
- **zaposlenikID** – podatak tipa INTEGER, predstavlja **strani ključ** koji označava zaposlenika koji je zaprimio narudžbu

U ovoj relaciji nisu eksplicitno navedena ograničenja NOT NULL, ali se pretpostavlja da su kupacID, datumVrijeme i zaposlenikID važni za integritet podataka te bi u konačnoj verziji trebali imati dodatna ograničenja poput NOT NULL i FOREIGN KEY odnosa.

![Screenshot 2025-05-26 203021](https://github.com/user-attachments/assets/3286d3ae-a43c-446e-af97-78695090719e)

**Relacija Rezervacija**\

Relacija Rezervacija prati podatke o rezervacijama koje kupci izrađuju za određene stolove u kafiću. Svaka rezervacija uključuje informaciju o vremenu, broju osoba te statusu rezervacije. Relacija se sastoji od sljedećih atributa:

*RezervacijaID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije i jedinstveno identificira svaku rezervaciju, ne smije biti NULL.

*KupacID* – podatak tipa INTEGER, koji se ponaša kao strani ključ i povezan je s primarnim ključem tablice Kupac, te ne smije biti NULL.

*StolID* – podatak tipa INTEGER, koji se ponaša kao strani ključ i povezan je s primarnim ključem tablice Stol, ne smije biti NULL.

*DatumVrijeme* – podatak tipa DATETIME, koji označava točan datum i vrijeme kada je rezervacija zakazana, ne smije biti NULL.

*BrojOsoba* – podatak tipa INTEGER, koji označava broj osoba za koje je rezervacija napravljena, ne smije biti NULL.

*Status* – podatak tipa VARCHAR(255). Koristi se za praćenje statusa rezervacije, primjerice: “aktivna”, “otkazana” ili “dovršena”, ne smije biti NULL.




