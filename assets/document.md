# Cel ćwiczenia

Celem przeprowadzonej symulacji była analiza danych pomiarowych rozpadu cząstki, w tym wyznaczenie jej czasu życia oraz rozkładu masy.

# Wstęp teoretyczny

**Cząstki Elementarne** charakteryzują się parametrem nazywanym **czasem życia** ($\tau$), który określa średni czas, po którym dana cząstka ulegnie rozpadowi.

Niech $\vec{x_0}$ oznacza położenie czastki w chwili powstania, natomiast $\vec{x_r}$ - w chwili zarejestrowanego rozpadu.

Można wyznaczyć wartość długości rozpadu według wzoru:

$$
L = \left|\vec{x_r} - \vec{x_0}\right|
$$ (L)

Na tej podstawie można wyznaczyć czas życia danej cząstki:

$$
\gamma = \frac{1}{\sqrt{1 - \beta^2}} \\
$$

$$
t = \frac{L * m}{p * c}
$$

gdzie:
- L - długość rozpadu (według {eq}`L`)
- c - prędkość światła (przyjęta według [literatura 2.](#literatura))
- m - masa 
- t - czas życia cząstki


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

# Wyniki symulacji

```{figure} ./mass_distribution.png
Rozkłąd masy.
```

```console
Counts:
	signal: 4080.564567 +- 83.782392
	background: 1738.336608 +- 46.387295
	signal-background ratio: 2.347396
```

```{figure} ./length_distribution.png
Rozkłąd długości rozpadu.
```

```{figure} ./lifetime_distribution.png
Rozkłąd czasu życia.
```

Na podstawie dopasowania krzywej {eq}`tau` do powyższego histogramu wyznaczono średni czas życia równy $\tau = 90.122221ps$

# Wnioski

# Podsumowanie

# Literatura

1. Kod symulacyjny + plik danych https://github.com/gucio321-studies/ceioProj - rewizja master (TODO)
2. Root `TMath::C()` - 
