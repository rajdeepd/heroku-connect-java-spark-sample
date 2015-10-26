# Heroku Connect with Java Spark Framework etting-started

This sample shows how to run 

## Running Locally

Make sure you have Java and Maven installed.  Also, install the [Heroku Toolbelt](https://toolbelt.heroku.com/).

```sh
$ git clone https://github.com/heroku/java-getting-started.git
$ cd java-getting-started
$ mvn install
$ foreman start web
```

Your app should now be running on [localhost:5000](http://localhost:5000/).

If you're going to use a database, ensure you have a local `.env` file that reads something like this:

```
DATABASE_URL=postgres://localhost:5432/java_database_name
```

## Deploying to Heroku

```sh
$ heroku create
$ git push heroku master
$ heroku open
```

## Adding Heroku Connect Add-On

Configure Heroku Connect Add-On

```sh
$ heroku addons:create herokuconnect
```

## Configure Herok Connect Add-On

* Create a Mapping with Contacts table using Heroku Dashboard

## Code for getting contacts

`/contacts` rest endpoint is tied to getting list of names from `salesforce.contact` and populating `contacts.ftl`.

```java 
 get("/contacts", (req, res) -> {
        Connection connection = null;
        Map<String, Object> attributes = new HashMap<>();
        try {
          connection = DatabaseUrl.extract().getConnection();
          Statement stmt = connection.createStatement();

          ResultSet rs = stmt.executeQuery("SELECT name FROM salesforce.contact");

          ArrayList<String> output = new ArrayList<String>();
          while (rs.next()) {
            output.add( rs.getString("name"));
          }
          attributes.put("results", output);
          return new ModelAndView(attributes, "contacts.ftl");
        } catch (Exception e) {
          attributes.put("message", "There was an error: " + e);
          return new ModelAndView(attributes, "error.ftl");
        } finally {
          if (connection != null) try{connection.close();} catch(SQLException e){}
        }
      }, new FreeMarkerEngine());
```
## Show Contacts
 Browse to URL `http://{your-app-name}.herokuapp.com/contacts` to see the list of contact names.
