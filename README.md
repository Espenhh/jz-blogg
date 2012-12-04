# JavaZone Blog

Innstruksjoner til hvordan publisere innhold på JavaZone-bloggen.

## Oppsett

Klon først dette repositoryet.

    git clone git@github.com:Espenhh/jz-blogg.git

Så må vi legge bloggen til som en `git remote`.

    cd jz-blogg
    git remote add test javabin@javazone.espenhh.com:/home/javabin/git/jz-blog/test
    git remote add prod javabin@javazone.espenhh.com:/home/javabin/git/jz-blog/prod
	
For å kunne se hvordan ting ser ut lokalt må også `jekyll` installeres.

    gem install jekyll

## Skriv bloggposter

For å skrive en ny bloggpost, lag en ny fil i mappen `jz-blogg/_posts`.
Denne må ha navnet `yyyy-mm-dd-navn-på-posten.md`, der `yyyy-mm-dd` er datoen posten er skrevet, og `navn-på-posten` er det som skal brukes som postens URL.

Innholdet i posten struktureres på følgende måte:

    ---
    layout: post
    categories: 
        - kategorier
        - posten
        - hører
        - hjemme
        - i
        - her
    title: Postens kule tittel
    published: true
    ---

    Her skrives innholdet i bloggposten. Dette formateres som 
    [markdown](http://daringfireball.net/projects/markdown/)

For å se hvordan resultatet blir underveis, start jekyll:

	jekyll --server --auto

Du kan nå browse bloggen lokalt på [localhost:4000](http://localhost:4000) i din foretrukne nettleser.

## Publisér

For å publisere innholdet til bloggen, commit og push bloggposten.

    git add _posts/2012-12-12-min-kule-bloggpost.md
    git commit -m "skrev en kul bloggpost"

    # Til test.blog.javazone.no:
	git push test

    # Til blog.javazone.no:
    git push prod

For å publisere må du har ssh-tilgang. Hvis du ikke har dét, snakk med noen i javaBin som kan ordne.