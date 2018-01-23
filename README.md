ZAJECIA 3

1. Biblioteki
install.packages("ggplot2")
library(ggplot2)
library(tidyverse)

2. Dane
data(diamonds)
diamonds <- sample_frac(diamonds, 0.2)

3. Eksploracja danych
str(diamonds)
summary(diamonds)

4. Histogram
hist(diamonds$carat)
hist(diamonds$carat, col = "red")
hist(diamonds$carat, col = "red", breaks = 20)
hist(diamonds$carat, col = "red", breaks = 50)
hist(diamonds$carat, col = "red", breaks = 100)
hist(diamonds$carat, col = "red", breaks = 500)

5. Wykresy bazowe
plot(diamonds$carat, diamonds$price)
plot(diamonds$carat, diamonds$price, col = "blue", pch = 20,
     xlim = c(0, 8), ylim = c(0, 20000), 
     xlab = "masa [karaty]", ylab = "cena [USD]",
     main = "Diamenty: zaleznosc ceny od masy")
     
     -> każdy wykres składa się z:
ggplot(diamonds)   +  # z odwolania do zbioru danych 
geom_point( # warstw ~ 'ksztaltow'
  aes( x = carat, y = price) # przyporzadkowania zmiennych do osi wewnatrz warstwy 
)

**Linia zamiast punktów:
ggplot(diamonds)   +
  geom_line( 
    aes( x = carat, y = price, color = "red") 
  )
  
  ** Linia trendu:
  
  ggplot(diamonds, aes( x = carat, y = price) )   +
  geom_point( ) +
  geom_smooth( 
    aes( x = carat, y = price) 
  )

Linia trendu prosta:
ggplot(diamonds, aes( x = carat, y = price) )   +
  geom_point( ) +
  geom_smooth( 
    aes( x = carat, y = price),
    method = "lm"
  )  
linia trendu krzywa: 
ggplot(diamonds,  aes( x = carat, y = price) ) +
  geom_point( ) +
  geom_smooth(method = "loess") 
  
  6. wykres pudełkowy
  
  ggplot(diamonds, aes(x = cut, y = price)) +
  geom_boxplot()
  
  7. wykres słupkowy
  diamondsMean <- diamonds %>% 
  group_by(., clarity) %>% 
  summarise(., price = mean(price))

8. średnia cena w podziale na szlif i czystość - wykres

ggplot(diamondsMeanCut) + 
  geom_bar( aes(x = clarity, y = price, fill = cut ),
            stat = "identity") 

ggplot(diamondsMeanCut) + 
  geom_bar( aes(x = clarity, y = price, fill = cut ),
            stat = "identity",
            position = "dodge")
                     
9. histogram
ggplot(diamonds) + 
  geom_histogram( aes(x = carat )) 

ggplot(diamonds) + 
  geom_histogram( aes(x = carat), binwidth = 0.02) 
  
  10. wykres gęstości
  
  ggplot(diamonds) + 
  geom_density( aes(x = price )) 
  
  -> w zależności od czystości:
  ggplot(diamonds) + 
  geom_density( aes(x = price, fill = clarity)) 
  
  -> przejrzystosć, parametr alpha
ggplot(diamonds) + 
  geom_density( aes(x = price, fill = clarity), alpha = 1) 
  
  11. Parametry warstw wykresów:
  
 * col
 ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price,
         col = clarity)
  )
  ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price), 
    col = "blue"
    )
    
 * fill => col = obwod slupków
 ggplot(diamonds)   +  
  geom_smooth( 
    aes( x = carat, y = price,
         fill = clarity)
  )
  
  *shape
  ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price,
         col = clarity,
         shape = cut)
  )
  
  *size
  ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price,
         size = z),
    alpha = 0.4
  )
* typ linii
ggplot(diamondsMeanCut)   +  geom_line(aes( x = clarity , y = price , linetype = cut, group = cut))
  
12. Grupowanie wykresów
* 1 wykres
ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price)) 

* wykres 3ymiarowy - poziomo (4wykresy) -> 4 osobe wykresy dla zmiennej cut
ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price)) +
    facet_grid(cut ~. )

*wykres 3wymiarowy pionowo (4 wykresy)
ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price)) +
  facet_grid(. ~ cut )
  
* wykres 4 wymiarowy (4 wykresy)

ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price)) +
  facet_grid(clarity ~ cut )
  
  13. Wygląd
  
  * wielkosć
  ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price,
         col = clarity)
  ) + 
  theme_bw(base_size = 16)
  
  * opis
  ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price,
         col = clarity)
  ) + 
  theme_bw(base_size = 16) +
  xlab("Karaty") +
  ylab("Cena") +
  theme(axis.text.x = element_text(face="bold", color="#993333", 
                                   size=14, angle=45))
 
 * zmiany na osiach
 ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price,
         col = clarity)
  ) + 
  theme_bw(base_size = 16) +
  xlab("Karaty") +
  ylab("Cena") +
  theme(axis.text.x = element_text(face="bold", color="#993333", 
                                   size=14, angle=45))
                                   
 *zmiany legendy 
 
 ggplot(diamonds)   +  
  geom_point( 
    aes( x = carat, y = price,
         col = clarity)
  ) + 
  xlab("Karaty") +
  ylab("Cena") +
  theme_bw(base_size = 16)  +
  scale_color_discrete(name = "Czystosc")
  
  14. eksport wykresów
  
  pdf("diamonds.pdf", width = 10, height = 8)

print(
  ggplot(diamonds)   +  
        geom_point( 
          aes( x = carat, y = price))
      )

dev.off()


ZADANIE 1
Dla danych o samochodach (cars) narysuj wykres punktowy dystansu hamowania w zaleznosci od predkosci, uzywajac:
a) funkcji bazowych,
b) ggplot.
W podpunkcie b) dopasuj linie trendu (na rozne sposoby). Wybierz najlepsza, wybor uzasadnij.
Nadaj wykresom tytuly i opisz osie (po polsku).

data(cars)
cars <- cars

str(cars)
summary(cars)

plot(x = cars$speed, y = cars$dist,
     xlab = "Predkosc", ylab = "Dystans hamowania", 
     main = "Wykres punktowy dystansu hamowania\n w zaleznosci od predkosci")

library(ggplot2)

plot_gg <- ggplot(cars, aes( x = speed, y = dist) )   +
  geom_point( ) +
  xlab("Predkosc") +
  ylab("Dystans hamowania") +
  ggtitle("Wykres punktowy dystansu hamowania w zaleznosci od predkosci")

plot_gg


plot_gg + geom_smooth(method = "lm")
plot_gg + geom_smooth(method = "glm")
plot_gg + geom_smooth(method = "gam")
plot_gg + geom_smooth(method = "loess")
plot_gg + geom_smooth(method = "loess", span = 2)


ZADANIE 2

Dla danych iris zwizualizuj zaleznosci pomiedzy wymiarami elementow kwiatow w zaleznosci od gatunku, uzywajac pakietu ggplot.
Nadaj wykresom tytuly i opisz osie (po polsku).
Sprobuj roznych (co najmniej 2) podejsc do wizualizacji danych. 
Wskaz sposob, ktory Twoim zdaniem bedzie najbardziej czytelny dla osoby niezaznajomionej ze statystykÄ… i programowaniem.
Zwiazki do pokazania (w zaleznosci od gatunku):
a) Dlugosc kwiatu a szerokosc kwiatu
b) Dlugosc liscia a szerokosc liscia
c) Dlugosc liscia a dlugosc kwiatu
(Sepal oznacza lisc, petal - kwiat)

data(iris)
iris <- iris
str(iris)
summary(iris)

a) długość a szerokość
* 3 wykresy 

ggplot(iris)   +  
  geom_point( 
    aes( x = Petal.Width, y = Petal.Length)) +
  facet_grid(. ~ Species) +
  xlab("Szerokosc kwiatu") +
  ylab("Dlugosc kwiatu ") +
  ggtitle("Dlugosc kwiatu a szerokosc kwiatu")
  
 * 1 wykres x3 wymiary 
 
  ggplot(iris)   +  
  geom_point( 
    aes( x = Petal.Width, y = Petal.Length,
         col = Species)
  ) +
  xlab("Szerokosc kwiatu") +
  ylab("Dlugosc kwiatu") +
  ggtitle("Dlugosc kwiatu a szerokosc kwiatu") +
  labs(color='Gatunek') 
  
            
 b) długoś a szerokość liścia
 * 3 wykresy
 ggplot(iris)   +  
  geom_point( 
    aes( x = Sepal.Width, y = Sepal.Length)) +
  facet_grid(. ~ Species) +
  xlab("Szerokosc liscia") +
  ylab("Dlugosc liscia") +
  ggtitle("Dlugosc liscia a szerokosc liscia")
  
  * 1 wykres 3 wymiary
  ggplot(iris)   +  
  geom_point( 
    aes( x = Sepal.Width, y = Sepal.Length,
         col = Species, shape = Species)
  ) +
  xlab("Szerokosc liscia") +
  ylab("Dlugosc liscia") +
  ggtitle("Dlugosc liscia a szerokosc liscia") +
  labs(color='Gatunek', shape = "Gatunek") 


c)długość liścia a długoś kwiatu
* 3 wykresy
ggplot(iris)   +  
  geom_point( 
    aes( x = Petal.Length, y = Sepal.Length)) +
  facet_grid(. ~ Species) +
  xlab("Dlugosc kwiatu") +
  ylab("Dlugosc liscia") +
  ggtitle("Dlugosc liscia a dlugosc kwiatu")
* 1 wykres

ggplot(iris)   +  
  geom_point( 
    aes( x = Petal.Length, y = Sepal.Length,
         col = Species, shape = Species)
  ) +
  xlab("Dlugosc kwiatu") +
  ylab("Dlugosc liscia") +
  ggtitle("Dlugosc liscia a dlugosc kwiatu") +
  labs(color='Gatunek', shape = "Gatunek") 
  
  
  Dodatkowa informacja o podziale na gatunki jest zwykle czytelniejsza, 
  gdy przedstawia sie ja za pomoca koloru, nie na osobnych wykresach,
  ulatwia to porowanywanie zaleznosci pomiedzy gatunkami. Gdy punkty nakladaja sie na siebie, oprocz zmiennego koloru
  warto rozwazyc dodanie zmiennego ksztaltu punktow.
  
  
  
