When COVID-19 came around, I had just started a new job at Digicel Trinidad & Tobago. 

![Covid 19 image](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExajZ4NDdkN2V3OWh2Nm5vOW9id2Q4Nmx0YzVraGNjM21vYnU1d3VoaiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/rIlmnpIaVVoxOwD9e0/giphy.gif){: width="150" }

I was working hard to learn the new job, but I also had some extra time at home.

I decided that I was going to use the time to learn something new. The first thing that I could think of was re-learning programming. 
I had some previous history in software using languages such as C++, C# and Java for some previous projects (including TINQA, my undergrad project), but I 
had stopped programming for a few years since I had originally decided to work in networking. Since I knew that most people
in the networking industry were using Python, I decided to continue my programming career by learning Python.

![Python image](https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExOHo3MzdsNmdqZHBhODN0cDkxcDRnZmJzOTlsd2p2NXl4aXhyNzNyZSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/KAq5w47R9rmTuvWOWa/giphy.gif){: width="150" }


Around the same time, I had just started investing in T&T's local stock market (the TTSE). I was pretty excited about that, but I was always disappointed whenever I visited the website of the stock exchange.

Back then, it was a pretty ancient site, and it was always slow. I thought I could solve two problems at once by learning Django/Python, and developing a better website at the same time!

I started by collecting data and building the database. I initially used the **requests** library in Python to navigate to each page that I wanted in the TTSE.
However, this was really slow, and the site admins were starting to set up firewalls for web scraping bots like mine 😁

Someone on LinkedIn eventually showed me that a lot of the historic stock data was actually stored in the javascript charts displayed on the website, so I used those to get a large dump
of all the data.

![Chart](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExbXdxdWo5dnRwYmtycGU3YTNnMDZ4aGtmZGg4YjR3N3l2ZDF3eHVyMyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/t7sEnf5w7wJ1CEPyy7/giphy.gif){: width="150" }

I imported the data as a CSV to my Postgres SQL database.

I then started to learn the Django ORM, which is still (imo) the best part of the Django library.

The basic Model/View/Controller (MVC) was pretty easy to understand, and I had a basic version of the website up and running relatively quickly.

![trinistocks screenshot](/assets/posts/2025-03-27-trinistocks/trinistocks_screenshot.jpg)

From there, I started to think about access. Most people that I knew used their phone to access the web. Few of them used their laptop browser.

I therefore started to look at mobile development frameworks. Flutter emerged as a really strong contender for cross-platform development.

![Flutter icon](/assets/posts/2025-03-27-trinistocks/flutter_icon.jpg){: width="150" }


I also needed to open up an API to allow my flutter app to fetch the data that it needed. Luckily, developing a REST framework in Django was pretty easy.

I ended up adding a ton of functionality from there: A portfolio tracking tool, to keep track of how much my shares were worth. A simulator game, so that you could test your mettle without spending real money. etc.

However, the largest headache was always scraping the latest data from the source. I ended up using a PDF scraping library to fetch the data from some other local stockbrokers.

Here's a diagram of the architecture:
![Image of the trinistocks architecture](/assets/posts/2025-03-27-trinistocks/trinistocks_diagram.png)

I eventually retired the website and the app, to focus more on other projects. The code for both is available publicly on my Github profile below.

All in all, it was a really great learning experience! 🎉

[trinistocks main repo](https://github.com/rishi-latchmepersad/trinistocks.com)

[trinistocks flutter repo](https://github.com/rishi-latchmepersad/trinistocks_flutter)