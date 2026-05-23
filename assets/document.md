# Abstrakt

Dokonano symulacji rozpadu kaonu $K^0_S$ na dwa mezony $\pi^{+}$ i $\pi^{-}$. Wyznaczono rozkład masy niezmienniczej układu, rozkład długości rozpadu oraz rozkład czasu życia.
Wyznaczono średni czas życia kaonu $K^0_S$ równy $\tau = (90.122221 +- 4.432650) ps$.

# Cel ćwiczenia

Celem przeprowadzonej symulacji była analiza danych pomiarowych rozpadu cząstki, w tym wyznaczenie jej czasu życia oraz rozkładu masy.

# Wstęp teoretyczny

**Cząstki Elementarne** charakteryzują się parametrem nazywanym **czasem życia** ($\tau$), który określa średni czas, po którym dana cząstka ulegnie rozpadowi.

Niech $\vec{x_0}$ oznacza położenie czastki w chwili powstania, natomiast $\vec{x_r}$ - w chwili zarejestrowanego rozpadu.

Można wyznaczyć wartość długości rozpadu według wzoru:

$$
L = \left|\vec{x_r} - \vec{x_0}\right|
$$ (L)

Długość rozpadu można również wyrazić poprzez prędkość cząstki:

$$
L = v * t_{lab}
$$

gdzie:
- $v$ - prędkość cząstki
- $t_{lab}$ - czas życia cząstki w układzie laboratoryjnym

Z STW można przekształcić powyższe równanie do postaci:

$$
L = \gamma * v * t
$$


Gdzie:
- $\beta$ - prędkość cząstki wyrażona jako ułamek prędkości światła, czyli $\beta = \frac{v}{c}$
- $\gamma$ - współczynnik: $\gamma = \frac{1}{\sqrt{1 - \beta^2}}$

W ujęciu relatywistycznym można zapisać pęd jako:

$$
p = \gamma * m * \beta \\
\gamma * \beta = \frac{p}{m} \\
\gamma * v = \frac{p}{m} * c
$$

Po podstawieniu otrzymujemy:

$$
L = \frac{p}{m} * c * t \\
t = \frac{L * m}{p * c}
$$ (t)

gdzie:
- L - długość rozpadu (według {eq}`L`)
- c - prędkość światła (przyjęta według [literatura 2.](#literatura))
- m - masa 
- t - czas życia cząstki

## Średni czas życia

Zakłada się, że rozpad cząstki jest procesem losowym o stałym prawdopodobieńśtwie rozpadu na jednostkę czasu.
Dlatego też rozkład **czasu życia** jest rozkładem wykładniczym, którego gęstość prawdopodobieństwa jest dana według wzoru:

$$
f(t) = \frac{1}{\tau} e^{-\frac{t}{\tau}}
$$ (tau)

gdzie:
- $\tau$ - średni czas życia cząstki

# Aparatura i metodyka wykonania

Otrzymano plik z danymi pomiarowymi (załączony wraz z kodem, patrz [literatura](#literatura)) w formacie kompatybilnym z językiem root.

W celu wykonania ćwiczenia opracowano kod w języku root analizujący dane z pliku.

Użyto języka root w wersji:

```console
$ root --version
ROOT Version: 6.38.04
Built for linuxx8664gcc on Apr 10 2026, 05:09:12
From tags/6-38-04@6-38-04
```

(szacowanie-niepewnosci)=
## Metoda szacowania niepewności

W ćwiczeniu wykorzystano metodę automatycznego szacowania niepewności.

Niech `fit0`, `fit1` oraz `fit2` oznaczają instancje klasy `TF1`.
Rozkład `fit0` jest jest sumą rozkładów `fit1` oraz `fit2` i został on dopasowany do danych.

Aby dokonać szacowania niepewności na zadanym zakresie dla `fit1`, w pierwszym kroku należy przepisać odpowiednie
parametry `fit0` do `fit1`

Następnie należy wytworzyć odpowiednią macierz kowariancji dla `fit1` na podstawie macierzy kowariancji `fit0` poprzez wybranie odpowiednich elementów według następującego schematu:

```{code-block} c++
:linenos:

    auto const covarianceMatrix = fitResult->GetCovarianceMatrix();
    TMatrixDSym covSig(3);
    for (int i = 0; i < 3; ++i)
        for (int j = 0; j < 3; ++j)
           covSig(i,j) = covarianceMatrix(i,j);

    auto const signalError = signalFit->IntegralError(
        sigMin, sigMax,
        sigParams,
        covSig.GetMatrixArray()
    ) / normalization;
```

gdzie:
- `fitResult` - obiekt uzyskiwany w wyniku dopasowania `fit0` do danych
- `sigMin` oraz `sigMax` - arbitralnie zadane granice przedziału sygnałowego
- `normalization` - normalizacja histogramu. Tu: szerokość binu.

# Wyniki symulacji

## Badanie rozkładu masy

```{code-block} yaml
:caption: Średnie wartości masy dla poszczególnych cząstek.
:name: mass

Average Mass:
    h1: 139.558045
    h2: 139.558045
    sum: 498.094379 +- 0.072204
```

```{figure} ./simulation/mass_distribution.png
:name: mass_distribution

Rozkład masy niezmienniczej układu wraz z dopasowaną krzywą.
```

Do histogramu masy niezmienniczej {ref}`mass_distribution` dopasowano krzywą, która jest sumą funkcji Gaussa (opisującej sygnał) oraz wielomianu 2 stopnia (opisującego tło).
Następnie wyznaczono liczbę faktycznych rozpadów (sygnałuu) oraz liczbę rozpadów uznanych za tło a także stosunek sygnału do tła.

```{note}
Sumowania dokonano na przedziale arbitralnie uznanym za przedział sygnałowy, to jest: $m_{sig} \in \left[487, 510\right]$.
```

Otrzymano następujące wyniki wraz z [niepewnościami](#szacowanie-niepewnosci):

```yaml
Counts:
    signal: 4080.564567 +- 83.782392
    background: 1738.336608 +- 46.387295
    signal-background ratio: 2.347396
```

# Długość rozpadu

Na podstawie wzoru {eq}`L` wyznaczono rozkład długości rozpadów w przedziale sygnałowym, który przedstawiono na poniższym histogramie:

```{figure} ./simulation/length_distribution.png
Rozkłąd długości rozpadu.
```

# Czas życia

Wyznaczono czas życia dla każdego rozpadu według wzoru {eq}`t` i przedstawiono jego rozkład na poniższym histogramie:

```{figure} ./simulation/lifetime_distribution.png
Rozkłąd czasu życia.
```

Do powyższego histogramu dopasowano krzywą według wzoru {eq}`tau`.

Na podstawie dopasowania krzywej {eq}`tau` wyznaczono średni czas życia równy $\tau = (90.122221 +- 4.432650) ps$

# Wnioski

## Identyfikacja cząstek

Zidentyfikowano cząstki biorące udział w symulacji, na podstawie [Literatura 3.](#literatura).

Zgodnie z [danymi otrzymanymi z symulacji](#mass) $m_{h1} = m_{h2} = 139.558045 MeV$ co odpowiada
masie mezonu $\pi^{\pm}$ ($m_{\pi^{\pm}} = 139.57039 MeV$).

Natomiast cząstka rozpadająca się o masie $m_{sum} = (498.094379 \pm 0.072204) MeV$ jest kaon $K^0$ ($m_{K^0} = 497.611 MeV$).

Kaon $K^0_S$ może rozpadać się na dwa mezony $\pi^{+}$ i $\pi^{-}$ (z prawdopodobieństwem $\left(69.20 \pm 0.05\right)\%$) (najbardziej prawdopodobny rozpad). Z tego wynika że cząstki $h1$ i $h2$ to odpowiednio $\pi^{+}$ i $\pi^{-}$.

# Podsumowanie

W poniższej tabeli zebrano najważniejsze wyniki wymulacji rozpadu kaonu $K^0_S$ na dwa piony $\pi^{+}$ i $\pi^{-}$:

```{table} Podsumowanie wyników symulacji
:name: summary

| Parametr | Wartość | Niepewność | Jednostka |
| --- | --- | --- | --- |
| Masa $K^0_S$ | 498.094 | 0.072 | MeV |
| Masa $\pi^{\pm}$ | 139.558 | - | MeV |
| Czas życia $\tau$ | 90.12 | 4.43 | ps |
```

# Literatura

1. Kod symulacyjny + plik danych https://github.com/gucio321-studies/ceioProj - rewizja `37f72814880db35210cb1032e09b54a89fb424f1`
2. Root `TMath::C()` - https://root.cern.ch/doc/master/namespaceTMath.html#a22dbfc37d5e400dec5ac6b1351e2061a
3. Light Unflavored Mesons by Particle Data Group (PDG) - https://pdg.lbl.gov/2025/tables/contents_tables.html dostęp 2026-05-23
