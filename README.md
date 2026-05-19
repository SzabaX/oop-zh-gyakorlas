```mermaid
classDiagram
    class Fold {
        - tulelok : Tulelo[*]
        - menedekek : Menedek[*]
        + csapasSzimulacio(c: Csapas)
        + legrosszabbMenedek(korlat: int) : Menedek
    }
    note for Fold "legrosszabbMenedek(korlat):\nlegrosszabb = null\nminAllapot = MAX_INT\nforeach m in menedekek:\n  if m.osszFizikaiAllapot() > korlat and m.allapot < minAllapot:\n    minAllapot = m.allapot\n    legrosszabb = m\nreturn legrosszabb\n\ncsapasSzimulacio(c):\n  c.sujt(this)"
    
    class Tulelo {
        - nev : string
        - eletero : int
        + Tulelo(n: string, e: int)
        + eleteroCsokken(ertek: int)
        + meghal()
    }
    note for Tulelo "eleteroCsokken(ertek):\n  eletero -= ertek\n  if eletero <= 0:\n    meghal()"
    
    class Menedek {
        - allapot : int
        - lakok : Tulelo[*]
        + Menedek(a: int)
        + allapotCsokken(ertek: int)
        + osszedol()
        + osszFizikaiAllapot() : int
    }
    note for Menedek "osszedol():\n  foreach t in lakok:\n    t.meghal()\n  lakok.clear()\n\nosszFizikaiAllapot():\n  sum = 0\n  foreach t in lakok:\n    sum += t.eletero\n  return sum"
    
    class Csapas {
        <<interface>>
        + sujt(f: Fold)
    }
    
    class Ehinseg {
        <<singleton>>
        + sujt(f: Fold)
    }
    note for Ehinseg "sujt(f):\n  foreach t in f.tulelok:\n    t.eleteroCsokken(2)"

    class RadioaktivEso {
        <<singleton>>
        + sujt(f: Fold)
    }
    note for RadioaktivEso "sujt(f):\n  foreach m in f.menedekek:\n    m.allapotCsokken(1)\n  foreach t in f.tulelok:\n    t.eleteroCsokken(1)"

    class MutansTamadas {
        <<singleton>>
        + sujt(f: Fold)
    }
    note for MutansTamadas "sujt(f):\n  foreach t in f.tulelok:\n    t.eleteroCsokken(3)"

    Fold "1" *-- "*" Tulelo : tartalmaz
    Fold "1" *-- "*" Menedek : tartalmaz
    Menedek "0..1" o-- "*" Tulelo : lakik
    
    Csapas <|.. Ehinseg : megvalósít
    Csapas <|.. RadioaktivEso : megvalósít
    Csapas <|.. MutansTamadas : megvalósít
