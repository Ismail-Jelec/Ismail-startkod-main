
DEL 1
--------------------------------------------------

Första felet som jag hittade va på raden "items.Add(new Item(parts[1], int.Parse(parts[0])));" i filen ShoppingList.cs och på raden "list.Load();" i Program.cs.

Jag hittade det genom att köra koden och läsa felmedelandet.


jag körde en debug och hitta detta: [alt text](image.png)
jag fixade felet genom att jag la in detta först i void metoden:
```csharp

        if (!File.Exists(path))
        {
            return;
        }


felet i ShoppingList.cs är fortfarande där så jag la in "Console.WriteLine($"[{line}] antal delar: {parts.Length}");" för att se vad som inte funkar


Jag bytte ut dessa rader:
        string text = File.ReadAllText(path);
        string[] lines = text.Split('\n');

Med denna:
        string[] lines = File.ReadAllLines(path);



Jag testa skriva random kod i panelen när jag kör dotnet run och programmet krashar

Jag ändrade från:

    int choice = int.Parse(Console.ReadLine());

till:

    if(int.TryParse(Console.ReadLine(), out int choice))
    {
        
    }

    else
    {
        Console.WriteLine("\nDu måste skriva ett tal");
        continue;    
    }


Jag ändra också dessa filer:

        int price = int.Parse(Console.ReadLine());

och

        int number = int.Parse(Console.ReadLine());


Till detta:
      
        if (!int.TryParse(Console.ReadLine(), out int price))
        {
            Console.WriteLine("\nDu måste skriva ett tal");
            continue;
        }

och

        if (!int.TryParse(Console.ReadLine(), out int number))
        {
            Console.WriteLine("\nDu måste skriva ett tal");
            continue;
        }


Jag märkte att jag kan ta bort vilket tal som helst så jag ändrade metoden "RemoveAt" från:

    public void RemoveAt(int number)
    {
        items.RemoveAt(number - 1);
    }


till:

    public void RemoveAt(int number)
    {
        if(number >= 1 && number <= items.Count)
        {
            items.RemoveAt(number - 1); 
        }
        else
        {
            Console.WriteLine("\nProdukten är inte i listan");
        }
    }



```

När jag la till produkter märkte jag att priset är helt fel, så jag fixa det från detta:
```csharp

    public int Total()
    {
        int sum = 0;

        for (int i = 1; i < items.Count; i++)
        {
            sum += items[i].Price;
        }

        return sum;
    }

till detta:

    public int Total()
    {
        int sum = 0;

        for (int i = 0; i < items.Count; i++)
        {
            sum += items[i].Price;
        }

        return sum;
    }
```
Programmet måste börja på 0 för att 0 är första är första nummret, inte 1



Jag skrev med Claude och den sa till mig att jag ska göra detta:
```csharp

        catch (UnauthorizedAccessException)
        {
            Console.WriteLine("Listan sparades inte. Filen är skrivskyddad.");
        }

        catch (IOException)
        {
            Console.WriteLine("Listan sparades inte. Det gick inte att skriva till filen.");
        }

Jag skulle göra det för att det gamla catch var tomt och "Listan är sparad." skrevs ut även när det inte gick att spara, så nu skrivs den bara ut när det lyckas och annars får användaren veta att listan inte sparades.



DEL 2
--------------------------------------------------


Jag la in detta i item.cs koden:

        if (string.IsNullOrWhiteSpace(name))
        {
            throw new ArgumentException("Namnet får inte vara tomt");
        }

        if (price < 0)
        {
            throw new ArgumentOutOfRangeException(nameof(price), "Priset får inte vara ett negativt nummer");
        }

Koden ger ett undantag om namnet är tomt eller priset är negativt så att det aldrig skapas en vara med ogiltiga värden. Programmet krashar just nu om man anger fel värden