# Automatska klasifikacija proizvoda po kategorijama

## Uvod
Ovaj projekat obuhvata razvoj sistema mašinskog učenja koji automatski predviđa kategoriju proizvoda na osnovu njegovog naziva.
Cilj je da se unapredi razvrstavanje proizvoda na platformama za onlajn trgovinu, smanji potreba za ručnim radom i poveća tačnost klasifikacije.

## Pregled skupa podataka
Početni skup podataka sadrži 35.311 proizvoda.
Nakon čišćenja i pripreme podataka:
- Broj proizvoda: 30.762
- Broj kategorija: 10
- Broj primera za treniranje: 24.609
- Broj primera za testiranje: 6.153

Fajl sa podacima: `products.csv`

## Priprema i čišćenje podataka
Primenjeni su sledeći koraci:
1. Uklanjanje zapisa kojima nedostaje naziv proizvoda ili kategorija.
2. Normalizacija teksta.
3. Uklanjanje dupliranih naziva proizvoda.
4. Uklanjanje zapisa sa neusaglašenim oznakama kategorija.
5. Ujednačavanje naziva kategorija.

Primeri ujednačavanja naziva kategorija:
- `CPU` → `CPUs`
- `Mobile Phone` → `Mobile Phones`
- `fridge` → `Fridges`

## Izdvajanje karakteristika teksta
Ispitana su dva pristupa TF-IDF vektorizaciji, kojom se tekst pretvara u numeričke karakteristike pogodne za modeliranje:
- TF-IDF na nivou reči: pojedinačne reči i parovi uzastopnih reči (unigrami i bigrami).
- TF-IDF na nivou karaktera: nizovi od 3 do 5 uzastopnih karaktera (karakterni n-grami).
TF-IDF na nivou karaktera dao je bolje rezultate, naročito kod naziva proizvoda koji sadrže tehničke oznake modela.

## Modeli mašinskog učenja
Upoređena su tri pristupa:

| Model | Tačnost (accuracy) | Makro F1 |
|-------|-------------------|----------|
| Multinomijalni naivni Bajes (Multinomial Naive Bayes) | 93,09% | 0,9309 |
| Logistička regresija sa TF-IDF karakteristikama na nivou reči | 94,91% | 0,9512 |
| Logistička regresija sa TF-IDF karakteristikama na nivou karaktera | 97,94% | 0,9796 |

## Konačni model
Izabrano rešenje objedinjuje sledeće komponente u scikit-learn tok obrade (`Pipeline`):
- TF-IDF vektorizator;
- karakterne n-grame dužine od 3 do 5 karaktera (`ngram_range=(3, 5)`);
- logističku regresiju.

Model je na korišćenom test skupu ostvario tačnost od 97,94%.
Ovo je najbolji rezultat zabeležen tokom eksperimenata. Za potvrdu uspešnosti na novim podacima potrebna je dodatna provera na nezavisnom skupu podataka.
Trenirani model sačuvan je u fajlu:
`models/product_classifier.pkl`

## Struktura projekta
- `product-classification/` — glavni folder projekta
  - `products.csv` — skup podataka
  - `train_model.py` — skript za treniranje i čuvanje modela
  - `predict_category.py` — skript za interaktivno predviđanje kategorije
  - `requirements.txt` — spisak potrebnih biblioteka
  - `README.md` — opis projekta i uputstvo za korišćenje
  - `models/` — folder sa sačuvanim modelom
    - `product_classifier.pkl`
  - `notebooks/` — folder sa Jupyter radnim sveskama
    - `01_product_classification_EDA.ipynb`

## Preuzimanje
Klonirati repozitorijum:
git clone https://github.com/nevena-blagojevic/product-classification.git

## Treniranje modela
Pokrenuti skript:
python train_model.py

Ovaj skript:
- učitava skup podataka;
- čisti i priprema nazive proizvoda;
- ujednačava oznake kategorija;
- trenira model u okviru toka obrade (`Pipeline`);
- ocenjuje uspešnost modela;
- čuva trenirani model.

## Predviđanje kategorije proizvoda
Pokrenuti skript:
python predict_category.py

Kada se pojavi upit, unesite naziv proizvoda.
Primer prikaza u terminalu:

Enter product title: apple iphone 7 32gb
Predicted category: Mobile Phones

U ovom primeru predviđena kategorija je `Mobile Phones` (mobilni telefoni). Poruke programa i oznake kategorija zadržane su na engleskom, kao u originalnom primeru.
Za izlazak iz programa unesite `exit`.

## Evaluacija modela
Uspešnost modela ocenjena je pomoću sledećih metrika i prikaza:
- tačnost (accuracy);
- preciznost (precision);
- odziv (recall);
- F1 mera (F1-score);
- matrica zabune (confusion matrix).

Najbolji model u sprovedenim eksperimentima ostvario je:

- Tačnost: 97,94%
- Makro F1: 0,9796

## Ograničenja i mogućnosti unapređenja
Model može pogrešno klasifikovati proizvode čiji su nazivi kratki ili dvosmisleni.
Na primer, pojedine oznake modela proizvoda ne pružaju dovoljno informacija za pouzdano određivanje kategorije.
Moguća unapređenja uključuju:

- proširenje skupa podataka za treniranje;
- podešavanje hiperparametara;
- ručnu proveru predikcija sa niskim stepenom pouzdanosti;
- proveru modela na nezavisnom skupu podataka;
- unapređenje postupka izdvajanja i oblikovanja karakteristika.

## Korišćeni alati
- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn
- Google Colab
- GitHub

## Autor:
Nevena Blagojević
