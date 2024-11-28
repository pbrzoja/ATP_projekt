# Rad s tekstualnim datotekama

Rad s običnim tekstualnim datotekama važniji je no što se to možda čini
na prvi pogled. Na primjer, svaki program nastaje kao običan tekst,
cijeli WWW se zasniva na običnom tekstu, kao i razmjena podataka među
informacijskim sustavima. Čitanje i pisanje tekstualnih datoteka je
stoga neobično važno ne samo za programiranje već i za sveukupno
računarstvo.

Glavni mehanizam za čitanje i pisanje tekstualnih datoteka u Pythonu je
funkcija `open`{.python}. Ovisno o parametrima, ova funkcija pristupa
nekoj datoteci za potrebe čitanja iz nje ili pisanja u nju. Za razliku
od istoimene mogućnosti u programima s grafičkim sučeljem, dakle,
funkcija `open`{.python} samo rezervira određenu datoteku za pristup, a
ne usnimava sadržaj datoteke. Radi toga, ova funkcija služi i pisanju u
datoteke, a čak i stvaranju novih tekstualnih datoteka. Bez obzira da li
čitali ili pisali, nakon što se izvrše potrebne radnje s otvorenom
datotekom, istu je potrebno zatvoriti putem metode `close`{.python}.
Pogledajmo kako zapisati neki tekst u novu datoteku:

Funkcija `open`{.python} prima putanju do datoteke kao prvi parametar,
mod rada kao drugi parametar i dodatne parametre od kojih je vrlo
korisna specifikacija kodne stranice putem opcionalnog parametra
`encoding`{.python}. Dapače, preporučeno je uvijek definirati
`encoding`{.python} prilikom čitanja i pisanja jer u tom slučaju jasno
kontroliramo kodnu stranicu našeg teksta i time sprječavamo gubitak
slova i ostalih znakova. U tom smislu, kodna stranica \"utf-8\" je danas
*de-facto* standard.

Putanja može biti apsolutna e.g. (\"c:/direktorij/datoteka.txt\") ili
relativna (\"datoteka.txt\" ili \"direktorij/datoteka.txt\"). Ukoliko je
putanja relativna, kao u našem primjeru, smatra se da je relativna od
direktorija u kojem se nalazi .py datoteka koju smo pokrenuli. Na
primjer, putanja \"datoteka.txt\" se odnosi na datoteku u istom
direktoriju u kojem je i pokrenuta .py datoteka. Putanja
\"neki_direktorij/datoteka.txt\" se odnosi na datoteku \"datoteka.txt\"
koja se nalazi u direktoriju \"neki_direktorij\" koji se pak nalazi u
istom direktoriju u kojem je i pokrenuta .py datoteka. U ranijem
primjeru, u istom direktoriju će nam se pojaviti datoteka \"papiga.txt\"
s tekstom koji smo naredili zapisati.

Funkicija `open`{.python}, može čitati postojeće datoteke, dodavati u
postojeće ili stvarati nove. To kontroliramo putem parametra
`mode`{.python} koji možemo shvatiti kao \"mod rada\" funkcije
`open`{.python}. Pogledajmo kako dodati neke retke u \"papiga.txt\"
datototeku.

Korisni modovi za `open`{.python} su:

-   **r** - čitaj; *default*

-   **w** - stvori i piši; ako datoteka postoji, obriši sadržaj

-   **x** - stvori i piši; ako datoteka postoji, javi grešku

-   **a** - stvori i piši; ako datoteka postoji, nastavi pisati na kraj

Do sada smo samo pisali u datoteku. Čitanje je jednostavno drugi način
operacije funkcije `open`{.python}. Čitanje je zapravo zadani (eng.
*default*) način rada ove funkcije, ali u ovom poglavlju smo prvo
stvorili novu datoteku kako bi imali što čitati. Također, parametar
`"r"`{.python} zapravo ne treba navoditi jer se podrazumijeva, ali
njegovim navođenjem se mod rada jasnije vidi.

::: pythonp
[\[listing:tekst_citanje\]](#listing:tekst_citanje){reference-type="ref"
reference="listing:tekst_citanje"} This parrot is no more! He has ceased
to be! 'E's expired and gone to meet 'is maker! 'Is metabolic processes
are now 'istory! 'E's off the twig! \..... THIS IS AN EX-PARROT!!
:::

Pogledajmo kako se petlja `for`{.python} može koristiti u kontekstu
čitanja teksta iz datoteke:

::: pythonp
[\[listing:tekst_prebiranje\]](#listing:tekst_prebiranje){reference-type="ref"
reference="listing:tekst_prebiranje"} 1 This parrot is no more! 2 He has
ceased to be! 3 'E's expired and gone to meet 'is maker! 4 'Is metabolic
processes are now 'istory! 5 'E's off the twig! 6 \..... THIS IS AN
EX-PARROT!!
:::

Drugim riječima, po otvorenoj datoteci se može iterirati po recima
teksta. Na ovaj način možemo raditi s datotekama bilo koje veličine pa
čak i onima koje nam ne stanu u memoriju jer u svakom trenutku imamo
samo jedan redak u memoriji, a ne cijeli tekstualni sadržaj neke
datoteke.

## Naredba `with`{.python}

Što će se dogoditi s datotekom ako slučajno ne pozovemo naredbu
`close`{.python}? Postoji šansa da se u datoteku nije zapisao sav tekst
i da ona u operacijskom sustavu ostane \"rezervirana za pristup\".
Problem s do sada prikazanim načinom zatvaranja datoteke nije samo u
tome da možemo zaboraviti provesti naredbu `close`{.python}, već se može
dogoditi da se ona ne provede radi ranije greške. Pogledajmo primjer:

Kako riješiti da se naredba `close`{.python} uvijek izvrši, bez obzira
na potencijalne ranije greške u kôdu? Iz onoga što do sad znamo, mogli
bismo probati s naredbom `try`{.python} i ukoliko iskoristimo i
komponentu `finally`{.python} u tome bi i uspjeli[^1]. Ipak, ovakvo
rješenje se smatra nezgrapnim i nije namijenjeno korištenje naredbe
`try`{.python}. Radi ovakvih i sličnih slučajeva se u noviji Python
dodala naredba `with`{.python} koja je postala idiom upravo za otvaranje
i zatvaranje datoteka kao i slične situacije.

Radi elegantnosti izvedbe i vezanu sigurnost, preporuča se koristiti
naredbu `with`{.python}. Prikazani način pristupa tekstualnim datotekama
je stoga idiom u novijem Pythonu, ali ne možemo u potpunosti shvatiti
kako funkcionira bez razumijevanja koncepata \"otvaranja\" i
\"zatvaranja\" datoteka. Također, postoje slučajevi u kojima je
korištenje naredbe `with`{.python} nezgrapno. U tim slučajevima možemo
nastaviti koristiti metodu `close`{.python}.

[^1]: Za vježbu razmislite kako bismo naredbom `try`{.python} mogli
    *garantirati* izvršavanje pozivanje metode `close`{.python} čak i u
    slučaju ranije pogreške
