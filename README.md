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

<img width="209" alt="image" src="https://github.com/user-attachments/assets/911358ea-0c1d-4366-b1ce-f4144d464e2a" />



**Relacija rezervacija**\
Relacija Rezervacija prati podatke o rezervacijama koje kupci izrađuju za određene stolove u kafiću. Svaka rezervacija uključuje informaciju o vremenu, broju osoba te statusu rezervacije. 

- *RezervacijaID* – podatak tipa INTEGER, koji je primarni ključ unutar relacije i jedinstveno identificira svaku rezervaciju
- *KupacID* – podatak tipa INTEGER, koji se ponaša kao strani ključ i povezan je s primarnim ključem tablice Kupac
- *StolID* – podatak tipa INTEGER, koji se ponaša kao strani ključ i povezan je s primarnim ključem tablice Stol
- *DatumVrijeme* – podatak tipa DATETIME, koji označava točan datum i vrijeme kada je rezervacija zakazana
- *BrojOsoba* – podatak tipa INTEGER, koji označava broj osoba za koje je rezervacija napravljena
- *Status* – podatak tipa VARCHAR(255). Koristi se za praćenje statusa rezervacije, primjerice: “aktivna”, “otkazana” ili “dovršena”

<img width="182" alt="image" src="https://github.com/user-attachments/assets/1ba958d2-e7f5-46fe-9496-03631c5dcd2f" />


**Relacija StakvaNarudzbe**\
Relacija StavkaNarudzbe predstavlja vezu između narudžbi i pojedinačnih proizvoda koji su naručeni. Svaka narudžba može sadržavati više stavki, a svaka stavka odnosi se na određeni proizvod s određenom količinom i cijenom.

  - *NarudzbaID* - složeni primarni ključ (zajedno s ProizvodID), označava kojoj narudžbi stavka pripada, podatak tipa INTEGER
  - *ProizvodID* -  složeni primarni ključ  označava koji je proizvod naručen, podatak tipa INTEGER
  - *Kolicina* - količina naručenog proizvoda, podatak tipa INTEGER
  - *JedinicnaCijena* - cijena po jedinici proizvoda u trenutku narudžbe (omogućuje praćenje povijesnih cijena), podatak tipa INTEGER
  - *PRIMARY KEY (NarudzbaID, ProizvodID)* - NarudzbaID je strani ključ prema tablici Narudzba. ProizvodID je strani ključ prema tablici Proizvod. Jedna narudžba može sadržavati više   stavki, a jedan proizvod može biti dio više narudžbi.
  
<img width="238" alt="image" src="https://github.com/user-attachments/assets/168ce966-159a-4dda-828e-8038db9df5d4" />



