# Azure API Management - Warsztat zapoznawczy - Lab 3

- [Spis treści](README.md)
- [Lab 1 - Utworzenie instancji API Management](apimanagement-1.md)
- [Lab 2 - Portal dewelopera i tworzenie produktów](apimanagement-2.md)
- [Lab 3 - Konfiguracja API](apimanagement-3.md)
- [Lab 4 - Wyrażenia polityk API](apimanagement-4.md)
- [Lab 5 - Wersjonowanie, rewizje](apimanagement-5.md)
- [Lab 6 - Monitorowanie usługi](apimanagement-6.md)
- [Lab 7 - Aspekty bezpieczeństwa](apimanagement-7.md)
- [Lab 8 - Self-hosted gateway](apimanagement-8.md)
- [Lab 9 - FusionDev](apimanagement-9.md)

## API Management

Każde API dodane do API Management może być nastepnie dodane do produktu i opublikowane na Developer Portalu. Od tego momentu developerzy będą w stanie je testować i używać zgodnie z opisanymi przez nas politykami.

### Zakładka APIs

W Azure Portalu wybierze z menu zakładkę `APIs`. Zobaczysz listę skonfigurowanych API. W tym miejscu możesz dodawać, usuwać i modyfikować API.

![List](Images/APIMListAPIs.png)

![Add](Images/APIMAddAPIs.png)

### Ręczne dodanie API bez kontraktu

Zamiast implementwać własne API użyjemy gotowego `Star Wars API` <https://swapi.dev>.

- Kliknij `Add API`
  - Wybierz `Add Blank API`
  - Wybierz wersję `Full` na górze okna
  - Wpisz nazwę i opis API
  - Wpisz następujący `Backend Service`: <https://swapi.dev/api>
  - Wpisz API URL jako `sw`
  - Przypisz API do produktu, np: Started albo Unlimited
  - Stwórz API

![Blank](Images/APIMAddBlankAPI.png)

Po utworzeniu wybierz z listy `Start Wars API`

![Add](Images/APIMAddStarWars.png)

Stwórzmy następujące operacje:

- **GetPeople** GET /people/ ... używaj małych znaków
- **GetPeopleById** GET /people/{id}/ ... używaj małych znaków

![Get People](Images/APIMAddSWGetPeople.png)

![Get People by Id](Images/APIMAddSWGetPeopleById.png)

![Add](Images/APIMAddSWOperations.png)

Następny krok to konfiguracja CORS dla naszego API

Wybierz `Star Wars API`, a następnie `All Operations`. W sekcji `Inbound processing` wybierze [Add policy]

![CORS](Images/APIMSWCORS1.png)

Wybierz `CORS`

![CORS](Images/APIMCORS2.png)

Ustaw CORS tak, jak na załączonym zrzucie ekranu. W rzeczywistości CORSy powinny być ustawione pod konkretne wymagania, w tym miejscu robimy uproszczoną konfigurację na potrzeby demo.

![CORS](Images/APIMCORS3.png)

Po zapisaniu polityki możemy przejrzeć jej kod uzywając [Code View]

![INSPECT](Images/APIMCORS4.png)

Uruchom ponownie Developer Portal

- Zaloguj się jako deweloper z aktywną subskrypcją do produktu w którym zostało skonfigurowane API.
- Wybierz `Start Wars API`

![TryIt](Images/APIMSWTryIt1.png)

- Przetestuj działanie operacji `GetPeople`
- Przetestuj działanie operacji `GetPeopleById` używając id = 2

![TryIt 2](Images/APIMSWTryIt2.png)

Przyjrzyj się odpowiedzi z serwisu

- Status odpowiedzi
- Informacje na temat C-3PO w Body uzyskanej odpowiedzi.

![TryIt 3](Images/APIMSWTryIt3.png)

#### Zaimportuj API za pomocą Swaggera

Zamiast ręcznie definiować metody API możemy je zaimportować automatycznie do usługi. Standard [OpenAPI](https://www.openapis.org/) (aka [Swagger](https://swagger.io)) służy do opisu kontraktów API RESTowych.

Na potrzeby demo użyjemy API prostego kalkulatora : [Calc API](http://calcapi.cloudapp.net/)

![Calc API](Images/APIMCalcAPI.png)

Przejdź do zakładki `APIs` i wybierze `Add OpenAPI Specification`

- Ustaw URL definicji na <http://calcapi.cloudapp.net/calcapi.json>
  - Niektóre pola powinny się automatycznie zczytać z definicji
  - Jako API suffix wpisz `calc`
  - Przypisz API do produktu, np: Started albo Unlimited

![Add Calc](Images/APIMAddCalcAPI1.png)

![Add Calc](Images/APIMAddCalcAPI2.png)

- Skonfiguruj CORSy podobnie jak w poprzednim API
- Zanjdź nowe API w Developer Portalu
  - Wypróbuj API (np. dodawanie dwóch liczb całkowitych)
- Przyjrzyj się uzyskanej odpowiedzi

![TryIt](Images/APIMCalcTryIt1.png)

Możesz przejrzeć całą konfigurację API poprzez naciśnięcie `Edit` w sekcji Frontend:

![View Swagger](Images/APIMCalcSwagger.png)

![View Swagger 2](Images/APIMCalcSwagger2.png)

Trzeci przykład to Colors API [Colors API](https://markcolorapi.azurewebsites.net/swagger/)

![Colors](Images/APIMColorAPI.png)

- Stwórz nowe API przez import definicji z następującego adresu: <https://markcolorapi.azurewebsites.net/swagger/v1/swagger.json>
  - Jako API suffix użyj "color".

![Add](Images/APIMAddColorAPI1.png)

![Add](Images/APIMAddColorAPI2.png)

- Uzupełnij CORSy

W tym przypadku Swagger nie zawierał nazwy hosta:

- Wejdź w `Settings`
- Uzupełnij `Web Service URL` jako <https://markcolorapi.azurewebsites.net>

![Settings](Images/APIMAddColorAPI3.png)

- Przetetsuj działanie API w zakładce `Test`

![Test](Images/APIMAddColorAPI.png)

#### Rate limit

Wejdź na [stronę Colors](https://markcolorweb.azurewebsites.net) wyświetlającą 500 świateł. Każde ze świateł w losowych odstępach czasu odpytuje RandomColor API i wyświetla otrzymany kolor.

![Web](Images/APIMColorWeb.png)

- W menu strony wstaw adres swojego API

![Config](Images/APIMColorWebConfig.png)

- W tym celu przygotuj następujący URL:

  - <https://YOURAPIM.azure-api.net/color/api/RandomColor?key=> _Starter-Key_
  - <https://YOURAPIM.azure-api.net/color/api/RandomColor?key=> _Unlimited-Key_

- Produkt Unlimited nie ma limitów

![No limit](Images/APIMColorWebUnlimited.png)

- Produkt Starter ma limit 5 wywołań na minutę.
- Powinien pojawić się błąd, np: _{ "statusCode": 429, "message": "Rate limit is exceeded. Try again in 54 seconds." }_

![Error](Images/APIMColorWebStarter.png)

---

[Spis treści](README.md) | [Lab 2 - Portal dewelopera i tworzenie produktów](apimanagement-2.md) | [Lab 4 - Wyrażenia polityk API](apimanagement-4.md)
