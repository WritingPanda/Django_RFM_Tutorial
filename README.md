## Django Rest Framework tutorial

The tutorial can be found [here](http://www.django-rest-framework.org/tutorial/1-serialization/).

I am on [step 6 of the tutorial](http://www.django-rest-framework.org/tutorial/6-viewsets-and-routers/), and I will be updating the README file each time to move to another step.

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

```
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
```

- Changed the urls.py file to match with the class-based views
- Refactored to use mixins to decrease amount of code written and to uphold the DRY method
- At the end, refactored again to use generic class-based views, as shown in the final version of this recent commit:

```
    from snippets.models import Snippet
    from snippets.serializers import SnippetSerializer
    from rest_framework import generics


    class SnippetList(generics.ListCreateAPIView):
        queryset = Snippet.objects.all()
        serializer_class = SnippetSerializer


    class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
        queryset = Snippet.objects.all()
        serializer_class = SnippetSerializer
```

#### Tutorial 4

- Worked with permissions on class-based views
- Code snippets are now always associated with a creator
- Only authenticated users can create snippets
- Creator may update or delete code snippet
- Unauthenticated requests should have full read-only access
- Added User classes along with views to see users
- Created UserSerializer to serialize data to JSON for viewing in browser
- Worked with object level permissions:

```
    class IsOwnerOrReadOnly(permissions.BasePermission):
    """
    Custom permission to only allow owners of an object to edit it.
    """

    def has_object_permission(self, request, view, obj):
        # Read permissions are allowed to any request,
        # so we'll always allow GET, HEAD or OPTIONS requests.
        if request.method in permissions.SAFE_METHODS:
            return True

        # Write permissions are only allowed to the owner of the snippet.
        return obj.owner == request.user
```


#### Tutorial 5

- Created relationships with hyperlinks instead of primary keys
- Removed pk from model and put url instead
- This allows me to customize what snippets I want linked to what
- I now have an HTML representation of the code snippets, which looks great
- Added names to URL patterns
- Added pagination

#### Tutorial 6

- Working on it now