# Predykcja bankructwa polskich przedsiębiorstw

## Cel projektu

Celem projektu jest przewidzenie, czy firma zbankrutuje w kolejnym roku na podstawie 64 wskaźników finansowych.

- `class = 0` — brak bankructwa,
- `class = 1` — bankructwo.

W tym problemie bardziej niekorzystny jest **fałszywy negatyw (FN)**, czyli sytuacja, w której model nie wykrywa firmy, która rzeczywiście bankrutuje. Dlatego oprócz accuracy ważne są także recall, F1, Average Precision oraz dobór progu klasyfikacji.

## Dane

Źródło: **Polish Companies Bankruptcy Data**, UCI Machine Learning Repository.

Projekt wykorzystuje plik:

`dane/5year.arff`

Po usunięciu 60 pełnych duplikatów pozostaje:

- **5850 obserwacji**,
- **64 cechy finansowe**,
- **408 przypadków bankructwa** — około **6,97%** zbioru.

Oczyszczone dane są zapisane jako:

`dane/5year_cleaned.csv`

## Struktura projektu

```text
ML_porownanie/
├── README.md
├── 01_czyszczenie.ipynb
├── 02_eda.ipynb
├── 03_porownanie_modeli.ipynb
├── 04_tuning_finalny.ipynb
├── wyniki_modeli.csv
└── dane/
    ├── 5year.arff
    └── 5year_cleaned.csv
```

`wyniki_modeli.csv` powstaje w notebooku 03 i przekazuje krótką tabelę wyników do notebooka 04.

## Najważniejsze decyzje

- pełne duplikaty są usuwane,
- wartości skrajne pozostają w danych,
- braki są uzupełniane medianą wewnątrz `Pipeline`,
- skalowanie jest stosowane tylko dla modeli, które go potrzebują,
- EDA jest wykonywane wyłącznie na części treningowej,
- test pozostaje odłożony do końcowej oceny,
- wszystkie modele są porównywane przez 5-fold cross-validation,
- do dokładniejszego strojenia przechodzą trzy najlepsze modele,
- próg finalnego modelu jest wybierany na walidacji według F2.

## Feature engineering

Dodane zostały dwie proste cechy:

- `missing_count` — liczba braków w danym wierszu,
- `has_missing` — informacja, czy wiersz ma przynajmniej jeden brak.

W EDA widać, że braki danych częściej występują w klasie `1`, dlatego warto zachować tę informację dla modelu.

## Porównanie modeli

Porównano 11 konfiguracji:

- Logistic Regression,
- kNN,
- SVM,
- Decision Tree,
- Random Forest,
- Gradient Boosting,
- XGBoost,
- LightGBM,
- MLP (32),
- MLP (64, 32),
- MLP (64, 32, 16).

Trzy najlepsze wyniki w 5-fold CV:

| Model | AP | F1 | Recall | Średni czas fitu |
|---|---:|---:|---:|---:|
| LightGBM | 0,791 | 0,698 | 0,567 | 1,19 s |
| XGBoost | 0,773 | 0,661 | 0,529 | 1,51 s |
| Gradient Boosting | 0,722 | 0,636 | 0,502 | 9,52 s |

Do dokładniejszego strojenia przeszły te trzy modele.

## Tuning i wybór progu

Po szerszym strojeniu LightGBM nadal był najlepszą rodziną. Dokładniejsze strojenie nie poprawiło jego wyniku CV AP (`0,7909 → 0,7875`), więc poprawa nie wynikała z samego zwiększenia liczby obliczeń.

Na walidacji przy standardowym progu `0,5` LightGBM miał wysokie precision, ale niższy recall. Dlatego próg został dobrany według F2.

Wybrany próg:

**0,20**

## Wynik końcowy

Na zbiorze testowym LightGBM uzyskał:

| Metryka | Wynik |
|---|---:|
| Accuracy | 0,960 |
| Precision | 0,692 |
| Recall | 0,768 |
| F1 | 0,728 |
| F2 | 0,752 |
| ROC-AUC | 0,964 |
| Average Precision | 0,814 |

Macierz pomyłek:

- **TN = 1060**
- **FP = 28**
- **FN = 19**
- **TP = 63**

W teście było 82 rzeczywistych bankructw. Model poprawnie wykrył **63 z 82**, czyli około **76,8%**.

## Ważność cech

Wśród ważnych cech znalazły się m.in. `Attr34`, `Attr21`, `Attr24`, `Attr46`, `Attr58` i `Attr27`.

W TOP 10 pojawiły się również wskaźniki braków dla `Attr21` i `Attr27`. Pasuje to do obserwacji z EDA, że sam fakt braku danych może zawierać dodatkową informację.

## Wniosek

Finalnym modelem został **LightGBM z progiem 0,20**.

Model wykrywa większość bankructw, a liczba fałszywych alarmów pozostaje stosunkowo niewielka. W tym zastosowaniu taki kompromis jest sensowny, ponieważ bardziej zależy mi na wykrywaniu zagrożonych firm niż na całkowitym unikaniu dodatkowych alarmów.

Model powinien służyć jako narzędzie wspierające analizę, a nie jako automatyczna decyzja o firmie.
