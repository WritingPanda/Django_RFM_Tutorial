## Django Rest Framework tutorial

The tutorial can be found [here](http://www.django-rest-framework.org/tutorial/1-serialization/).

I am on [step 2 of the tutorial](http://www.django-rest-framework.org/tutorial/2-requests-and-responses/), and I will be updating the README file each time to move to another step.

### What I have done so far:

#### Tutorial 1

- I have created models and serialized models
- Initially, the tutorial had me creating a model in the "Django" way in order to make initial migrations
- Afterward, I made a Serializer class the long way in order to introduce myself to the format that the Serializer works with
- After doing it the long way, I worked in the shell to update a few items to the DB via the python shell
- I took data, serialized it in JSON, and then deserialized it back in a python dictionary
- The tutorial had me refactor the initial SnippetSerializer to take advantage of a shortcut using the Django model
- Looking through the shell, I was able to see how my class was generated
- Next, I wrote a Django view using the Serializer
- I tested it out using the shell and httpie

#### Tutorial 2

- Working on it now