# Klasy i domknięcia w C#

Od jakiegoś czasu interesuje się FP (Functional Programming) oraz tym jak zastosować jego dobrodziejstwa w językach, których główną naturą napewno nie jest FP. Mam głównie na myśli C#. Wśród wielu terminów związanych z FP (higer order functions, monada, pattern matching) przewija się **domknięcie**; 

Często w różnych blogach pojawia się stwierdzenie, że obiekty to naiwne domknięcia, a domknięcia to naiwna obiekty. 

> Closures are a poor man's object, and objects are a poor man's closure

## Przykład
Można pokusić się na mały eksperyment i spróbować to zaimplementować w C#

Najpierw klasa:

``` cs
class Greeting
{
    private readonly string _prefix = "Hello, ";
    private readonly string _name;
    public Greeting(string name)
    {
        _name = name;
    }

    public string Invoke()
    {
        return _prefix + _name;
    }
}
```

następnie funkcja z domknięciem:

``` cs
Func<string> GreetingFn(string name) 
{
    var prefix = "Hello, ";
    return () => prefix + name;
}
```

A teraz jak to działa:

``` cs
void Main()
{
    // Class
    var greetingObj = new Greeting("You");
    Console.WriteLine( greetingObj.Invoke() ); // Hello, You 

    // Function
    var greetingFn = GreetingFn("You");
    Console.WriteLine( greetingFn.Invoke() ); // Hello, You 
    // W tym wypadku Invoke() jest opcjonalne i można to uprościć do
    // Console.WriteLine( greetingFn() ); 
}
```
## Spostrzeżenia
Różnice są chyba widoczne na pierwszy rzut oka:
- **Wersja oparta na domknięciu jest zdecydowanie krótsza**
- Wywołanie obydwu wersji jest praktycznie identyczne
- Wersja z domknięciem ma troche mniej czytelną składnie (generics i te dziwne funkcje lambda)

Oczywiście implementacja oparta o domknięcie ma swoje ograniczenia (stąd nazwa Poor man :) ) np. można zwrócić tylko jedną funkcję. W przypadku implementacji opartej o klasę możemy bardzo łątwo dodać nową metodę.

Warto się zastanowić gdzie coś takiego możę się przydać, jakie ma własności itp...

PS> Może jako zamiennik dla klas implementujących interfejs z tylko jedną metodą?