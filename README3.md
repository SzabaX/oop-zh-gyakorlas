```mermaid
classDiagram
    class Urbazis {
        - nev : string
        - urhajosok : Urhajos[*]
        - epuletek : Epulet[*]
        + veszelySzimulacio(v: Veszely)
        + gyengeEpuletek(korlat: int) : string[*]
    }
    
    class Urhajos {
        - nev : string
        - allapot : int
        + Urhajos(n: string, a: int)
        + allapotCsokken(ertek: int)
        + meghal()
    }
    
    class Epulet {
        - nev : string
        - allag : int
        - lakok : Urhajos[*]
        + Epulet(n: string, a: int)
        + allagCsokken(ertek: int)
        + osszedol()
        + epuletAllapot() : int
    }
    
    %% Tervminta: Stratégia (Strategy) 
    class Veszely {
        <<interface>>
        + sujt(u: Urbazis)
    }
    
    %% Tervminta: Egyke (Singleton)
    class Porvihar {
        <<singleton>>
        + sujt(u: Urbazis)
    }
    class Meteorzapor {
        <<singleton>>
        + sujt(u: Urbazis)
    }
    class Oxigenhiany {
        <<singleton>>
        + sujt(u: Urbazis)
    }

    Urbazis "1" *-- "*" Urhajos : tartalmaz
    Urbazis "1" *-- "*" Epulet : tartalmaz
    Epulet "0..1" o-- "*" Urhajos : lakik
    
    Veszely <|.. Porvihar : megvalósít
    Veszely <|.. Meteorzapor : megvalósít
    Veszely <|.. Oxigenhiany : megvalósít
