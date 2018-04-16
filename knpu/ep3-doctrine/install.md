# Install

Coming soon...

Hey guys, welcome back to the third episode in our starting in symphony series. Our project is already already had some really cool stuff, but there's one super important thing. We're missing a database, so it's time to bring in a database and actually become total masters on how to Querie save and do everything else that you need to do and actually symphony itself doesn't have a database. It relies on a really high quality external project called doctrine, but don't worry. The integration is really good and doctrine is incredibly powerful. It does have a reputation of being a little bit hard to learn, but that's. That's also gotten a lot better over the last few years, so as always 

to get the most out of your database, you should totally code along with me. Download the course code from this page. When you unzip it, you'll find a start directory that will have the same code that you see here. Open the read me. Diary the file for details on how to get set up. The last step will be to open a terminal, move into the project and run Bin Console Server Colin. Run to start the built in web server, then spin back to your browser and go to local host on [inaudible] to find the space bar are inter galactic news site, which has some cool stuff, but it's still just a bunch of hard coated articles. Time to change that, so since doctrine is an external library, step one is to install it, which of course thanks to symphony flex is super easy. Open a new tab in your terminal and just run composer require doctrine. This will download a pack which will contain a few libraries, including doctrine in also a migrations library to help us manage database changes on production. More on that soon when it finishes, it gives us a really nice little message. Since we're going to be talking to a database, obviously we're going to need to configure that database somewhere and it tells us and of course that's done in the.in file. It tells us to modify the database url there. So move over to your code. Open that end and yes, he doctrine bundle added a new database. You were out that we need to configure, set up yours. I'll use, I use route with no password locally and I'll call it the space note. The space bar. 

Yeah, 

of course. This sets a database url environment variable that's used in a new config packages doctrine dot yammel file that was installed by the about the recipe. If you scroll down a little bit here, this database you will is used to configure doctrine. 

Yeah, 

they're actually a lot of options in here, but you probably don't need to change any of them. These are just really good defaults like their defaults to make sure that you get utf eight tables. There are defaults down here that guarantee that make your database tables and column names underscored versions. 

Yeah, 

of course. If you want to use something other than my sql, you can configure that and you should set your server version to the server version of my sql that you're using on production. Alright, with doctrine configured and we can actually already create our database, go back to your terminal and run bin Console doctrine database and create an yes. Say hello to our new database. It's empty of course, but it's ready to go. Now doctrine is a is n o r, m, which basically means that every table in the database is going to have a corresponding class in our coat, so if we want to create an article table, it means that we need to create an article class. You can create this class as my hand. There's also a really good generation tool that comes from maker bundle. We install maker bundle in the last tutorial and before we started coding I updated it to the latest version so that it has this command run bin Console, make colon entity. 

That entity word is important. That's the word that doctrine gives to the classes. That model to the database, as you'll see in a second. They're just normal phd classes, but they have this special name. Let's call ours article and then we can start giving it a few fields. We'll need a title field, we'll give it a string type and if you hit question mark, you can see what all the different types are. These aren't my sql types, these are doctrine types, which are an abstraction and they mapped to different Maya sql types behind the scenes. So let's use a string, we'll let it be 2:55 length and we want this to not be allowed to be no and the database. So we'll say no to. Next we're gonna create a slug, which we'll use the url string field again, let's make this be 100 field length and then also not knowing the database. And then let's add content. This will be a text field and then let, let's say yes to this being knowledge database so that we can create articles and write content later. And then finally, let's add a published at field. Let this be daytime and allow this to be in all the database because it will be no, unless you've actually published it. Then once you're finished adding fields, hit enter and don't worry if you make any mistakes so you can always update things later. OK, so what did that just do? The only thing that just did 

was in the SEC directory, inside a new entity directory. We now have an article class will actually full disclosure. There's also a new article repository class, but I want you to ignore that for now. It's not important yet. The this article class is your entity and you check it out. It's just a normal php class with properties for all of our fields, so id titles, slug content in, published at the magic is the annotations that are above them. So the orm entity is what tells doctor that this is an entity class that should be mapped to the database and then above each column we have a bunch of annotations that help doctrine configure that and this is totally yours to change. Actually, if you go back to Google for doctrine, annotations, reference annotation, reference, if you want to know the options and things you can put on here. It has a great 

yeah 

article here which goes through all the different annotations in the all of the different annotation options and at the bottom of this class, since these are private properties, there's just a bunch of getter and setter methods. The important thing is this is totally your class change. We generated it to be for simplicity, but we own this class. So yeah, so we have our article entity and this is completely ready to go, but well, of course we don't have an article table yet in the database, so how do we create that? Actually this is one of doctrines superpowers. Go back to your terminal and actually you can see red on the bottom. It suggests that we run the make migration command to create a database migration. This is actually really cool, or I'm been console, make colon migration. It says it. It created a new source migrations version file that we should review this. Let's move over here. Open the migrations directory and yeah, there it is. One migration file with the Maya sql code needed. Create that article with those columns. So this is really cool that make migration command actually looked at our database, looked at all of our entities, which is just one entity right now and fig and generated the sql code necessary to update our database to match our entity. 

Since this looks good to me, let's close it and then to actually execute the migration, we'll run bin Console Doctrine, Colin Migrations calling migrate. You can actually see that command suggested right above. This will ask you if you want to to run the migrations and we'll click yes and it executes that migration. 

Yeah, 

if you run it again, it does nothing. Run Doctrine, migrations status. This tells you a little bit more about how the migration systems work inside of our database. This just created a new table called migration underscore versions that we run on migration. It's stores that in that table so that it knows it's already executed it, so this is really cool because it means whenever we make a database change, we just need to generate a migration, run that migration. Then we'll commit the migrations to our pository, redeploy this doctrine. Migrations Migrate Command will be part of our deploy, so production will have its own migration versions table and it will execute whatever migrations haven't been run on production. In fact, let's do one more little change. So if you look back in your article class, we have this slug field. This is going to be used inside of our as a unique identifier and so like this string here, so this actually needs to be unique across all the articles in the, in the database, so make that happen. We can add a new option here called unique, equal to true. Now the results of that is that it. 

What that does is it tells doctrine that it should create this column in the database or the unique constraints on it, but of course if we make the change here, the database doesn't automatically update. We need to generate a migration for just that change, so no problem. Flip over and once again we'll do bin Console, make migration. 

You can even see if you misspell some sometimes talking about it runs into any ways. This generates a second migration, so there's our first migration that creates the table. The second migration. Check this out. All it does is create that unique index so it looks at our database, it looks at our entity, and it realized this was the only thing that was missing. So now let's run bin Console doctrine called migrations to migrate. The system is smart enough to only run that new migration and now it knows that that's the status. All right, so we have our database set up, we have our articles, our entity set up. We have a great migration system set up. So let's talk about saving articles to the database.