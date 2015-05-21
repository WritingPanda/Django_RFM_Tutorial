## Django Rest Framework tutorial

The tutorial can be found [here](http://www.django-rest-framework.org/tutorial/1-serialization/).

I am on [step 4 of the tutorial](http://www.django-rest-framework.org/tutorial/4-authentication-and-permissions/), and I will be updating the README file each time to move to another step.

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

- Learned and worked with both the Request and Response objects
- Used the Status codes built into the framework
- Used the api\_view decorator instead of the csrf\_exempt
- Refactored views.py to use the @api\_view decorator along with status codes, response, and request
- Added the optional suffix to the URLs
- Tested it out using httpie and a browser

#### Tutorial 3

- Rewrote API with using class based views
- Initially began with a more verbose manner of writing classes:
    from snippets.models import Snippet
    from snippets.serializers import SnippetSerializer
    from django.http import Http404
    from rest_framework.views import APIView
    from rest_framework.response import Response
    from rest_framework import status

    class SnippetList(APIView):
        """
        List all snippets, or create a new snippet.
        """
        def get(self, request, format=None):
            snippets = Snippet.objects.all()
            serializer = SnippetSerializer(snippets, many=True)
            return Response(serializer.data)

        def post(self, request, format=None):
            serializer = SnippetSerializer(data=request.data)
            if serializer.is_valid():
                serializer.save()
                return Response(serializer.data, status=status.HTTP_201_CREATED)
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
- Changed the urls.py file to match with the class-based views
- Refactored to use mixins to decrease amount of code written and to uphold the DRY method
- At the end, refactored again to use generic class-based views, as shown in the final version of this recent commit:
    from snippets.models import Snippet
    from snippets.serializers import SnippetSerializer
    from rest_framework import generics


    class SnippetList(generics.ListCreateAPIView):
        queryset = Snippet.objects.all()
        serializer_class = SnippetSerializer


    class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
        queryset = Snippet.objects.all()
        serializer_class = SnippetSerializer

#### Tutorial 4
- Working on it now