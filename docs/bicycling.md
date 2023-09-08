---
layout: page
title: Bicycling Around The World
permalink: /bicycling/
comments: true
---

![eljas-cycles]({{site.baseurl}}/assets/e-cycle-crop.jpg)

I have been bicycling for my whole life.
To cycle 100km (62mi) at one sitting felt magical to me.
Almost impossible.
But I did it the first time in 2014.
Not long after that did I do my first overnight bicycling tour in Southern Finland.
That started a series of events that led to me bicycling around the world.

- 2014: Helsinki-Lahti (100km/62mi)
- 2015: Helsinki-Lahti-Evo-Lahti (200km/120mi)
- 2017: Helsinki-Porvoo-Virojoki-Lappeenranta (250km/180mi)
- 2019: Lappeenranta-Koli (330km/205mi)

In 2019 I bicycled abroad for the first time in my life: I went from Estonia to Latvia.
That gave me the confidence that I am capable of bicycling abroad, getting food and sleeping outside.

So naturally in 2020, just before the pandemic hit, I bought flights to New Zealand and decided to bicycle around the south island.
My plan: Christchurch-Hokitika-Queenstown-Christchurch.
I only made it to Wanaka before the country went to lockdown.
Bicycling around 650km/400mi in two weeks.
That was the first time I went that far from home.
To be exact it'd be difficult to be farther away from Finland than that.
It was also my first time to fly with a bicycle.
I proved myself anything is possible.

2021: I bicycled from Montpellier, France to Italy along the Riviera.
Then headed to the mountains and went past the Alps.
I finished in Zurich, Switzerland totaling at about 1100km/680mi in one month.
That tour changed my world.
I got hungry for more.
I decided I have to do this more often and I need to bicycle around the world for good.

2022: Helsinki-Cluj-Napoca, Romania.
Via Estonia, Latvia, Lithuania, Poland, Slovakia and Hungary.
Totaling roughly 1300km/800 in one month since we used the train at times.

2023: Bicycling in the Nordics: Kolari-Abisko-Narvik-Alta-Hetta.
Finland/Sweden/Norway.
1100km/680mi in two weeks.

---

I have already done some cool tours in 15 countries on two continents.
They have teached me a lot about myself, the world and my gear.
As a European citizen it's easy for me to travel inside EU.
However, I don't want to be "locked" here forever.
It's too easy and comfortable.
So my next trip will be in Africa.
I will post about that later if I come back alive.

---



<div class="home">
  {% if site.paginate %}
    {% assign posts = paginator.posts %}
  {% else %}
    {% assign posts = site.posts %}
  {% endif %}

  {%- if posts.size > 0 -%}
    {%- if page.list_title -%}
      <h2 class="post-list-heading">{{ page.list_title }}</h2>
    {%- endif -%}
    <ul class="post-list">
      {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
      {%- for post in posts -%}
        {%- if post.tags contains 'bicycling' -%}
        <li>
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            <h3>
            <a class="post-link" href="{{ post.url | relative_url }}">
                {{ post.title | escape }}
            </a>
            </h3>
            {%- if site.show_excerpts -%}
            {{ post.excerpt }}
            {%- endif -%}
        </li>
        {%- endif -%}
      {%- endfor -%}
    </ul>

    {% if site.paginate %}
      <div class="pager">
        <ul class="pagination">
        {%- if paginator.previous_page %}
          <li><a href="{{ paginator.previous_page_path | relative_url }}" class="previous-page">{{ paginator.previous_page }}</a></li>
        {%- else %}
          <li><div class="pager-edge">•</div></li>
        {%- endif %}
          <li><div class="current-page">{{ paginator.page }}</div></li>
        {%- if paginator.next_page %}
          <li><a href="{{ paginator.next_page_path | relative_url }}" class="next-page">{{ paginator.next_page }}</a></li>
        {%- else %}
          <li><div class="pager-edge">•</div></li>
        {%- endif %}
        </ul>
      </div>
    {%- endif %}

  {%- endif -%}

</div>


















<!-- 
Bicycling around the world. I can't think of any better way to use my limited lifetime. To explore the hidden gems of the world, learn about new cultures, challenge myself and feel the freedom.

I started bicycling at the age of three or four and I never stopped. I started exploring Finland but then I started doing trips abroad in the Baltics, Mediterranean, East Europe and even New Zealand!

I am dreaming of cycling around the world but I still don't know how. My biggest pain points:

- How to fund the trip?
- Do I have to "ruin" my career in programming by quitting my awesome and well-paying job?
- How to plan the route?
  - [RateMyRoute and ZenCycling apps](https://hyrtsi.github.io/bicycling-route-planner-app)
  - [Manually](https://hyrtsi.github.io/bicycling-route-planning)
- Is my bicycle and my gear good enough for the trip?
- What happens to my social relationships during the tour?
- Do I want to do it all in one go or split the trip into separate parts and come back to my home in the meantime?
- When to do it?
- Which time of the year do I want to spend in each country? Do I want to try to survive extreme temperatures of -40 or +40 degrees celsius or heavy downpour, floods and landslips during the monsoon?

As you can see I have quite many unknows even though I have bicycled probably tens of thousands of kilometers in Finland and a couple of thousand of kms abroad. But I don't let them stop me. I know that I already have what it takes for the trip. Planning can only make it better. But it is already possible.

I will collect here resources that are useful for me and hopefully are useful to you.

# Index

- [Other people who have cycled the world](https://hyrtsi.github.io/bicycling-study)
- [Next level path planning for long bicycle tours around the world](https://hyrtsi.github.io/bicycling-route-planner-app)
  - [Visas](https://www.visahq.com/)
  - Weather
  - Security
  - Road condition
  - [Other criteria](https://hyrtsi.github.io/bicycling-route-planning)
- Building the perfect bicycle for a long tour
- What to take with you
- Funding
- Psychology

# Big picture

I love to travel the world. I love bicycling, too. On my last trip from Finland to Romania I got two ideas: a route planning application for bicyclists and my own trip around the world. I had earlier thought that I would be able to do my RTW trip in smaller pieces but now I got frustrated. I have about 4 or 5 weeks of paid holiday each year. I can apply for an unspecified amount of unpaid holiday, too. I can travel 1000km, maybe 2000km max during that time. It doesn't sound too appealing.

- It takes a while to go from bicycle mental mode to working and living in a city and vice versa so long trips are preferred
- Flying is almost certainly required to travel from Finland to anywhere unless I have lots of more time
- It's frustrating to disassemble and package the bicycle for flying
- Many short trips is more expensive than few long trips
- I don't need much money on the tour, maybe 5-10 euros a day on average
- I don't want to travel the same routes every time

so I must try to travel for longer periods of time. At least two months, preferably more at a time. Since most of our planet is water the route must be split to parts and boats, ferries and airplanes must be used. This also means that each one of these journeys can be done separately and I can return back home in between them.

The fastest person to bicycle around the world is Mark Beaumont. He cycled around the world 29000km in 78 and a half days. The slowest person to cycle around the world is Heinz Stucke, who cycled around the world for 50 years. My trip will be something in between probably. Those men have set the boundaries for me.

I have decided that I must maximize the fun, safety and meaning in my trip. I don't have to follow the Guiness World Records rules even though it would be awesome to get my name on the same page with the big boys and girls.

Things that motivate me:
- Travelling countries that none of my friends even know about
- Meeting local people and other cyclists
- Adventuring
- Having a big goal
- Visiting many countries
- Seeing beautiful places

Things that do not motivate me:
- Ugly places
- Crowded and busy streets
- Landmines, war, protests
- Pollution
- Corrupt police
- Trash and broken glass on the streets
- Natural disasters
- Wild animals that try to kill me

It is inevitable that the list of countries I can visit is really low if I want to minimize all risks. Consider [all the horrible things that happened to Heinz Stuck](https://en.wikipedia.org/wiki/Heinz_St%C3%BCcke#Global_bicycle_tour) on his journey. But that doesn't make me want to stay at home. He did survive it so I will survive, too.

 -->
