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

```{code-block} yaml
:caption: Średnie wartości masy dla poszczególnych cząstek.
:name: mass

Average Mass:
    h1: 139.558045
    h2: 139.558045
    sum: 497.960529
```

```{figure} ./mass_distribution.png
Rozkład masy.
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

## Identyfikacja cząstek

Zidentyfikowano cząstki biorące udział w symulacji, na podstawie [Literatura 3.](#literatura).

Zgodnie z [danymi otrzymanymi z symulacji](#mass) $m_{h1} = m_{h2} = 139.558045 MeV$ co odpowiada
masie mezonu $\pi^{\pm}$ ($m_{\pi^{\pm}} = 139.57039 MeV$).

Natomiast cząstka rozpadająca się o masie $m_{sum} = 497.960529 MeV$ jest kaon $K^0$ ($m_{K^0} = 497.611 MeV$).

# Podsumowanie

# Literatura

1. Kod symulacyjny + plik danych https://github.com/gucio321-studies/ceioProj - rewizja master (TODO)
2. Root `TMath::C()` - https://root.cern.ch/doc/master/namespaceTMath.html#a22dbfc37d5e400dec5ac6b1351e2061a
3. Light Unflavored Mesons by Particle Data Group (PDG) - https://pdg.lbl.gov/2025/tables/contents_tables.html dostęp 2026-05-23
