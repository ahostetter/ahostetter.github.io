---
layout: post
title:  "Configuring My Tech Blog Site"
date:   2022-04-11 21:04:47 -0400
categories: jekyll update
---
I built this site with jekyll using markdown to create post. I chose this method so that I can practice my documentation with markdown, and because it is a easy way to make site look good. Post are located in the `_posts` directory. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight c# %}
// If hero xp is over 100 then add 1 to Hero level.
public static void HeroLevelCheck(Hero hero, int xp)
{
  hero.xp = hero.xp + xp;

  if (hero.xp >= 100)
  {
    hero.level = hero.level + 1;
    hero.xp = hero.xp - 100;
  }
}
{% endhighlight %}

OR

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

This is the [Jekyll docs][jekyll-docs]. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
