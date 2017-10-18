---
title       : Andmetöötluspakett plyr
description : Lisapaketi plyr kasutamine

--- type: NormalExercise key: f8b1ce389e lang: r xp: 100 skills: 1
## Andmestikku tunnuste lisamine 



Paketi **plyr** funktsioonidest `mutate()` on abiks kui on vaja andmestikku uusi tunnuseid lisada. Korraga saab defineerida mitu uut tunnust, kusjuures tunnust, mille defintsioon on käsus juba kirjas, saab kohe samas kasutada.<br><br>



Töölaual on olemas andmestik `A`. Andmestikus on kirjas 40 inimese id-kood, sugu, elukoht, vanus, pikkus, kaal, käte siruulatus ning arstivisiidi toimumine.




*** =instructions
- **Ülesanne 1** Aktiveeri pakett **plyr**.
- **Ülesanne 2** Kasutades paketi **plyr** funktsiooni `mutate()`, lisa andmestikku uuritavate KMI väärtus (tunnus nimega `kmi`) ja tunnus, mis sellesama KMI põhjal jagab inimesed 2 gruppi: kui KMI on kuni 25 (kaasa arvatud), siis on grupitunnuse väärtus `ala voi normkaal`, kui KMI on üle 25, siis `ylekaal`. Grupitunnuse nimeks vali `kaalugrupp`, selle moodustamiseks kasuta funktsiooni `ifelse()`. 
- **Ülesanne 3** Vaata üle uue andmestiku struktuur käsuga `str()`.


*** =hint
- Kui kirjutad `mutate()` käsku uute tunnuste definitsioonid, siis vaata, et KMI arvutus eelneks kaalugruppide määramisele, sest siis on KMI väärtust juba vaja kasutada.
- KMI  arvutusvalem: (kaal kilogrammides) / (pikkus meetrites)^2

*** =pre_exercise_code
```{r}
A <- read.csv2("http://kodu.ut.ee/~annes/R/A.csv", nrows = 45)

```



*** =sample_code
```{r}
# Vaata andmestik üle
head(A)


# Ülesanne 1: aktiveeri pakett
_____________


# Ülesanne 2: lisa tunnused
A1 <- mutate(___________________)


# Ülesanne 3: vaata tulemust
____________


```

*** =solution
```{r}
# Vaata andmestik üle
head(A)


# Ülesanne 1: aktiveeri pakett
library(plyr)
 


# Ülesanne 2: lisa tunnused
A1 <- mutate(A, 
            kmi = kaal/(kasv/100)^2, 
            kaalugrupp = ifelse(kmi <= 25, "ala voi normkaal", "ylekaal"))


# Ülesanne 3: vaata tulemust
str(A1)
```





*** =sct
```{r}
# 1
test_function(name = "library", 
              args = "package",
              index = 1,
              eval = FALSE,
              eq_condition = "equivalent",
              not_called_msg = "Esimeses ülesandes pead kasutama funktsiooni `library()`.",
              args_not_specified_msg = "Käsule `library()` tuleb argumendiks anda paketi nimi.",
              incorrect_msg =  "Käsu `library()` argumendiks anna paketi nimi  `plyr`")



# 2
test_function(name = "mutate", 
              args = ".data",
              index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Teises ülesandes pead kasutama funktsiooni `mutate()`.",
              args_not_specified_msg = "Käsule `mutate()` tuleb  esimeseks argumendiks anda andmestiku nimi `A`.",
              incorrect_msg =  "Käsule `mutate()` antud andmestik on vale.")




test_function(name = "ifelse", 
              args = c("test", "yes", "no"),
              index = 1,
              eval = c(F, T, T),
              eq_condition = "equivalent",
              not_called_msg = "Teises ülesandes pead kasutama funktsiooni `ifelse()`.",
              args_not_specified_msg = paste("Käsu `ifelse()` argumentidest peab " ,
                                c("esimene olema loogiline test", 
                                "teine  määratav väärtus, kui loogilise testi tulemus on `TRUE`", 
                                "kolmas olema `FALSE` tulemuse korral määratav väärtus."))
                                ,
              incorrect_msg =  paste("Käsule `ifelse()` on antud vale väärtus ", c("esimeseks", "teiseks", "kolmandaks"), " argumendiks.")   )




test_data_frame("A1", columns = c("kmi", "kaalugrupp"),
            undefined_msg = "Andmetabel `A1` on defineerimata.",
            undefined_cols_msg = paste("Andmestikus `A1` on mingi veerg puudu, võibolla on veeru nimi vale."),
            incorrect_msg = "Andmetabelis `A1` on mingi veeru väärtused valed või on veeru nimi vale. Proovi uuesti." )

 

# 3
test_function(name = "str", 
              args = "object",
              index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Viimases ülesandes pead kasutama funktsiooni `str()`.",
              args_not_specified_msg = "Käsule `str()` tuleb argumendiks anda andmestiku nimi.",
              incorrect_msg =  "Käsu `str()` argumendiks on vale andmestik.")

			  
			  

success_msg("Tubli!")               
       
            

###########################################################
```
























--- type: NormalExercise key: d7c62aafff lang: r xp: 100 skills: 1
## Grupikokkuvõtete arvutamine



Paketi **plyr** funktsiooni `summarise()` saab kasutada andmete agregeerimiseks. <br><br>



Töölaual on olemas andmestik `A1` eelmisest ülesandest. Andmestikus on kirjas 40 inimese id-kood, sugu, elukoht, vanus, pikkus, kaal, käte siruulatus ning arstivisiidi toimumine. Lisatud on KMI ja kaalugrupi tunnus. 


Aktiveeritud on pakett **plyr**.


*** =instructions
- **Ülesanne 1** Kasutades funktsiooni `ddply()` leia tabel, kus soo ja elukoha gruppides oleks esitatud uuritavate arv (`n`), keskmine vanus (`kesk.vanus`), keskmine KMI (`kesk.kmi`) ja  arstivisiidil käinute osakaal (`visiit.osak`). Tulemuseks olevas tabelis peaks esimeseks veeruks olema soo tunnus, teiseks elukoht.
- **Ülesanne 2** Milline grupp on kõige madalama arstivisiidil käinute osakaaluga? Omista selle grupi koodid (0 või 1)  vastusesse.

*** =hint
- Kahe grupeeriva tunnuse määramiseks esita need funktsioonile `ddply()` kujul `c("tunnus1", "tunnus2")` või `.(tunnus1, tunnus2)`.
- Käsus `ddply()` tuleb funktsioonina kasutada `summarise`, sest arvutada tuleb grupikokkuvõtted. 

*** =pre_exercise_code
```{r}
library(plyr)
A <- read.csv2("http://kodu.ut.ee/~annes/R/A.csv", nrows = 45)
A1 <- mutate(A, kmi = kaal/(kasv/100)^2, kaalugrupp = ifelse(kmi <= 25, "ala- või normaalkaal", "ylekaal"))
rm(A)
```

*** =sample_code
```{r}
# Vaata andmestik üle
str(A1)


# Ülesanne 1: leia tabel
tabel <- ddply(A1, __________________________________________)
tabel

# Ülesanne 2: leia mis grupp on kõige madalama arstivisiidil käimise osakaaluga.
madal <- list(sugu = ___, elukoht = ___)
```

*** =solution
```{r}
# Vaata andmestik üle
str(A1)


# Ülesanne 1: leia tabel
tabel <- ddply(A1, c("sugu", "elukoht"), summarise, 
                n = length(sugu), 
                kesk.vanus = mean(vanus), kesk.kmi = mean(kmi), 
                visiit.osak = sum(visiit)/length(visiit))
tabel

# Ülesanne 2: leia mis grupp on kõige madalama arstivisiidil käimise osakaaluga.
madal <- list(sugu = 1, elukoht = 0)
```

*** =sct
```{r}

# 1
test_or(
test_function("ddply",
              args = c(".data", ".fun", ".variables"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `ddply()`.",
              args_not_specified_msg = paste("Funktsioonis `ddply()` pead määrama  argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtuse"),
              incorrect_msg = paste("Funktsioonis `ddply()` on argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtus vale.")), 
test_function("ddply",
              args = c(".data", ".fun", ".variables"), index = 1,
              eval = c(TRUE, TRUE, NA),
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `ddply()`.",
              args_not_specified_msg = paste("Funktsioonis `ddply()` pead määrama  argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtuse"),
              incorrect_msg = paste("Funktsioonis `ddply()` on argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtus vale."))
)
       
       
test_data_frame("tabel",
                columns = c("sugu", "elukoht",  "n",  "kesk.vanus", "kesk.kmi", "visiit.osak"),
                eq_condition = "equivalent",
                undefined_msg = "Tabelit `tabel` pole tekitatud!",
                undefined_cols_msg = paste("Tabelis `tabel` on mõni nõutud veerg puudu või on veeru nimi vale! "),
                incorrect_msg = paste("Tabelis `tabel` on mõni veerg valede väärtustega või on veeru nimi vale!"))
                
                
 

# 2
test_object("madal",
            eq_condition = "equivalent",
            eval = TRUE,
            undefined_msg = "List `madal` on tekitamata.",
            incorrect_msg = "Listis `madal`  on mingi väärtus vale.")



success_msg("Väga tubli!")               
       
            

###########################################################
```



























--- type: NormalExercise key: ad2cd178ec lang: r xp: 100 skills: 1
## Grupipõhiste näitajate lisamine andmestikku
 


 


Töölaual on olemas andmestik `A1` üleeelmisest ülesandest. Andmestikus on kirjas 40 inimese id-kood, sugu, elukoht, vanus, pikkus, kaal, käte siruulatus ning arstivisiidi toimumine. Lisatud on KMI ja kaalugrupi tunnus. 


Aktiveeritud on pakett **plyr**.


*** =instructions
- **Ülesanne 1** Kasutades funktsiooni `ddply()` moodusta andmetabel `A2`, kus lisaks olemasolevatele tunnustele oleks lisatud kaks grupipõhist tunnust. Tunnus `n`, mis näitaks soo ja elukoha grupi(kuhu uuritav kuulub) mahtu ning tunnus `kesk.kmi`, mis näitaks keskmist KMI väärtust soo ja elukoha grupis, kuhu uuritav kuulub. Mõtle, kumba funktsiooni peaks kasutama: kas `mutate()` või `summarise()`?
- **Ülesanne 2** Kasutades funktsiooni `arrange()` sorteeri vaatlused soo, elukoha ja id-koodi tunnuste põhjal. Tulemus omista muutujale `A3`.
- **Ülesanne 3** Kasutades funktsiooni `dlply()`  moodusta andmestikust `A3` list, mille elementideks oleks vastavalt soo ja elukoha gruppidele tekitatud alamandmestikud, kuhu lisa veel üks tunnus nimega `nr`, mis näitaks inimese järjekorranumbrit (id-koodi järgi) soo ja elukoha grupi sees. Järjekorranumbri loomiseks kasuta funktsiooni `seq_along()`. Selle ülesande lahenduse kirjapanek on sarnane esimese ülesande juhuga. Erinev on *apply* -funktsioon, mis annab tulemuseks mitte andmetabeli vaid listi, samuti on erinev tunnus, mida peab lisama.


*** =hint
- Käsu `mutate()` abil saab lisada olemasolevasse andmestikku tunnuseid (või ka kustutada tunnuseid), see ei muuda andmestiku ridade arvu. Nii esimeses kui kolmandas ülesandes sobiks see funktsioon.
- Järjekorranumbri lisamiseks võtta aluseks järjestatud andmestik. Lisatav tunnus on defineeritav nii: `nr = seq_along(sugu)`.

*** =pre_exercise_code
```{r}
library(plyr)
A <- read.csv2("http://kodu.ut.ee/~annes/R/A.csv", nrows = 45)
A1 <- mutate(A, kmi = kaal/(kasv/100)^2, kaalugrupp = ifelse(kmi <= 25, "ala voi normkaal", "ylekaal"))
rm(A)
```

*** =sample_code
```{r}
# Vaata andmestik üle
str(A1)


# Ülesanne 1: lisa uued tunnused andmestikku
A2 <- ddply(A1, __________________________________________)
head(A2)

# Ülesanne 2: sorteeri
A3 <- arrange(_____________)

# Ülesanne 3: lisa järjekorranumbri tunnus
A4 <- dlply(______________________________________________)
head(A4)

```

*** =solution
```{r}
# Vaata andmestik üle
str(A1)


# Ülesanne 1: lisa uued tunnused andmestikku
A2 <- ddply(A1, c("sugu", "elukoht"), mutate, n = length(sugu),  kesk.kmi = mean(kmi))
head(A2)

# Ülesanne 2: sorteeri
A3 <- arrange(A2, sugu, elukoht, id)

# Ülesanne 3: lisa järjekorranumbri tunnus
A4 <- dlply(A3, c("sugu", "elukoht"), mutate, nr = seq_along(sugu))
A4
```

*** =sct
```{r}
# 1
test_or(
test_function("ddply",
              args = c(".data", ".fun", ".variables"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `ddply()`.",
              args_not_specified_msg = paste("Funktsioonis `ddply()` pead määrama  argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtuse"),
              incorrect_msg = paste("Funktsioonis `ddply()` on argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtus vale."))
              , 
test_function("ddply",
              args = c(".data", ".fun", ".variables"), index = 1,
              eval = c(TRUE, TRUE, NA),
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `ddply()`.",
              args_not_specified_msg = paste("Funktsioonis `ddply()` pead määrama  argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtuse"),
              incorrect_msg = paste("Funktsioonis `ddply()` on argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtus vale."))
)
       
       

       
       
       
test_data_frame("A2",
                columns = c("sugu", "elukoht",  "n",  "kesk.kmi" ),
                eq_condition = "equal",
                undefined_msg = "Andmestikku `A2` pole tekitatud!",
                undefined_cols_msg = paste("Andmestikus `A2` on mõni nõutud veerg puudu! Võibolla on veeru nimi vale? "),
                incorrect_msg = paste("Andmestikus `A2` on mõni veerg valede väärtustega või on veerunimed valed."))
                
# 2
test_function("arrange",
              args = "df", index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "teises ülesadnes kasuta funktsiooni `arrange()`.",
              args_not_specified_msg = "Funktsioonile `arrange()` anna esimeseks argumendiks sorteeritav andmestik, järgmiseks loetle tunnused, mille järgi sorteerida.",
              incorrect_msg = "Funktsioonile `arrange()` anna esimeseks andmestik `A2`, edasi tunnusenimed `sugu, elukoht, id`. Kontrolli ka, mis järjekorras on andmestiku `A2` veerud.")


#test_student_typed("A2, sugu, elukoht, id)", not_typed_msg = "Kontrolli, kas nimetasid sorteerimiskäsus kõik nõutud tunnused ja kas need on õiges järjekorras.")

test_data_frame("A3",
                columns = c("sugu", "elukoht",  "n",  "kesk.kmi" ),
                eq_condition = "equivalent",
                undefined_msg = "Andmestikku `A3` pole tekitatud!",
                undefined_cols_msg = paste("Andmestikus `A3` on mõni nõutud veerg puudu! "),
                incorrect_msg = paste("Andmestikus `A3` on midagi valesti, kontrolli kas sorteerisid andmestiku nii nagu vaja?"))
                
                
                
                
                
                
                
# 4
test_or(
test_function("dlply",
              args = c(".data", ".fun", ".variables"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta viimases ülesandes funktsiooni `dlply()`.",
              args_not_specified_msg = paste("Funktsioonis `dlply()` pead määrama  argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtuse"),
              incorrect_msg = paste("Funktsioonis `dlply()` on argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtus vale.")), 
test_function("dlply",
              args = c(".data", ".fun", ".variables"), index = 1,
              eval = c(TRUE, TRUE, NA),
              eq_condition = "equivalent",
              not_called_msg = "Kasuta viimases ülesandes funktsiooni `dlply()`.",
              args_not_specified_msg = paste("Funktsioonis `dlply()` pead määrama  argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtuse"),
              incorrect_msg = paste("Funktsioonis `dlply()` on argumendi " , c("`.data`", "`.fun`", "`.variables`"), "väärtus vale."))
)
       

 

test_object("A4",
            eq_condition = "equal",
            eval = TRUE,
            undefined_msg = "Listi `A4` pole tekitatud!",
            incorrect_msg = "List `A4` on valede elementidega.")




success_msg("Väga tubli!")       

```

































--- type: NormalExercise key: abdce39411 lang: r xp: 100 skills: 1
## Mitmele tunnusele kirjeldavate karakteristikute leidmine 1
 
 


Paketi **plyr** abil saab mugavalt teha täiendusi andmestikule või agregeerida andmeid. Paketis on ka mitmeid abifunktsioone, mis võtavad argumendiks funktsiooni(d) ja väljastavad uue funktsiooni. Näiteks funktsiooni `each()` abil saab mitu funktsiooni kokku kombineerida üheks:
```{r, eval =T}
x <- 1:5
each(prod, sum)(x)

prod  sum 
 120   15 
```
Kasulik on see seetõttu, et paketi *apply*-käsud  `?!ply` nõuavad argumendina ühte funktsiooni.<br><br>


Töölaual on olemas andmestik `B`. Andmestikus on 160 inimese kohta mitmete testide ja ankeetküsitluse tulemused ning taustatunnuste väärtused.

Aktiveeritud on pakett **plyr**.



*** =instructions
- **Ülesanne 1** Moodusta tõeväärtustega vektor, mille abil saaks andmestikust `B` valida need veerud, mille nimi algab sõnaga *test*. Sõne alguse kontrolliks kasuta funktsiooni `startsWith()`. Omista  tõeväärtustega vektor muutujale `testid`.
- **Ülesanne 2** Ava abifailid funktsioonide `ldply()` ja  `each()` kohta, vaata üle funktsioonide argumendid.
- **Ülesanne 3** Täienda antud koodi nii, et tulemuseks olevas tabelis oleks iga testitulemuse tunnuse kohta leitud keskväärtus, standardhälve, miinimum ja maksimum. Kasuta ära esimeses ülesandes moodustatud tõeväärtuste vektor.  Iga testitulemuse tunnus peaks määrama tulemuseks olevas tabelis ühe rea, mille esimene väärtus on vastava tunnuse nimi. Tunnusenime veeru nimi olgu `"Tunnus"`. Järgmised veerud tabelis peaks olema keskväärtused, standardhälbed, miinimumid ja maksimumid. 

*** =hint
- Kolmandas ülesandes saab testi nime veeru määramiseks kasutada argumenti `.id`: `.id = "Tunnus"`.
- Funktsioonid saab `ldply()` käsule ette anda `each()` funktsiooniga: `each(mean, sd, min, max)`.

 
*** =pre_exercise_code
```{r}
B <- read.csv2("http://kodu.ut.ee/~annes/R/B.csv", nrows = 160)
library(plyr)

```

*** =sample_code
```{r}
# Vaata üle tunnusenimed ja andmestiku dimensioonid
names(B)
dim(B)

# Ülesanne 1: Moodusta tõeväärtustega vektor
testid <- __________________________________

# Ülesanne 2: Vaata abifaile
_____________
_____________

# Ülesanne 3: Täienda koodi
tabel <- ldply(______________, .fun = ______________, .id = ________________)
tabel[order(tabel$Tunnus),]

```

`@solution`
```{r}
# Vaata üle tunnusenimed ja andmestiku dimensioonid
names(B)
dim(B)

# Ülesanne 1: Moodusta tõeväärtustega vektor
testid <- startsWith(names(B),  "test")

# Ülesanne 2: Vaata abifaile
?each
?ldply

# Ülesanne 3: Täienda koodi
tabel <- ldply(B[, testid], .fun = each(mean, sd, min, max), .id = "Tunnus")
tabel[order(tabel$Tunnus),]
```

`@sct`
```{r}

# 1.
test_function("startsWith",
              args = c("x", "prefix"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta esimeses ülesandes funktsiooni `startsWith()`.",
              args_not_specified_msg = paste("Funktsioonis `startsWith()` pead määrama  argumendi " , c("`x`, vektori kontrollitavate tunnusenimedega", "`prefix` ehk mis sõnega tunnuse nimed peaks algama. ")),
              incorrect_msg = paste("Funktsioonis `startsWith()` on argumendi " , c("`x`", "`prefix`"), "väärtus vale. See peaks olema", c(" vektor tunnuste nimedega.", " tekst `test`.")))
       


test_object("testid",
            eq_condition = "equivalent",
            eval = TRUE,
            undefined_msg = "Tõeväärtustega vektor `testid` on tekitamata",
            incorrect_msg = "Tõeväärtustega vektori `testid`  väärtused on valed")
            
# 2
test_or(
test_student_typed("?each"), 
test_student_typed("?each()"), 
test_student_typed("help(each)"), 
test_student_typed("help('each')"), 
incorrect_msg = "Kas avasid käsu `each()` abifaili?" )
        
            
            
test_or(
test_student_typed("?ldply"), 
test_student_typed("?ldply()"), 
test_student_typed("help(ldply)"), 
test_student_typed("help('ldply')"), 
incorrect_msg = "Kas avasid käsu `ldply()` abifaili?" )
      
       
       
# 3
test_function("ldply",
              args = c(".data", ".fun", ".id"), index = 1,
              eval = TRUE,
              eq_condition = "equivalent",
              not_called_msg = "Kasuta kolmandas ülesandes funktsiooni `ldply()`.",
              args_not_specified_msg = paste("Funktsioonis `ldply()` pead määrama  ka argumendi " , c("`.data`", "`.fun`", "`.id`"), "väärtuse"),
              incorrect_msg = paste("Funktsioonis `ldply()` on argumendi " , c("`.data`", "`.fun`", "`.id`"), "väärtus vale."))
       
       
test_data_frame("tabel",
                columns = c("Tunnus", "mean", "sd", "min", "max"),
                eq_condition = "equivalent",
                undefined_msg = "Tabelit `tabel` pole tekitatud!",
                undefined_cols_msg = paste("Tabelis `tabel` on mõni nõudud veerg puudu! "),
                incorrect_msg = paste("Tabelis `tabel` on mõni veerg valede väärtustega!"))
                
                
                
                
success_msg("Ülesanne tehtud. Tubli!")               
       
            

```


 





