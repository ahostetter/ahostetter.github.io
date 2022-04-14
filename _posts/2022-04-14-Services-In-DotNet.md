---
layout: post
title:  "Learning about Services in .Net"
date:   2022-04-14 08:00:00 -0400
categories: jekyll update
---
This is the source for this post: [Worker Services in .NET Core][Worker-Services-in-.NET-Core]

```c#
//If the hero has space in their inventory then randomly select an item
public static void HeroPickupItem(Hero hero)
{
    Console.WriteLine();
    Console.WriteLine("You find something.");

    if (Inventory.inventorySpaceCheck(hero.inventory))
    {
        Random rand = new Random();

        int randomItem = rand.Next(0, 2);

        if (randomItem == 0)
        {
            Console.WriteLine("It's a Health potion");
            hero.inventory.healthPotion = hero.inventory.healthPotion + 1;
            Console.WriteLine("You now have " + hero.inventory.healthPotion + " Health Potions in your Inventory.");
        }
        else if (randomItem == 1)
        {
            Console.WriteLine("It's a Strength potion");
            hero.inventory.strengthPotion = hero.inventory.strengthPotion + 1;
            Console.WriteLine("You now have " + hero.inventory.strengthPotion + " Strength Potions in your Inventory.");
        }
    }
}
```

[Worker-Services-in-.NET-Core]: https://www.youtube.com/watch?v=PzrTiz_NRKA
