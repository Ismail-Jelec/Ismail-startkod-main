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
