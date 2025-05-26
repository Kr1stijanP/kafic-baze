## UVOD

U priloženoj dokumentaciji prezentirat ćemo naš projektni zadatak iz kolegija Baze podataka 1. Tema projekta je izrada baze podataka za vođenje poslovanja kafića. Kao prvi korak izrade baze podataka odlučili smo generirati vlastite tablice i podatke, kako bismo pojednostavili unos i izbjegli oslanjanje na vanjske izvore.
Cilj naše baze podataka je omogućiti kvalitetno i jasno vođenje osnovnih operacija u kafiću. Prikazujemo osnovne informacije o proizvodima, zaposlenima, narudžbama, zalihama 

## Opis ER dijagrama

![er](https://github.com/user-attachments/assets/c68751f2-a408-43b5-ac22-b7100ebaa69d)

ntitet **Kupac** predstavlja osobu koja koristi usluge restorana. Sadrži atribute poput KupacID, Ime, Prezime, Telefon, Email i StatusVjernosti, što omogućuje razlikovanje redovnih gostiju i onih koji možda ostvaruju određene pogodnosti.

**Rezervacija** je povezana s kupcem i definira kada je određeni gost rezervirao stol. Entitet ima RezervacijaID i DatumVrijeme, a preko veze "Rezervira", povezan je s entitetom Kupac. To implicira da jedan kupac može imati više rezervacija, a svaka rezervacija pripada jednom kupcu (1:N veza).

Entitet **Zaposlenik** opisuje djelatnike restorana putem atributa poput ZaposlenikID, Ime, Prezime, Telefon i Email. Svaki zaposlenik također ima pridruženu Ulogu kroz entitet Uloga, koji određuje funkciju zaposlenika (npr. konobar, kuhar itd.).

Zaposlenik može biti raspoređen na više **smjena**. Svaka Smjena ima svoj SmjenaID, DatumVrijemePocetka i DatumVrijemeZavrsetka. Veza "Radi" povezuje zaposlenika sa smjenom.

Entitet **Stol** predstavlja fizičke stolove u restoranu. Svaki stol ima StolID, BrojStola i Kapacitet. Stol je povezan s rezervacijom putem veze "Za_stol", što omogućuje rezervaciju konkretnog stola u određeno vrijeme.

**Narudžbe** koje kupci izvršavaju po dolasku evidentiraju se u entitetu Narudzba, koji ima NarudzbaID i DatumVrijeme. Svaka narudžba se sastoji od više stavki narudžbe -**StavkaNarudzbe**, što se prikazuje vezom "Sadrži". Svaka stavka uključuje ProizvodID, Kolicina i JedinicnaCijena.

Svaka narudžba rezultira **plaćanjem**, koje se evidentira u entitetu Placanje s atributima PlacanjeID, Iznos, NacinPlacanja i DatumVrijeme. Veza "Plaća" povezuje narudžbu i plaćanje.

**Proizvod** opisuje artikle koji se nude u restoranu, s atributima ProizvodID, Naziv, Opis i Cijena. Svaki proizvod pripada određenoj kategoriji -**KategorijaProizvoda**, poput pića, predjela, glavnih jela itd., kroz vezu "Klasificira".

**Dobavljac** predstavlja vanjske suradnike koji dostavljaju sirovine za pripremu proizvoda. Sadrži atribute poput DobavljacID, Naziv, Telefon i Email. Povezan je s entitetom Proizvod preko veze "Opskrbljuje" – gdje se definira i RokIsporuke. Sirovine se naručuju putem NabavnaNarudzba, a svaka narudžba može sadržavati više stavki -**StavkaNabavneNarudzbe**, uključujući količinu i odabranu sirovinu.

Entitet **Sirovina** sadrži SirovinaID, Naziv i JedinicaMjere. Te sirovine se nalaze u zalihama -**ZalihaSirovine** koje prati KolicinaNaSkladistu. Zalihe se ažuriraju prema isporukama i potrošnji u proizvodima.
