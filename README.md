## UVOD

U priloženoj dokumentaciji prezentirat ćemo naš projektni zadatak iz kolegija Baze podataka 1. Cilj projekta je razviti funkcionalan model baze podataka koji omogućuje učinkovito upravljanje različitim aspektima rada kafića, uključujući narudžbe, rezervacije, zaposlenike, zalihe sirovina i financijske transakcije. 
Kao prvi korak izrade baze podataka odlučili smo generirati vlastite tablice i podatke, kako bismo pojednostavili unos točnih podataka i izbjegli oslanjanje na vanjske izvore.
## Opis ER dijagrama

[Link na ER dijagram](https://lucid.app/lucidchart/7e3ca596-78ec-4f8d-9e66-618cb6cf1f40/edit?viewport_loc=-2689%2C-743%2C4235%2C1887%2C0_0&invitationId=inv_76bfcfcb-73cd-451d-8128-f57a1b90cb83)

Entitet **Kupac** predstavlja osobu koja koristi usluge restorana. Sadrži atribute poput KupacID, Ime, Prezime, Telefon, Email i StatusVjernosti, što omogućuje razlikovanje redovnih gostiju i onih koji možda ostvaruju određene pogodnosti.

**Rezervacija** je povezana s kupcem i definira kada je određeni gost rezervirao stol. Entitet ima RezervacijaID i DatumVrijeme, a preko veze "Rezervira", povezan je s entitetom Kupac. To implicira da jedan kupac može imati više rezervacija, a svaka rezervacija pripada jednom kupcu (1:N veza).

Entitet **Zaposlenik** opisuje djelatnike restorana putem atributa poput ZaposlenikID, Ime, Prezime, Telefon i Email. Svaki zaposlenik također ima pridruženu Ulogu kroz entitet Uloga, koji određuje funkciju zaposlenika (npr. konobar, kuhar itd.).

Zaposlenik može biti raspoređen na više **smjena**. Svaka Smjena ima svoj SmjenaID, DatumVrijemePocetka i DatumVrijemeZavrsetka. Veza "Radi" povezuje zaposlenika sa smjenom.

Entitet **Stol** predstavlja fizičke stolove u restoranu. Svaki stol ima StolID, BrojStola i Kapacitet. Stol je povezan s rezervacijom putem veze "Za_stol", što omogućuje rezervaciju konkretnog stola u određeno vrijeme.

**Narudžbe** koje kupci izvršavaju po dolasku evidentiraju se u entitetu Narudzba, koji ima NarudzbaID i DatumVrijeme. Svaka narudžba se sastoji od više stavki narudžbe -**StavkaNarudzbe**, što se prikazuje vezom "Sadrži". Svaka stavka uključuje ProizvodID, Kolicina i JedinicnaCijena.

Svaka narudžba rezultira **plaćanjem**, koje se evidentira u entitetu Placanje s atributima PlacanjeID, Iznos, NacinPlacanja i DatumVrijeme. Veza "Plaća" povezuje narudžbu i plaćanje.

**Proizvod** opisuje artikle koji se nude u restoranu, s atributima ProizvodID, Naziv, Opis i Cijena. Svaki proizvod pripada određenoj kategoriji -**KategorijaProizvoda**, poput pića, predjela, glavnih jela itd., kroz vezu "Klasificira".

**Dobavljac** predstavlja vanjske suradnike koji dostavljaju sirovine za pripremu proizvoda. Sadrži atribute poput DobavljacID, Naziv, Telefon i Email. Povezan je s entitetom Proizvod preko veze "Opskrbljuje" – gdje se definira i RokIsporuke. Sirovine se naručuju putem NabavnaNarudzba, a svaka narudžba može sadržavati više stavki -**StavkaNabavneNarudzbe**, uključujući količinu i odabranu sirovinu.

Entitet **Sirovina** sadrži SirovinaID, Naziv i JedinicaMjere. Te sirovine se nalaze u zalihama -**ZalihaSirovine** koje prati KolicinaNaSkladistu. Zalihe se ažuriraju prema isporukama i potrošnji u proizvodima.

## Relacije, atributi i ograničenja


Relacija __Kupac__

Prati osnovne, nepromjenjive podatke o kupcima koji posjećuju kafić. Relacija Kupac sastoji se od sljedećih atributa:

- KupacID – podatak tipa int, koji predstavlja primarni ključ unutar relacije. Jedinstveno identificira svakog kupca u bazi podataka.
- Ime – podatak tipa varchar, maksimalne duljine 255 znakova. Predstavlja osobno ime kupca.
- Prezime – podatak tipa varchar, maksimalne duljine 255 znakova. Predstavlja prezime kupca.
- Telefon – podatak tipa varchar, maksimalne duljine 255 znakova. Služi za pohranu telefonskog broja kupca.
- Email – podatak tipa varchar, maksimalne duljine 255 znakova. Koristi se za kontakt podatke te može biti koristan pri rezervacijama ili programu vjernosti.*
- StatusVjernosti – podatak tipa varchar, maksimalne duljine 255 znakova. Označava razinu lojalnosti kupca (npr. “standardni”, “član”, “VIP”) te se može koristiti u svrhu nagrađivanja redovitih gostiju.
- Svi atributi mogu sadržavati null vrijednosti osim primarnog ključa, a dodatna ograničenja (kao što su not null ili unique) mogu se dodati ovisno o potrebama daljnjeg razvoja baze i validacije podataka.

Relacija **zaposlenik**\
Evidentira podatke o zaposlenicima, sastoji od sljedećih atributa:

- *zaposlenikID* – podatak tipa integer, koji je primarni ključ unutar relacije
- *ime* – podatak tipa varchar(255), koji ne smije biti null
- *prezime* – podatak tipa varchar(255), koji ne smije biti null
- *ulogaID* – podatak tipa integer, predstavlja strani ključ koji referencira relaciju uloga
- *telefon* – podatak tipa varchar(255)
- *email* – podatak tipa varchar(255), mora biti jedinstven

Ograničenje NOT NULL označava da podaci ime i prezime ne smiju biti null tipa podatka. Atribut email je dodatno ograničen kao jedinstven (UNIQUE). Atribut ulogaID može biti null ako zaposlenik nije dodijeljen nijednoj ulozi.

