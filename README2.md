```mermaid
classDiagram
    class Sziget {
        - hajotorottek : Hajotorott[*]
        - kunyhok : Kunyho[*]
        + katasztrofaSzimulacio(k: Katasztrofa)
        + tobbAllapotuTulelok() : int
    }
    
    class Hajotorott {
        - nev : string
        - allapot : int
        + Hajotorott(n: string, a: int)
        + allapotCsokken(ertek: int)
        + meghal()
    }
    
    class Kunyho {
        - allapot : int
        - lakok : Hajotorott[*]
        + Kunyho(a: int)
        + allapotCsokken(ertek: int)
        + osszedol()
    }
    
    %% Tervminta: Stratégia (Strategy) 
    class Katasztrofa {
        <<interface>>
        + sujt(sz: Sziget)
    }
    
    %% Tervminta: Egyke (Singleton)
    class TropusiVihar {
        <<singleton>>
        + sujt(sz: Sziget)
    }
    class MediterranEsozes {
        <<singleton>>
        + sujt(sz: Sziget)
    }
    class SivatagiPorvihar {
        <<singleton>>
        + sujt(sz: Sziget)
    }

    Sziget "1" *-- "*" Hajotorott : tartalmaz
    Sziget "1" *-- "*" Kunyho : tartalmaz
    Kunyho "0..1" o-- "*" Hajotorott : lakik
    
    Katasztrofa <|.. TropusiVihar : megvalósít
    Katasztrofa <|.. MediterranEsozes : megvalósít
    Katasztrofa <|.. SivatagiPorvihar : megvalósít
